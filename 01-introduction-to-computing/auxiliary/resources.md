# Resources for Basics of Computers & Computing

## Understanding Computers
- [Computer Basics - Khan Academy](https://www.khanacademy.org/computing/computer-science/computers-and-internet)
- [What is a Computer? - BBC Bitesize](https://www.bbc.co.uk/bitesize/articles/z9myvcw)

## Hardware Components
- [Computer Hardware Components Explained - Computer Hope](https://www.computerhope.com/jargon/h/hardware.htm)
- [Understanding Computer Hardware - Lifewire](https://www.lifewire.com/computer-hardware-2625895)
- [Inside a Computer - Intel](https://www.intel.com/content/www/us/en/education/lesson-plans/inside-a-computer.html)

## Software and Operating Systems
- [Operating Systems - CrashCourse Computer Science](https://www.youtube.com/watch?v=26QPDBe-NB8)
- [Software Basics - Techopedia](https://www.techopedia.com/definition/4356/software)
- [Windows vs Linux: What's the Difference? - How-To Geek](https://www.howtogeek.com/windows-vs-linux-whats-the-difference-for-everyday-users/)

## Data Basics
- [What is Data? - Simplilearn](https://www.simplilearn.com/what-is-data-article)
- [Binary Numbers Explained - BBC Bitesize](https://www.bbc.co.uk/bitesize/articles/z9j2jsg#zhxsn9q)
- [Structured vs Unstructured Data - IBM Cloud](https://www.ibm.com/think/topics/structured-vs-unstructured-data)






# NovaCore Data Platform — Architectural Guide

## Purpose  
NovaCore is a lightweight, **configuration‑driven** data platform designed around three guiding principles:

1. **Separation of Concerns** – Configuration, processing logic, and I/O are strictly isolated.  
2. **System‑Agnostic Core** – All data‑processing code can run anywhere Python runs, with zero assumptions about storage or compute.  
3. **KISS I/O Layer** – Each concrete reader/writer does _one_ thing well and exposes the same minimal interface, so they can be hot‑swapped by simply changing configs.

---

## Repository Layout

| Path | Responsibility | Key Points |
|------|----------------|------------|
| **`nova/configManager/`** | Parse & validate **lineage**, **transformation**, and **catalog** configs. | Central source of truth for what, how, and where to move data. |
| **`nova/core/`** | Pure processing logic. No cloud SDKs, no DB connectors. | Orchestrates transformations using abstract I/O interfaces. |
| **`nova/core/io/`** | All concrete **I/O** implementations & their factories. | Keeps external systems out of `core`. |
| `nova/core/utils/` | Generic helpers shared by processing modules. | No side effects. |
| `nova/core/data_processor.py` | High‑level pipeline driver. | Glues configs ⇢ I/O ⇢ processing. |
| `README.md` | _You are here._ | Living architecture doc. |

---

### 1. `configManager/`

```
configManager/
├── __init__.py
├── processing/
│   └── __init__.py
├── project/
│   └── __init__.py
└── security/
    └── __init__.py
```

* **`processing/`** – Rules for chaining transformations (e.g., DAG definitions, dependency graphs).  
* **`project/`** – Project‑level knobs (naming conventions, SLA windows, alert hooks).  
* **`security/`** – Source‑level ACLs & PII tagging used downstream by writers.  

> **Contract:** A loader in `configManager.__init__` exposes `load_config(section: str) -> dict`.

---

### 2. `core/`

```
core/
├── __init__.py
├── data_processor.py
├── io/
│   └── …
└── utils/
    └── __init__.py
```

* **Stateless transformation modules** live here (row‑level maps, aggregations, data‑quality checks).  
* They accept/return **Pandas or PySpark DataFrames** only – never touch files or sockets.  
* `data_processor.py` orchestrates:  
  1. Pull config via `configManager`.  
  2. Ask an **I/O controller** for matching readers/writers.  
  3. Execute the transform chain.  
  4. Commit lineage + push metrics.

---

### 3. `core/io/` – Pluggable I/O Layer

```
io/
├── interfaces/
│   ├── __init__.py
│   └── input.py           # BaseIO, Reader, Writer ABCs
├── controller/
│   ├── __init__.py
│   ├── reader.py          # ReaderController
│   └── writer.py          # WriterController
├── adls/
│   ├── __init__.py
│   ├── reader.py          # Δ tables, parquet, csv
│   └── writer.py
└── unityCatalog/
    ├── __init__.py
    ├── reader.py          # Databricks Unity Catalog tables
    └── writer.py
```

#### Interfaces  
```python
class Reader(ABC):
    def read(self, *, table: str, **opts) -> DataFrame: ...

class Writer(ABC):
    def write(self, df: DataFrame, *, table: str, mode: str = "append", **opts) -> None: ...
```

#### Controllers  
Think of them as **constructor proxies**.  
```python
reader = ReaderController.from_config(src_cfg)   # returns AdlsReader or UnityCatalogReader
writer = WriterController.from_config(dst_cfg)   # returns matching writer
```
*They provide a single decision point, keeping the rest of the codebase blissfully unaware of storage details.*

#### Concrete Adapters
* **`adls.*`** – Speaks directly to Azure Data Lake Storage (Delta & Parquet).  
* **`unityCatalog.*`** – Wraps Databricks SQL API for managed tables.  
* New systems (e.g., S3, Snowflake) plug in by dropping a `{system}/reader.py` & `{system}/writer.py` and registering them in the controller map.

---

## Data‑Flow Walk‑Through

1. **Config Load** – `configManager` ingests YAML/JSON, validates schema, resolves environment vars.  
2. **I/O Binding** – `ReaderController` & `WriterController` pick the correct adapters.  
3. **Processing** – `core.data_processor` applies the declared transformations in memory.  
4. **Write‑back & Lineage** – Results persisted by the writer; lineage emitted to catalog.  

![](docs/diagrams/high_level_flow.png) <!-- optional future diagram -->

---

## Extending the Platform

| Task | Hook Point | Steps |
|------|------------|-------|
| **Add new storage type** | `core/io/{your_system}` | 1. Implement `reader.py` & `writer.py` subclasses<br>2. Register in `controller/__init__.py`. |
| **Add new transform** | `core/` | 1. Write pure function / class.<br>2. Reference it in the processing config. |
| **Add new config section** | `configManager/` | 1. Define schema.<br>2. Update `load_config` switch. |

---

## Design Rationale & Trade‑offs

* **Why Controllers?** Keeps factory logic in one place, so swapping storage back‑ends never ripples outward.  
* **Why ABC Interfaces?** Guarantees that every reader/writer supports the same verbs, enabling generic orchestration.  
* **Why “core is ignorant”?** Improves testability and portability—unit tests run on a laptop with in‑mem DataFrames.  

