# Linux Fundamentals: Class 02-3

## Advanced File Permissions

In Linux, file permissions are a crucial aspect of system security and user management. Each file and directory has associated permissions that dictate who can read, write, or execute them. Understanding these permissions is vital for maintaining a secure and efficient environment.

### Understanding File Permissions

Permissions are represented by a series of characters. For example, `-rwxr-xr--` can be broken down as follows:

- The first character indicates the type of file: `-` for a regular file, `d` for a directory.
- The next three characters (`rwx`) represent the owner's permissions: read (r), write (w), and execute (x).
- The following three characters (`r-x`) represent the group's permissions.
- The last three characters (`r--`) represent permissions for others.

### Changing Permissions with `chmod`

The `chmod` command is used to change file permissions. You can use either symbolic (letters) or numeric (octal) representation.

**Example:**

To give execute permission to the owner of a file named `script.sh`:
```bash
chmod u+x script.sh
```

Using numeric representation:
```bash
chmod 755 script.sh
```

### Real-World Analogy

Think of file permissions like keys to a house. The owner has all the keys (read, write, execute), while guests may only have a key to enter but not to modify anything.

## User Management

Managing users in Linux is essential for controlling access and maintaining system integrity. This involves creating, modifying, and deleting user accounts.

### Adding a User

To add a new user, you can use the `useradd` command:

```bash
sudo useradd -m newuser
```

This command creates a new user and a home directory for them.

### Modifying User Information

To change a user's information, you can use the `usermod` command:

```bash
sudo usermod -aG sudo newuser
```

This command adds `newuser` to the `sudo` group, giving them administrative privileges.

### Deleting a User

To remove a user, use the `userdel` command:

```bash
sudo userdel -r newuser
```

The `-r` option also removes the user's home directory.

## Installing Software Packages

Linux distributions use package managers to install, update, and remove software. The two most common package managers are `apt` for Debian-based systems and `yum` for Red Hat-based systems.

### Installing Software with `apt`

To install a package using `apt`, you can use the following command:

```bash
sudo apt update
sudo apt install package-name
```

### Example: Installing `curl`

To install `curl`, a command-line tool for transferring data with URLs:

```bash
sudo apt update
sudo apt install curl
```

### Removing Software Packages

To remove a package, use the `remove` command:

```bash
sudo apt remove package-name
```

## Process Management

Understanding how to manage processes is essential for any Linux user. Processes are instances of running applications, and Linux provides several commands to manage them.

### Viewing Running Processes

To view currently running processes, you can use the `ps` command:

```bash
ps aux
```

This command lists all running processes along with their details.

### Killing a Process

If a process becomes unresponsive, you can terminate it using the `kill` command. First, find the process ID (PID) using `ps`, then:

```bash
kill PID
```

To forcefully kill a process, use:

```bash
kill -9 PID
```

## Introductory Scripting (Basic Bash Scripting)

Bash scripting allows you to automate tasks in Linux. A script is simply a file containing a series of commands.

### Creating a Simple Script

1. Create a new file:
   ```bash
   nano myscript.sh
   ```

2. Add the shebang line at the top:
   ```bash
   #!/bin/bash
   ```

3. Write your commands below:
   ```bash
   echo "Hello, World!"
   ```

4. Save and exit.

### Making the Script Executable

Before running the script, you need to make it executable:

```bash
chmod +x myscript.sh
```

### Running the Script

To execute the script, simply run:

```bash
./myscript.sh
```

### Example: A Simple Backup Script

Here’s a simple script that backs up a directory:

```bash
#!/bin/bash
tar -czf backup.tar.gz /path/to/directory
echo "Backup completed!"
```

## Conclusion

In this class, we covered advanced file permissions, user management, software installation, process management, and an introduction to basic Bash scripting. Mastering these concepts will greatly enhance your proficiency in using Linux systems.
