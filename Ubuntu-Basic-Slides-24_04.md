# Ubuntu Server Basic Training

**Ubuntu 24.04 LTS (Noble Numbat)**

---

## Table of Contents

1. [What is Ubuntu](#1-what-is-ubuntu)
   - [1.1 What is Linux?](#11-what-is-linux)
   - [1.2 What is the Kernel?](#12-what-is-the-kernel)
   - [1.3 What is Ubuntu?](#13-what-is-ubuntu)
   - [1.4 The Release Cycle](#14-the-release-cycle)
   - [1.5 Ubuntu Lab](#15-ubuntu-lab)

2. [CLI Environment](#2-cli-environment)
   - [2.1 Secure Shell](#21-secure-shell)
   - [2.2 Getting Help](#22-getting-help)
   - [2.3 Shell Environment](#23-shell-environment)
   - [2.4 Shell Prompts](#24-shell-prompts)
   - [2.5 Path](#25-path)
   - [2.6 Command Line Editing Keys](#26-command-line-editing-keys)
   - [2.7 CLI Lab](#27-cli-lab)

3. [Basic File Operations](#3-basic-file-operations)
   - [3.1 Navigating the Filesystem](#31-navigating-the-filesystem)
   - [3.2 File Operations](#32-file-operations)
   - [3.3 Viewing File Contents](#33-viewing-file-contents)
   - [3.4 Wildcards and Pattern Matching](#34-wildcards-and-pattern-matching)
   - [3.5 File Operations Lab](#35-file-operations-lab)

4. [Text Editing with nano](#4-text-editing-with-nano)
   - [4.1 nano Text Editor](#41-nano-text-editor)
   - [4.2 nano Keyboard Shortcuts](#42-nano-keyboard-shortcuts)
   - [4.3 nano Lab](#43-nano-lab)

5. [Understanding File Permissions](#5-understanding-file-permissions)
   - [5.1 File Permissions Basics](#51-file-permissions-basics)
   - [5.2 Changing Permissions with chmod](#52-changing-permissions-with-chmod)
   - [5.3 Changing Ownership with chown](#53-changing-ownership-with-chown)
   - [5.4 Permissions Lab](#54-permissions-lab)

6. [Finding Files and Text](#6-finding-files-and-text)
   - [6.1 Finding Files with find](#61-finding-files-with-find)
   - [6.2 Searching Text with grep](#62-searching-text-with-grep)
   - [6.3 Combining Commands with Pipes](#63-combining-commands-with-pipes)
   - [6.4 Other Useful Search Commands](#64-other-useful-search-commands)
   - [6.5 Find and Grep Lab](#65-find-and-grep-lab)

7. [Process Management](#7-process-management)
   - [7.1 Viewing Processes](#71-viewing-processes)
   - [7.2 Process Control](#72-process-control)
   - [7.3 Monitoring System Resources](#73-monitoring-system-resources)
   - [7.4 Process Priorities](#74-process-priorities)
   - [7.5 Process Management Lab](#75-process-management-lab)

8. [Redirection and Special Characters](#8-redirection-and-special-characters)
   - [8.1 Standard Streams](#81-standard-streams)
   - [8.2 Output Redirection](#82-output-redirection)
   - [8.3 Input Redirection](#83-input-redirection)
   - [8.4 Pipes](#84-pipes)
   - [8.5 Special Characters](#85-special-characters)
   - [8.6 Redirection Lab](#86-redirection-lab)

9. [Archiving and Compression](#9-archiving-and-compression)
   - [9.1 tar Command](#91-tar-command)
   - [9.2 Compression Tools](#92-compression-tools)
   - [9.3 zip and unzip](#93-zip-and-unzip)
   - [9.4 Archive and Compression Lab](#94-archive-and-compression-lab)

10. [Package Management](#10-package-management)
    - [10.1 APT Package Manager](#101-apt-package-manager)
    - [10.2 Basic APT Commands](#102-basic-apt-commands)
    - [10.3 dpkg Command](#103-dpkg-command)
    - [10.4 Snap Packages](#104-snap-packages)
    - [10.5 Holding Packages](#105-holding-packages)
    - [10.6 Repository Management](#106-repository-management)
    - [10.7 Package Management Lab](#107-package-management-lab)

[Appendix: Networking](#appendix-networking)

---

# 1. What is Ubuntu

**Description:**

In this section you will learn to:
* Differentiate between the flavours of Ubuntu Linux
* Explain the Ubuntu Linux release cycle
* Explain the Ubuntu naming convention
* Explain Ubuntu LTS maintenance cycle


Environment connection:

1. Please use SSH to connect to the public IP you've 
received via mail, credentials should also be provided.

2. After you connect to the `STUDENT HOST` machine, do another
SSH connection to the `LAB HOST`, the password is `ubuntu`:

```bash
# password is ubuntu
ssh ubuntu@192.168.100.3
```

3. Here you will run the commands for the next chapters.


## 1.1 What is Linux?

Linux was created as a clone of MINIX, a Unix-like system
intended for academic use. What began as a program to
use specific hardware independent of an operating system
evolved into the Linux kernel. It was created and released in
1991 by Linus Torvalds. Torvalds created Linux as a free,
and later to become open-source, alternative to MINIX in
response to the restrictions on modification and
redistribution of MINIX and other UNIX-like operating
systems.

More commonly though, Linux is thought of as an operating
system (OS). In this role, Linux is a set of programs, tools,
and services that are typically bundled with the Linux kernel
to provide all of the necessary components of an operating
system.

Linux is the world's largest and most ubiquitous open
source software project in history. The Linux Foundation
fosters Linux kernel development so it can be protected and
accelerated for years to come. Linux is used throughout
computing on everything from embedded systems to
supercomputers. For example:

* 95% of domains use Linux as the operating system
* 80% of smartphones run Android, which is based on
the Linux kernel
* 98% of the 500 fastest supercomputers in the world
run on Linux
* 75% of cloud-enabled enterprises use Linux as the
primary cloud platform
* NYSE, NASDAQ, London Exchange, Tokyo Stock
Exchange, and more run on Linux
* The majority of consumer electronic devices use
Linux because of its small footprint
* Amazon, Ebay, Paypal, Walmart, and others use Linux

Linux is also the operating system of choice for the Internet
of Things (IoT), cloud computing, and big data.


## 1.2 What is the Kernel?


The kernel is the core of the operating system. It is the
software that functions as the interface between the
computer hardware and the user programs. You can find
more information at:

<https://kernel.org/>


The Linux kernel controls the CPU, memory, inter-process
communication, device drivers, filesystem management,
and system server calls. If a program needs information
from memory or another process, the kernel can quickly get
it since it manages the hardware and multi tasking. The
kernel also makes it easier for processes to communicate
with each other.

The Linux kernel is monolithic. A monolithic kernel is an OS
architecture where all device drivers, file system(s), and
application inter-process communication works in kernel
space. This means that the kernel alone defines the highlevel
virtual interface over computer hardware. A set of primitives 
or system calls implement all OS services such as process 
management, concurrency, and memory management. Device drivers 
can be added to the kernel as modules.

Linux supports true preemptive multi tasking, virtual
memory, shared libraries, demand loading, shared 
copy-on-write executables, memory management, the Internet
protocol suite, and threading.

The kernel is written in C and assembly language. It is
developed by contributors worldwide. Development
discussion can be viewed on the Linux Kernel Mailing List
(LKML).

Within the operating system, the kernel runs in kernel mode
and user processes run in user mode. Code running in
kernel mode has unrestricted access to the processor and
main memory. Most of the software with which you'll
interact runs in user space and interfaces with the kernel to
accomplish tasks.


## 1.3 What is Ubuntu?

Ubuntu is a Linux distribution based upon the Debian Linux
distribution that runs on platforms from smartphones to the
cloud.

Canonical supports and provides Ubuntu images for server,
desktop, and many public cloud providers. We also provide
images for phones, tablets, and core for IoT devices.

Ubuntu names are based on a numerical model with a two-digit 
year and a two-digit month of release. For example,
Ubuntu 24.04 was released in April of 2024 and Ubuntu
23.10 was released in October of 2023. Ubuntu also
receives a code name in the form `Adjective Animal`.
Ubuntu 24.04 is called `Noble Numbat` and Ubuntu 23.10 is
`Mantic Minotaur`.


## 1.4 The Release Cycle

Ubuntu updates are released every six months. Every fourth
release (2 years) is designated as long-term support (LTS).
LTS versions focus on stability and provide newer kernel
support with Hardware enablement (HWE). Non-LTS
releases provide the latest and greatest enhancements.
Regular releases are supported for nine months and LTS
releases are supported for five years with Extended Security 
Maintenance (ESM) available for up to ten years total. Older 
releases may have had different support lengths.

For more information go to:

<https://www.ubuntu.com/>


## 1.5 Ubuntu Lab


1. Go to the Ubuntu.com site and find the latest
Server download. What is the version?


# 2. CLI Environment

**Description:**

In this section you will learn to:
* Define the shell
* Use help features
* Use various CLI commands to work within the shell environment
* Use nano to edit files


**What is a shell?**

The shell is an environment provided for user interaction
with the underlying operating system. It is a command
language interpreter. When you type a command, the shell
receives it and sends it to the operating system, which
executes it.

On modern Linux systems, there are many shells from
which to choose. On Ubuntu Server, only bash and dash are
installed by default. The default shell is bash. System
scripts declare which shell they are using.
* `bash` - Bourne Again Shell
* `dash` - Debian Almquist Shell

The user's shell is defined in the `/etc/passwd` file.

The command line interface (CLI) is independent of distros.
The CLI gives a user more control and options. Commands
are entered in a terminal window. The terminal window is a
terminal emulator which lets you interact with the shell . In
the window, you should see a shell prompt that contains
your user name and the name of the machine followed by a
dollar sign.

**What are commands?**

There are different types of commands and they are used
throughout this course.

`Executable program` - a compiled or scripted program
that executes a series of commands

`Built-in shell commands` - commands that are part of
the shell such as `cd`

`Shell functions` - miniature shell scripts incorporated
into the environment.

`An alias` - a user-defined command built from other
commands.

You can use the mouse in the terminal window for such
tasks as copying text. To copy text, press the left button and
drag your mouse to highlight the text. Release the left
button, move your mouse pointer to the terminal window
and press the middle mouse button (or click the right button
and select paste). The text you highlighted in the browser
window should be copied into the command line.


## 2.1 Secure Shell

Secure Shell (SSH) enables you to connect to another
computer on the Internet that could be physically located
anywhere in the world via a window on your local machine.
With `ssh`, you can run programs interactively on the remote
machine.

`SSH` is a protocol used to securely log onto remote systems.
It is the most common way to access remote Linux servers.
It provides secure access to the remote computer by
encrypting the session and facili tating authentication. With
SSH, a user can manage multiple computers from one PC.

To log in to a remote host for which the username is the
same as the local computer use:

```
ssh remote_host
```

If the username on the remote computer is not the same,
the following command would be used (user is the account
on the remote computer):

```
ssh user@remote_host
```

When first connecting to a server you will be asked to accept
the authenticity of the remote host. You can type yes or y
and hit return. Once a machine is in the list of known hosts,
you will not be asked again.

The syntax to run a command on a remote system would be:
```
ssh user@remote_host command_to_run
```

To close an ssh connection, you can use the command:
```
exit
```


## 2.2 Getting Help

An important skill to have when working in the command
line is knowing how to find help. The man pages are often
the best source of information for a particular command.
`Man` is short for manual and the man pages contain
information like an encyclopedia.

Below is the basic format of a man page entry:
```
man command_name
```

To navigate through a man page:
* Up and Down arrows to move up and down a line
* Space-bar to move down a page
* Type `/` to start a search and type the word to
search. Hit `n` to find the next instance and `N` to go
back to a previous one.
* Type `q` to quit.

Commands are also covered in the `info` documentation.

To exit info pages, hit `q` for quit.

The whatis command displays a brief description of a
command:

```
whatis ls
```

Most commands also include a help option:

```
ls --help
```


## 2.3 Shell Environment

When a terminal is started, bash reads configuration files
which build and/or set the environment. There are startup
files on a system-wide or per-user basis.

Some of the configuration files:

**`/etc/profile`**  
This is the master file. It loads first for all users.

**`~/.bash_profile`**  
This is for the login user only. Each user can have his or her
own bash_profile in his or her home directory. This file
overrides settings in the /etc/profile.

**`~/.bashrc`**  
This file is read immediately following your successful login.

**`~/.bash_logout`**  
This file is read when the user logs out of the shell (terminal
window). You can edit this file and add commands like
`clear` so that the terminal window is cleared when you log
out.

You can run the source command to re-read a configuration
file and apply changes without logging out and logging back
in:
```
source ~/.bashrc
```


## 2.4 Shell Prompts

The default command line prompt for a regular user is a
dollar sign `$`. The prompt for the root/superuser is a
pound `#` sign. These characters are chosen so you can
tell if you are logged in as root or a regular user.

You will often see the dollar sign used on the Internet to
show examples that can be run by regular users and the
pound sign for examples that need to be run by the root
user.

The command line prompt is defined in the `PS1` variable.
This variable can be customized (Google bash colors). To
see the value of the PS1 variable:
```
echo $PS1
```

To change it:
```
PS1="$ "
```

You can make the change permanent by adding the setting
to the `.bashrc` file in your home directory.


## 2.5 Path

The PATH environment variable is a colon-delimited list of
directories that the shell searches through when you type a
command. You only need to use the command name rather
than the complete path to the executable. To see your
current path, echo the PATH variable:
```
echo $PATH
```

If you want to add another directory to the path, there are
two options:
1. Add to the path for this session only:
```
PATH=$PATH:/bin
```
2. Make it permanent by adding the command to your
`.bashrc` file in your home directory:
```
echo 'PATH=$PATH:/bin' >> ~/.bashrc
```


## 2.6 Command Line Editing Keys

Below is a list of commonly used keystrokes for editing the
command line:

| Keystroke | Description |
|-----------|-------------|
| Ctrl-A | Move cursor to the beginning of the line |
| Ctrl-E | Move cursor to the end of the line |
| Ctrl-K | Delete from the cursor location to end of line |
| Ctrl-U | Delete from the cursor location to beginning of line |
| Ctrl-L | Clear the screen |
| Ctrl-C | Cancel the running command/clear current line |
| Ctrl-R | Search command history |
| Up arrow | Previous command in history |
| Down arrow | Next command in history |
| !! | Repeat last command |
| Tab | Command/file/directory completion |


## 2.7 CLI Lab

1. Connect to your environment via SSH:
```bash
ssh student@<ip-address>
```

2. Check which shell you're using:
```bash
echo $SHELL
```

3. View your command history:
```bash
history
```

4. Use Ctrl-R to search your history for a command containing "ls"

5. View the manual page for the ls command:
```bash
man ls
```

6. Use whatis to get a brief description:
```bash
whatis ls
```

7. Check your current PATH:
```bash
echo $PATH
```

8. View your current prompt setting:
```bash
echo $PS1
```

9. Create a simple alias:
```bash
alias ll='ls -la'
ll
```

10. Make the alias permanent by adding it to your .bashrc file:
```bash
echo "alias ll='ls -la'" >> ~/.bashrc
source ~/.bashrc
```


# 3. Basic File Operations

**Description:**

In this section you will learn to:
* Navigate the filesystem
* List directory contents
* Copy, move, and delete files
* Create and remove directories
* Use wildcards and pattern matching


## 3.1 Navigating the Filesystem

The Linux filesystem is organized as a tree structure with the
root directory (/) at the top. Everything in Linux is a file or a
directory.

Key navigation commands:

`pwd` - Print working directory (shows your current location)
```bash
pwd
```

`cd` - Change directory
```bash
cd /etc          # Go to /etc directory
cd ..            # Go up one directory
cd ~             # Go to your home directory
cd -             # Go to previous directory
```

`ls` - List directory contents
```bash
ls               # List files in current directory
ls -l            # Long format with details
ls -la           # Include hidden files (starting with .)
ls -lh           # Human-readable file sizes
ls -lt           # Sort by modification time
ls -ltr          # Sort by time, reverse order
```


## 3.2 File Operations

`cp` - Copy files or directories
```bash
cp file1 file2              # Copy file1 to file2
cp -r dir1 dir2             # Copy directory recursively
cp -p file1 file2           # Preserve permissions and timestamps
```

`mv` - Move or rename files
```bash
mv file1 file2              # Rename file1 to file2
mv file1 /path/to/dir/      # Move file1 to directory
```

`rm` - Remove files or directories
```bash
rm file1                    # Remove file1
rm -r dir1                  # Remove directory recursively
rm -f file1                 # Force removal without prompt
rm -rf dir1                 # Force remove directory (use carefully!)
```

`mkdir` - Create directories
```bash
mkdir newdir                # Create directory
mkdir -p dir1/dir2/dir3     # Create nested directories
```

`rmdir` - Remove empty directories
```bash
rmdir emptydir              # Remove empty directory
```

`touch` - Create empty file or update timestamp
```bash
touch newfile               # Create empty file
touch existingfile          # Update timestamp
```


## 3.3 Viewing File Contents

`cat` - Display entire file contents
```bash
cat file1                   # Display file contents
cat file1 file2             # Display multiple files
```

`less` - View file contents one page at a time
```bash
less file1                  # Navigate with arrows, q to quit
```

`head` - Display first lines of file
```bash
head file1                  # Show first 10 lines
head -n 20 file1            # Show first 20 lines
```

`tail` - Display last lines of file
```bash
tail file1                  # Show last 10 lines
tail -n 20 file1            # Show last 20 lines
tail -f logfile             # Follow file in real-time
```


## 3.4 Wildcards and Pattern Matching

Wildcards help you match multiple files:

`*` - Matches any characters
```bash
ls *.txt                    # All files ending in .txt
rm file*                    # All files starting with file
```

`?` - Matches single character
```bash
ls file?.txt                # file1.txt, file2.txt, etc.
```

`[]` - Matches characters in brackets
```bash
ls file[123].txt            # file1.txt, file2.txt, file3.txt
ls file[a-z].txt            # filea.txt through filez.txt
```

`{}` - Matches patterns
```bash
cp file.{txt,doc} backup/   # Copy both file.txt and file.doc
```


## 3.5 File Operations Lab

1. Check your current directory:
```bash
pwd
```

2. List all files including hidden ones:
```bash
ls -la
```

3. Create a test directory structure:
```bash
mkdir -p testdir/subdir1/subdir2
```

4. Navigate to the new directory:
```bash
cd testdir
pwd
```

5. Create some test files:
```bash
touch file1.txt file2.txt file3.doc
```

6. Copy a file:
```bash
cp file1.txt file1_backup.txt
```

7. List files to verify:
```bash
ls -l
```

8. Move a file to subdirectory:
```bash
mv file3.doc subdir1/
```

9. View the directory structure:
```bash
ls -lR
```

10. Use wildcards to list only .txt files:
```bash
ls *.txt
```

11. Create multiple files using brace expansion:
```bash
touch test{1..5}.txt
ls test*.txt
```

12. Remove test files:
```bash
rm test*.txt
```

13. Return to home directory:
```bash
cd ~
```

14. Remove test directory:
```bash
rm -rf testdir
```


# 4. Text Editing with nano

**Description:**

In this section you will learn to:
* Open and edit files with nano
* Save and exit nano
* Use basic nano features


## 4.1 nano Text Editor

`nano` is a simple, user-friendly text editor that's perfect for
beginners. It displays helpful keyboard shortcuts at the
bottom of the screen.

To open or create a file with nano:
```bash
nano filename
```

To open with sudo for system files:
```bash
sudo nano /etc/hosts
```


## 4.2 nano Keyboard Shortcuts

Key shortcuts in nano (^ means Ctrl):

| Shortcut | Description |
|----------|-------------|
| Ctrl-O | Save file (WriteOut) |
| Ctrl-X | Exit nano |
| Ctrl-K | Cut current line |
| Ctrl-U | Paste cut text |
| Ctrl-W | Search for text |
| Ctrl-\ | Search and replace |
| Ctrl-G | Display help |
| Ctrl-C | Show cursor position |
| Ctrl-_ | Go to line number |
| Alt-U | Undo last action |
| Alt-E | Redo last undone action |


## 4.3 nano Lab

1. Create a new file with nano:
```bash
nano myfile.txt
```

2. Type some text:
```
This is my first file.
Ubuntu 24.04 Noble Numbat.
Testing the nano editor.
```

3. Save the file:
   - Press Ctrl-O
   - Press Enter to confirm filename

4. Search for text:
   - Press Ctrl-W
   - Type "Ubuntu"
   - Press Enter

5. Cut and paste:
   - Move to a line
   - Press Ctrl-K to cut
   - Move to new location
   - Press Ctrl-U to paste

6. Exit nano:
   - Press Ctrl-X

7. Edit a system file (view only):
```bash
nano /etc/os-release
```
View the Ubuntu version information, then exit without saving (Ctrl-X, then N)

8. Edit with sudo:
```bash
sudo nano /etc/hosts
```
Add a comment line:
```
# Test comment
```
Save and exit (Ctrl-O, Enter, Ctrl-X)


# 5. Understanding File Permissions

**Description:**

In this section you will learn to:
* Understand Linux file permissions
* Use chmod to modify permissions
* Use chown to change ownership
* Understand special permissions


## 5.1 File Permissions Basics

Every file in Linux has three types of permissions for three
categories of users:

Permission types:
* `r` (read) - View file contents or list directory contents
* `w` (write) - Modify file or directory contents
* `x` (execute) - Run file as program or enter directory

User categories:
* `u` (user/owner) - The file owner
* `g` (group) - Users in the file's group
* `o` (others) - All other users

View permissions with ls -l:
```bash
ls -l filename
```

Output format:
```
-rwxr-xr-x 1 user group 1234 Jan 01 12:00 filename
 |  |  |
 |  |  └─ Others permissions (r-x)
 |  └──── Group permissions (r-x)
 └─────── Owner permissions (rwx)
```

The first character indicates file type:
* `-` = regular file
* `d` = directory
* `l` = symbolic link


## 5.2 Changing Permissions with chmod

Symbolic method:
```bash
chmod u+x file          # Add execute for owner
chmod g-w file          # Remove write from group
chmod o+r file          # Add read for others
chmod a+x file          # Add execute for all
chmod u=rwx,g=rx,o=r file  # Set specific permissions
```

Numeric (octal) method:
```bash
chmod 755 file          # rwxr-xr-x
chmod 644 file          # rw-r--r--
chmod 700 file          # rwx------
chmod 777 file          # rwxrwxrwx (not recommended!)
```

Octal values:
* 4 = read (r)
* 2 = write (w)
* 1 = execute (x)

Add values for combined permissions:
* 7 = rwx (4+2+1)
* 6 = rw- (4+2)
* 5 = r-x (4+1)
* 4 = r-- (4)


## 5.3 Changing Ownership with chown

Change file owner:
```bash
sudo chown newowner file
```

Change owner and group:
```bash
sudo chown newowner:newgroup file
```

Change group only:
```bash
sudo chown :newgroup file
```

Recursive change:
```bash
sudo chown -R user:group directory/
```


## 5.4 Permissions Lab

1. Create a test file:
```bash
touch testfile.sh
ls -l testfile.sh
```

2. Add execute permission:
```bash
chmod +x testfile.sh
ls -l testfile.sh
```

3. Set specific permissions using octal:
```bash
chmod 755 testfile.sh
ls -l testfile.sh
```

4. Create a test directory:
```bash
mkdir testdir
ls -ld testdir
```

5. Set directory permissions:
```bash
chmod 755 testdir
ls -ld testdir
```

6. View your user and group:
```bash
id
```

7. Change file ownership (if you have permissions):
```bash
sudo chown $USER:$USER testfile.sh
ls -l testfile.sh
```

8. Remove test files:
```bash
rm testfile.sh
rmdir testdir
```


# 6. Finding Files and Text

**Description:**

In this section you will learn to:
* Use find to locate files
* Use grep to search text
* Combine commands with pipes


## 6.1 Finding Files with find

Basic find syntax:
```bash
find /path -name filename
```

Common find examples:
```bash
# Find by name
find /home -name "*.txt"

# Find by type
find /etc -type f        # Files only
find /etc -type d        # Directories only

# Find by size
find /var -size +100M    # Larger than 100MB
find /tmp -size -1M      # Smaller than 1MB

# Find by modification time
find /home -mtime -7     # Modified in last 7 days
find /var/log -mtime +30 # Modified more than 30 days ago

# Find and execute command
find /tmp -name "*.tmp" -delete
find /home -name "*.log" -exec ls -lh {} \;

# Find by permissions
find /home -perm 777     # Exactly 777
find /var -perm /u+s     # SUID files
```


## 6.2 Searching Text with grep

Basic grep syntax:
```bash
grep "pattern" filename
```

Common grep options:
```bash
# Case-insensitive search
grep -i "error" logfile

# Recursive search in directories
grep -r "TODO" /home/user/code/

# Show line numbers
grep -n "error" logfile

# Invert match (show non-matching lines)
grep -v "debug" logfile

# Count matches
grep -c "warning" logfile

# Show only filenames with matches
grep -l "error" *.log

# Show context (lines before/after match)
grep -A 3 "error" logfile    # 3 lines after
grep -B 3 "error" logfile    # 3 lines before
grep -C 3 "error" logfile    # 3 lines before and after

# Multiple patterns
grep -E "error|warning|critical" logfile

# Whole word match
grep -w "test" file          # Matches "test" but not "testing"
```


## 6.3 Combining Commands with Pipes

Pipes ( | ) connect commands, using output of one as input to another:

```bash
# Find large files and sort by size
find /var -type f -size +10M -exec ls -lh {} \; | sort -k5 -h

# Search for processes and filter
ps aux | grep apache

# Count occurrences
grep "error" logfile | wc -l

# Unique sorted list
cat file.txt | sort | uniq

# Display first few results
ls -l /etc | head -20

# Chain multiple commands
ps aux | grep python | grep -v grep | awk '{print $2}'
```


## 6.4 Other Useful Search Commands

`locate` - Fast file search using database:
```bash
locate filename
sudo updatedb            # Update locate database
```

`which` - Find command location:
```bash
which python3
which ls
```

`whereis` - Find binary, source, and man page:
```bash
whereis nginx
```


## 6.5 Find and Grep Lab

1. Find all .conf files in /etc:
```bash
find /etc -name "*.conf" 2>/dev/null | head -10
```

2. Find files modified in the last day:
```bash
find /var/log -mtime -1 -type f 2>/dev/null
```

3. Search for "ubuntu" in /etc/os-release:
```bash
grep -i "ubuntu" /etc/os-release
```

4. Search recursively for "error" in system logs:
```bash
sudo grep -r "error" /var/log/ 2>/dev/null | head -10
```

5. Count lines containing "systemd" in syslog:
```bash
sudo grep -c "systemd" /var/log/syslog
```

6. Find and list large files in /var:
```bash
sudo find /var -type f -size +100M 2>/dev/null
```

7. Search with context:
```bash
grep -A 2 -B 2 "VERSION=" /etc/os-release
```

8. Find all directories named "log":
```bash
sudo find / -type d -name "log" 2>/dev/null | head -5
```

9. Use pipes to find and count:
```bash
ls -la /etc | grep "\.conf" | wc -l
```

10. Find python processes:
```bash
ps aux | grep python | grep -v grep
```


# 7. Process Management

**Description:**

In this section you will learn to:
* View running processes
* Control processes
* Monitor system resources
* Understand process priorities


## 7.1 Viewing Processes

`ps` - Display process information:
```bash
ps                  # Current shell processes
ps aux              # All processes, detailed
ps -ef              # All processes, full format
ps -u username      # Processes by user
```

`top` - Interactive process viewer:
```bash
top
```
Top keyboard shortcuts:
* Press `q` to quit
* Press `k` to kill a process
* Press `M` to sort by memory
* Press `P` to sort by CPU
* Press `h` for help

`htop` - Enhanced process viewer (install if not available):
```bash
sudo apt install htop
htop
```


## 7.2 Process Control

Running processes:
```bash
# Background process
command &

# Foreground to background
Ctrl-Z              # Suspend
bg                  # Resume in background

# Background to foreground
fg                  # Bring to foreground
fg %1               # Bring job 1 to foreground

# List background jobs
jobs
```

Killing processes:
```bash
# By PID
kill <PID>
kill -9 <PID>       # Force kill
kill -15 <PID>      # Graceful termination (default)

# By name
killall processname
pkill processname

# Kill all user processes
pkill -u username
```


## 7.3 Monitoring System Resources

`free` - Display memory usage:
```bash
free -h             # Human-readable
```

`df` - Display disk space:
```bash
df -h               # Human-readable
df -i               # Show inodes
```

`du` - Display directory space usage:
```bash
du -sh directory/   # Summary in human-readable format
du -h --max-depth=1 # One level deep
```

`uptime` - System uptime and load:
```bash
uptime
```

`vmstat` - Virtual memory statistics:
```bash
vmstat 1 5          # Update every 1 sec, 5 times
```

`iostat` - I/O statistics:
```bash
sudo apt install sysstat
iostat -x 1 5
```


## 7.4 Process Priorities

View priority:
```bash
ps -el              # Show priority in PRI column
```

Change priority with `nice`:
```bash
nice -n 10 command  # Start with lower priority (+10)
nice -n -10 command # Start with higher priority (-10, needs sudo)
```

Change priority of running process with `renice`:
```bash
renice -n 5 -p <PID>
sudo renice -n -5 -p <PID>
```

Priority values range from -20 (highest) to 19 (lowest).
Default is 0.


## 7.5 Process Management Lab

1. View all running processes:
```bash
ps aux | head -20
```

2. View processes in tree format:
```bash
ps aux --forest | less
```

3. Start top and explore:
```bash
top
```
Press P to sort by CPU, M for memory, q to quit

4. Find a specific process:
```bash
ps aux | grep sshd
```

5. Check system memory:
```bash
free -h
```

6. Check disk space:
```bash
df -h
```

7. Find largest directories in /var:
```bash
sudo du -h --max-depth=1 /var | sort -h
```

8. Start a background process:
```bash
sleep 100 &
```

9. List background jobs:
```bash
jobs
```

10. View the sleep process:
```bash
ps aux | grep sleep | grep -v grep
```

11. Kill the process by PID:
```bash
kill %1
# or
pkill sleep
```

12. Check system uptime and load:
```bash
uptime
```


# 8. Redirection and Special Characters

**Description:**

In this section you will learn to:
* Redirect input and output
* Use special characters effectively
* Handle standard streams


## 8.1 Standard Streams

Every process has three standard streams:
* `stdin` (0) - Standard input
* `stdout` (1) - Standard output
* `stderr` (2) - Standard error


## 8.2 Output Redirection

Redirect standard output:
```bash
command > file              # Overwrite file
command >> file             # Append to file
command 1> file             # Explicit stdout (same as >)
```

Redirect standard error:
```bash
command 2> error.log        # Redirect errors to file
command 2>> error.log       # Append errors to file
```

Redirect both stdout and stderr:
```bash
command > output.log 2>&1   # Traditional method
command &> output.log       # Shorter method
command &>> output.log      # Append both
```

Discard output:
```bash
command > /dev/null         # Discard stdout
command 2> /dev/null        # Discard stderr
command &> /dev/null        # Discard both
```


## 8.3 Input Redirection

Redirect input from file:
```bash
command < inputfile
```

Here document:
```bash
command << EOF
multiple lines
of input
EOF
```

Here string:
```bash
command <<< "single line input"
```


## 8.4 Pipes

Connect commands:
```bash
command1 | command2         # Stdout of cmd1 → stdin of cmd2
command1 |& command2        # Stdout and stderr → cmd2
```

Tee command (write to file and stdout):
```bash
command | tee file.txt      # Write to file and display
command | tee -a file.txt   # Append to file and display
```


## 8.5 Special Characters

| Character | Description | Example |
|-----------|-------------|---------|
| * | Wildcard (any characters) | ls *.txt |
| ? | Wildcard (single character) | ls file?.txt |
| [] | Character class | ls file[12].txt |
| {} | Brace expansion | touch file{1..5}.txt |
| ~ | Home directory | cd ~/Documents |
| . | Current directory | ./script.sh |
| .. | Parent directory | cd .. |
| / | Directory separator | /etc/hosts |
| \ | Escape character | echo \"Hello\" |
| \| | Pipe | ls \| grep txt |
| > | Output redirection | echo "test" > file |
| >> | Append redirection | echo "test" >> file |
| < | Input redirection | command < input |
| & | Background execution | command & |
| && | AND (run if previous succeeds) | make && make install |
| \|\| | OR (run if previous fails) | command \|\| echo "failed" |
| ; | Command separator | cd /tmp; ls |
| $ | Variable prefix | echo $HOME |
| ' | Single quote (literal) | echo 'Price: $5' |
| " | Double quote (interpret vars) | echo "Home: $HOME" |
| \` | Backtick (command substitution) | echo \`date\` |
| $() | Command substitution | echo $(date) |
| # | Comment | # This is a comment |


## 8.6 Redirection Lab

1. Redirect output to file:
```bash
ls -la > listing.txt
cat listing.txt
```

2. Append to file:
```bash
date >> listing.txt
cat listing.txt
```

3. Redirect errors:
```bash
ls /nonexistent 2> error.txt
cat error.txt
```

4. Redirect both stdout and stderr:
```bash
ls /etc /nonexistent &> combined.txt
cat combined.txt
```

5. Discard errors:
```bash
find / -name "*.conf" 2> /dev/null | head -5
```

6. Use here document:
```bash
cat << EOF > message.txt
This is line 1
This is line 2
This is line 3
EOF
cat message.txt
```

7. Use tee to save and display:
```bash
ps aux | head -10 | tee processes.txt
```

8. Chain multiple commands:
```bash
cd /tmp && pwd && ls -la | head
```

9. Command substitution:
```bash
echo "Current date: $(date)"
echo "Current user: $(whoami)"
```

10. Clean up:
```bash
rm listing.txt error.txt combined.txt message.txt processes.txt
```


# 9. Archiving and Compression

**Description:**

In this section you will learn to:
* Create and extract tar archives
* Compress and decompress files
* Understand different compression formats


## 9.1 tar Command

`tar` (Tape ARchive) is used to create and extract archives.

Create archive:
```bash
tar -cvf archive.tar files/      # Create
tar -czvf archive.tar.gz files/  # Create and gzip
tar -cjvf archive.tar.bz2 files/ # Create and bzip2
tar -cJvf archive.tar.xz files/  # Create and xz
```

Extract archive:
```bash
tar -xvf archive.tar             # Extract
tar -xzvf archive.tar.gz         # Extract gzipped
tar -xjvf archive.tar.bz2        # Extract bzip2
tar -xJvf archive.tar.xz         # Extract xz
```

List archive contents:
```bash
tar -tvf archive.tar             # List contents
tar -tzvf archive.tar.gz         # List gzipped archive
```

Extract to specific directory:
```bash
tar -xvf archive.tar -C /path/to/directory/
```

Common tar options:
* `-c` Create archive
* `-x` Extract archive
* `-t` List contents
* `-v` Verbose (show progress)
* `-f` File (specify archive name)
* `-z` gzip compression
* `-j` bzip2 compression
* `-J` xz compression
* `-C` Change directory


## 9.2 Compression Tools

`gzip` - GNU zip:
```bash
gzip file.txt               # Compress (creates file.txt.gz)
gzip -d file.txt.gz         # Decompress
gunzip file.txt.gz          # Decompress (same as gzip -d)
gzip -k file.txt            # Keep original file
```

`bzip2` - Better compression:
```bash
bzip2 file.txt              # Compress (creates file.txt.bz2)
bzip2 -d file.txt.bz2       # Decompress
bunzip2 file.txt.bz2        # Decompress
```

`xz` - Best compression:
```bash
xz file.txt                 # Compress (creates file.txt.xz)
xz -d file.txt.xz           # Decompress
unxz file.txt.xz            # Decompress
```


## 9.3 zip and unzip

`zip` - Create zip archives:
```bash
zip archive.zip file1 file2  # Create zip
zip -r archive.zip directory/ # Recursive zip
zip -e secure.zip file       # Encrypted zip
```

`unzip` - Extract zip archives:
```bash
unzip archive.zip            # Extract
unzip -l archive.zip         # List contents
unzip archive.zip -d /path/  # Extract to path
```


## 9.4 Archive and Compression Lab

1. Create test directory structure:
```bash
mkdir -p testarchive/subdir1/subdir2
echo "File 1" > testarchive/file1.txt
echo "File 2" > testarchive/subdir1/file2.txt
echo "File 3" > testarchive/subdir1/subdir2/file3.txt
```

2. Create tar archive:
```bash
tar -cvf test.tar testarchive/
ls -lh test.tar
```

3. List archive contents:
```bash
tar -tvf test.tar
```

4. Create compressed archives:
```bash
tar -czvf test.tar.gz testarchive/
tar -cjvf test.tar.bz2 testarchive/
tar -cJvf test.tar.xz testarchive/
```

5. Compare file sizes:
```bash
ls -lh test.tar*
```

6. Extract archive:
```bash
mkdir extract_test
tar -xzvf test.tar.gz -C extract_test/
ls -la extract_test/
```

7. Test gzip compression:
```bash
cp testarchive/file1.txt file_to_compress.txt
gzip file_to_compress.txt
ls -lh file_to_compress.txt.gz
gunzip file_to_compress.txt.gz
```

8. Create zip archive:
```bash
zip -r testarchive.zip testarchive/
ls -lh testarchive.zip
```

9. List zip contents:
```bash
unzip -l testarchive.zip
```

10. Extract zip archive:
```bash
mkdir zip_extract
unzip testarchive.zip -d zip_extract/
```

11. Clean up:
```bash
rm -rf testarchive extract_test zip_extract
rm test.tar* testarchive.zip file_to_compress.txt
```


# 10. Package Management

**Description:**

In this section you will learn to:
* Use APT package manager
* Install and remove software
* Update and upgrade the system
* Work with Snap packages


## 10.1 APT Package Manager

APT (Advanced Package Tool) is the primary package
management system for Ubuntu. It handles software
installation, updates, and dependencies.


## 10.2 Basic APT Commands

Update package lists:
```bash
sudo apt update
```

Upgrade installed packages:
```bash
sudo apt upgrade              # Upgrade packages
sudo apt full-upgrade         # Upgrade with dependency changes
```

Install packages:
```bash
sudo apt install package_name
sudo apt install package1 package2 package3
sudo apt install -y package_name    # Auto-answer yes
```

Remove packages:
```bash
sudo apt remove package_name          # Remove package, keep config
sudo apt purge package_name           # Remove package and config
sudo apt autoremove                   # Remove unneeded dependencies
```

Search for packages:
```bash
apt search keyword
apt-cache search keyword
```

Show package information:
```bash
apt show package_name
apt-cache show package_name
```

List installed packages:
```bash
apt list --installed
apt list --upgradable
dpkg -l
dpkg -l | grep package_name
```


## 10.3 dpkg Command

`dpkg` is the low-level package manager:

```bash
dpkg -i package.deb           # Install .deb package
dpkg -r package_name          # Remove package
dpkg -P package_name          # Purge package
dpkg -l                       # List installed packages
dpkg -L package_name          # List package files
dpkg -S /path/to/file         # Find which package owns file
```


## 10.4 Snap Packages

Snaps are universal Linux packages that work across
distributions. They're self-contained and auto-updating.

Install snapd (if not already installed):
```bash
sudo apt install snapd
```

Snap commands:
```bash
snap find package_name        # Search for snaps
snap info package_name        # Show snap information
sudo snap install package_name # Install snap
sudo snap refresh package_name # Update snap
sudo snap refresh             # Update all snaps
sudo snap remove package_name # Remove snap
snap list                     # List installed snaps
snap services                 # List snap services
```


## 10.5 Holding Packages

Prevent package from being upgraded:
```bash
sudo apt-mark hold package_name    # Hold package
sudo apt-mark unhold package_name  # Unhold package
apt-mark showhold                  # List held packages
```


## 10.6 Repository Management

Add repository:
```bash
sudo add-apt-repository ppa:repository/name
sudo apt update
```

Remove repository:
```bash
sudo add-apt-repository --remove ppa:repository/name
```

List repositories:
```bash
apt-cache policy
grep ^ /etc/apt/sources.list /etc/apt/sources.list.d/*
```


## 10.7 Package Management Lab

1. Update package lists:
```bash
sudo apt update
```

2. Search for a package:
```bash
apt search htop
```

3. Show package information:
```bash
apt show htop
```

4. Install htop:
```bash
sudo apt install -y htop
```

5. Run htop and verify it works:
```bash
htop
```
Press q to quit

6. Check if package is installed:
```bash
dpkg -l | grep htop
```

7. Find which files htop installed:
```bash
dpkg -L htop | head -10
```

8. Remove htop:
```bash
sudo apt remove htop
```

9. Purge configuration files:
```bash
sudo apt purge htop
```

10. Verify removal:
```bash
dpkg -l | grep htop
```

11. Install snapd (if needed):
```bash
sudo apt install snapd -y
```

12. Search for hello-world snap:
```bash
snap find hello-world
```

13. Install hello-world snap:
```bash
sudo snap install hello-world
```

14. Run the snap:
```bash
hello-world
```

15. List installed snaps:
```bash
snap list
```

16. Refresh snap:
```bash
sudo snap refresh hello-world
```

17. Remove snap:
```bash
sudo snap remove hello-world
```

18. Clean up unneeded packages:
```bash
sudo apt autoremove
sudo apt autoclean
```


# Appendix: Networking

**Networking Concepts - TCP/IP**

The Transmission Control Protocol and Internet Protocol
(TCP/IP) is a standard set of protocols developed in the late
1970s by the Defense Advanced Research Projects Agency
(DARPA) as a means of communication between different
types of computers and computer networks. TCP/IP is the
driving force of the Internet, and thus it is the most popular
set of network protocols on Earth.

The two protocol components of TCP/IP deal with different
aspects of computer networking. Internet Protocol, the "IP"
of TCP/IP is a connectionless protocol which deals only with
network packet routing using the IP Datagram as the basic
unit of networking information. The IP Datagram consists of
a header followed by a message. The Transmission Control
Protocol, the "TCP", enables network hosts to establish
connections which may be used to exchange data streams.
TCP also guarantees that the data between connections is
delivered and arrives in the same order as it was sent.

The TCP/IP protocol configuration consists of several
elements which must be set by editing the appropriate
configuration files or deploying solutions such as the
Dynamic Host Configuration Protocol (DHCP) server. The
DHCP can be configured to provide the proper TCP/IP
configuration settings to network clients automatically.
These configuration values must be set correctly to ensure
the proper network operation of your Ubuntu system.


The common configuration elements of TCP/IP and their purposes are as follows:
* Network interfaces are logical constructs that
represent a network device. The interface contains
the L2 (physical network) and L3 (IPv4 or IPv6
protocol layer) address information, configuration
information, and device state

* Network devices, frequently called NIC, are
physical hardware that connect to a network.
Virtual devices emulate a physical network facility
in software, (ex. a bond is a virtual device that
aggregates together a set of network interfaces.)

* The neighbour subsystem manages information
and configuration for L2 (link layer) destinations.

* Routing is the act of sending traffic originating on
one subnet to a second subnet.

* The routing layer manages information and
configuration for L3 protocol forwarding. This allows
L3 subnets to communicate with one another.

* The IP address is a unique string comprised of 4
decimal numbers ranging from 0 (zero) to 255,
separated by periods, with each of the four
numbers representing 8 bits of the 32 bit address.
This dotted quad notation is obtained through static
assignment or dynamically via DHCP.

* A subnet (for this document) is the extent of the
L3 (i.e., IPv4 or IPv6) network segment that
delineates the set of destinations to which an IP
datagram may be delivered without being routed.

* The Subnet Mask (or netmask) is a local bit
mask, or set of flags which separates the portions
of an IP address significant to the network from the
bits significant to the subnetwork. The standard
netmask 255.255.255.0 masks the first 3 bytes and
allows the last byte to remain available to specify
hosts on the subnetwork.

* The Network Address represents the bytes
comprising the network portion of an IP address.
The host 12.128.1.2 would use 12.0.0.0 as the
network address, where 12 represents the first byte
(the network part) and zeroes (0) in the remaining
3 bytes represent the potential host values.

* Gateway is synonymous with router; it is
responsible for the connection of two or more
subnets by passing L3 protocol traffic between
them.

* The Broadcast Address is an IP address which
allows network data to be sent simultaneously to all
hosts on a given subnetwork rather than specifying
a particular host. For example, on 192.168.1.0, the
broadcast address is 192.168.1.255. Broadcast
messages are typically produced by network
protocols such as the Address Resolution Protocol
(ARP) and the Routing Information Protocol (RIP).

* A Gateway Address is the IP address through
which a particular network, or host on a network,
may be reached. If one network host wishes to
communicate with another network host, and that
host is not located on the same network, then a
gateway must be used (ex. a router on the same
network).

* Nameserver Addresses represent the IP
addresses of Domain Name Service (DNS) systems,
which resolve network hostnames into IP
addresses. In order for your system to be able to
resolve network hostnames into their
corresponding IP addresses, you must specify valid
Nameserver Addresses which you are authorized to
use in your system's TCP/IP configuration. In many
cases, these addresses can and will be provided by
your network service provider, but many free and
publicly accessible nameservers are available.

**IP Routing**

IP routing is a means of specifying and discovering paths in
a TCP/IP network along which network data may be sent.
Routing uses a set of routing tables to direct the forwarding
of network data packets from their source to the
destination, via many intermediary network nodes known as
routers. The primary forms are Static Routing and Dynamic
Routing.

`Static routing` involves manually adding IP routes to the
system's routing table with the route command. It has the
advantage of simplicity of implementation on smaller
networks, predictability (the routing table is always
computed in advance so the route is the same each time),
and low overhead on other routers and network links due to
the lack of a dynamic routing protocol. The disadvantages
are it is limited to small networks and does not scale well . It
also fails completely to adapt to network outages and
failures along the route due to the fixed nature of the route.

`Dynamic routing` depends on large networks with multiple
possible IP routes from a source to a destination and makes
use of special routing protocols, such as the Router
Information Protocol (RIP), which handle the automatic
adjustments in routing tables that make dynamic routing
possible. It has several advantages over static routing:
superior scalability; the ability to adapt to failures and
outages along network routes; and less manual
configuration of the routing tables (routers learn from one
another about their existence and available routes
eliminating human errors). The disadvantages are
complexity and additional network overhead from router
communications, which consumes network bandwidth.


**Network Configuration Files**

In Ubuntu 24.04, networking is managed by Netplan.
The configuration file to manage the network interfaces is
typically `/etc/netplan/50-cloud-init.yaml` or similar. Here you 
can configure static, dynamic routing and DNS options.

Netplan uses YAML configuration files and supports both
NetworkManager and systemd-networkd as renderers. For
Ubuntu Server, systemd-networkd is the default renderer.

Example Netplan configuration:
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      dhcp4: true
```

Apply Netplan changes:
```bash
sudo netplan apply
```
