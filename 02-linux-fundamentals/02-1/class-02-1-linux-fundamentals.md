# Class 02-1: Linux Fundamentals - Introduction and Basic Operations

## Overview
In this first part of Linux fundamentals, students will understand what Linux is, its advantages, and get comfortable navigating and performing basic tasks on a Linux system. By the end of this session, you will have a solid foundation in Linux that will aid you in your technical journey.

## Learning Objectives
- Understand what Linux is and why it's popular in technical environments.
- Navigate the Linux file system effectively.
- Learn essential Linux commands with practical examples.
- Manage files and directories confidently.
- Set up and log into a Linux server using SSH.

## Content

### What is Linux?
Linux is a family of open-source Unix-like operating systems based on the Linux kernel. Imagine Linux as the operating system's equivalent of a public park; it's open for everyone to use and contribute to, making it a vibrant ecosystem. It is widely used for servers, cloud infrastructures, and development environments due to its stability, security, and flexibility. 

### Advantages of Linux
- **Open-source and free**: Like a community garden, anyone can contribute to and improve Linux.
- **High security and stability**: Linux is known for being less susceptible to viruses and malware compared to other operating systems.
- **Powerful command-line interface (CLI)**: The CLI allows users to execute complex operations with simple commands, similar to how a chef can create a gourmet meal with just a few high-quality ingredients.
- **Highly customizable**: Users can modify and tailor their Linux experience to fit their needs, much like customizing a car.

### Linux File System Structure
Understanding the Linux file system is crucial for effective navigation. Think of the file system as a filing cabinet:

- **`/`**: The root directory, akin to the top drawer of the filing cabinet. Everything in Linux starts from here.
- **`/home`**: User directories, similar to individual folders in the drawer where each user keeps their personal files.
- **`/etc`**: Configuration files, like the instruction manuals for the filing cabinet, detailing how everything should operate.
- **`/var`**: Variable files like logs, comparable to a logbook that records all activities and changes made within the cabinet.

#### More on Linux File System Structure:
- **`/usr`**: Contains user programs and utilities. It’s like the toolbox where all the tools for use are stored.
- **`/bin`**: Essential user binaries (programs) that are required for system booting and repairing, similar to the most-used tools in the toolbox.
- **`/lib`**: Contains shared libraries needed by programs, akin to the reference books that provide information to the tools.

### Essential Linux Commands
Here are some essential Linux commands with detailed explanations and examples:

#### Navigation Commands
- **`pwd`**: Print Working Directory. This command shows you the current directory you are in. 
  - **Example**: If you type `pwd`, and it returns `/home/user`, you are currently in the user’s home directory. 
  - **Usage Scenario**: This command is particularly useful when you are deep in the directory structure and need to confirm your location before proceeding with other commands.

- **`cd`**: Change Directory. Use this command to move between directories.
  - **Example**: To navigate to the `Documents` folder within your home directory, type `cd Documents`. To go back to the previous directory, use `cd ..`.
  - **In-Depth Walkthrough**: 
    1. Start in your home directory by typing `pwd` to confirm your location.
    2. Type `cd Documents` and press Enter. 
    3. Type `pwd` again to see that you are now in `/home/user/Documents`.
    4. To go back, type `cd ..` and confirm your previous directory with `pwd`.

- **`ls`**: List files and directories in the current directory.
  - **Example**: Typing `ls` will show all files and folders in your current directory. Use `ls -l` for a detailed view, which includes file permissions, ownership, size, and modification date.
  - **Troubleshooting Common Issues**: If you don’t see expected files, ensure you are in the correct directory using `pwd`.

#### File Management Commands
- **`cp`**: Copy files and directories.
  - **Example**: To copy a file named `file.txt` to a directory called `backup`, use `cp file.txt backup/`.
  - **Detailed Walkthrough**:
    1. First, ensure the `backup` directory exists using `ls`.
    2. Type `cp file.txt backup/` and press Enter.
    3. Check the contents of `backup` using `ls backup/` to confirm the file is copied.

- **`mv`**: Move or rename files and directories.
  - **Example**: To move `file.txt` into the `Documents` folder, type `mv file.txt Documents/`. To rename it, you can use `mv file.txt newfile.txt`.
  - **Usage Scenario**: This command is useful for organizing files. Suppose you have a cluttered home directory; moving files into specific folders can help maintain order.

- **`rm`**: Remove files or directories. 
  - **Example**: To delete `file.txt`, type `rm file.txt`. Be cautious, as this action is irreversible.
  - **In-Depth Analogy**: Think of `rm` like throwing something away. Once you toss it in the trash, it’s gone for good unless you retrieve it from the bin before it’s emptied.

- **`mkdir`**: Make directories.
  - **Example**: To create a new directory called `Projects`, type `mkdir Projects`.
  - **Detailed Walkthrough**: 
    1. Use `ls` to see your current directories.
    2. Type `mkdir Projects` and press Enter.
    3. Confirm the creation by typing `ls` again to see `Projects` listed.

- **`touch`**: Create a new empty file or update the timestamp of an existing file.
  - **Example**: To create a new file named `notes.txt`, use `touch notes.txt`.
  - **Usage Scenario**: This command is handy when you want to create a placeholder for a file that you plan to fill in later.

#### Viewing Files
- **`cat`**: Concatenate and display file content.
  - **Example**: To view the content of `notes.txt`, type `cat notes.txt`.
  - **In-Depth Walkthrough**: 
    1. Ensure `notes.txt` exists using `ls`.
    2. Type `cat notes.txt` to display its contents on the terminal.

- **`less`**: View file content one screen at a time, allowing for easier reading.
  - **Example**: To view `longfile.txt`, type `less longfile.txt` and use the arrow keys to scroll.
  - **Usage Scenario**: This command is particularly useful for log files or long documents where scrolling through all content at once can be overwhelming.

- **`tail`**: View the last part of a file.
  - **Example**: To see the last 10 lines of `logfile.log`, use `tail logfile.log`.
  - **Usage Scenario**: This is especially useful for monitoring log files in real-time to quickly check the latest entries.

- **`head`**: View the beginning of a file.
  - **Example**: To see the first 10 lines of `file.txt`, type `head file.txt`.
  - **In-Depth Analogy**: Think of `head` as the cover of a book; it gives you a preview without revealing the entire story.

#### System Information Commands
- **`uname`**: Display system information.
  - **Example**: Typing `uname -a` will show detailed information about your system.
  - **Usage Scenario**: This command is useful when troubleshooting compatibility issues or checking system specifications.

- **`top`**: Display running processes and system resource usage in real-time.
  - **Example**: Simply typing `top` will show you which processes are consuming the most resources.
  - **In-Depth Walkthrough**: 
    1. Type `top` and press Enter.
    2. Observe the list of processes and their resource usage.
    3. Press `q` to exit the top command.

- **`df`**: Report file system disk space usage.
  - **Example**: To see how much disk space is available, type `df -h`, where `-h` makes the output human-readable.
  - **Usage Scenario**: This command is particularly useful before installing new software to ensure enough disk space is available.

- **`free`**: Display memory usage.
  - **Example**: Typing `free -h` shows the total, used, and free memory in a human-readable format.
  - **In-Depth Analogy**: Think of your computer’s memory like a workspace; knowing how much space you have helps you manage your tasks effectively.

### Setting Up and Logging into a Linux Server using SSH

#### Initial Setup
1. **Obtain Server Credentials**: Ensure you have the server's IP address and SSH credentials (username and password or key file).

2. **Open Terminal**:
   - On macOS or Linux, open your terminal application.
   - On Windows, you can use Command Prompt, PowerShell, or Windows Terminal.

3. **Connect via SSH**:
   - Use the following command to connect to your server:
     ```
     ssh username@server_ip
     ```
   - Replace `username` with your actual username and `server_ip` with the server's IP address.
   - If you are using a private key, the command will be:
     ```
     ssh -i /path/to/private_key username@server_ip
     ```

4. **Accept the Host Key**: The first time you connect, you'll see a message asking if you want to continue connecting. Type `yes` and press Enter.

5. **Enter Your Password**: If prompted, enter your password. You won’t see the characters as you type for security reasons.

#### Connecting from Visual Studio Code
1. **Install Remote Development Extension**: 
   - Open Visual Studio Code.
   - Go to Extensions (Ctrl+Shift+X) and search for "Remote - SSH".
   - Install the extension.

2. **Open the Command Palette**: Press `F1` or `Ctrl+Shift+P`.

3. **Connect to Host**:
   - Type `Remote-SSH: Connect to Host...` and select it.
   - If it's your first time, you may need to configure your SSH settings. Follow the prompts to add your server's IP, username, and private key if applicable.

4. **Enter Your Password**: If prompted, enter your password.

5. **Open Folder**: Once connected, you can open folders on the remote server and start working on files directly.

#### Troubleshooting Common SSH Connection Issues
- **Connection Timed Out**: Ensure that the server is running and that the IP address is correct. Check firewall settings on the server.
- **Permission Denied**: Verify that you are using the correct username and password. If using a key, ensure the key has the correct permissions (e.g., `chmod 600 /path/to/private_key`).
- **Host Key Verification Failed**: If you see this error, it may be due to a change in the server’s host key. Remove the old key from `~/.ssh/known_hosts` and try connecting again.

#### Tips for Configuring Visual Studio Code for Remote Development via SSH
- **Use SSH Config File**: Create or edit the `~/.ssh/config` file to simplify your SSH commands. Add entries like:
  ```
  Host myserver
      HostName server_ip
      User username
      IdentityFile /path/to/private_key
  ```
  You can then connect using `ssh myserver`.

- **Set Up Extensions for Remote Development**: Consider installing additional extensions for languages and tools you plan to use on the remote server to enhance your development experience.
