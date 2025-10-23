# Introduction to Ubuntu Server

**Ubuntu 24.04 LTS (Noble Numbat)**

**This material is copyright of Canonical Limited. This material may be used for personal and non-commercial use only.**

**This documentation is copyright of Canonical Limited. You are welcome to display on your computer, download and print this documentation or to use the hard copy provided to you for personal, education and non-commercial use only. You must retain copyright, trademark and other notices unaltered on any copies or printouts you make. Any trademarks, logos an service marks displayed in this document are property of their owners, whether Canonical or third parties.**

**This documentation is provided on an "as is" basis, without warranty of any kind, either express or implied. Your use of this documentation is at your own risk. Canonical disclaims all warranties and liability that may result directly or indirectly from the use of this documentation.**

---

## Table of Contents

1. [What is Ubuntu](#1-what-is-ubuntu)
2. [CLI Environment](#2-cli-environment)
3. [Linux Filesystem Hierarchy](#3-linux-filesystem-hierarchy)
4. [Identity and Ownership](#4-identity-and-ownership)
5. [Logging and Initialization](#5-logging-and-initialization)
6. [Storage](#6-storage)
7. [Networking](#7-networking)
8. [Process Management](#8-process-management)
9. [Backup and Recovery](#9-backup-and-recovery)
10. [Software Management](#10-software-management)

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

Linux is the world’s largest and most ubiquitous open
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


1. Go to the Ubuntu.com si te and find the latest
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

To end an SSH session use:

```
exit
```

To execute a single command on a remote system:

```
ssh remote_host command_to_run
```



## 2.2 CLI Commnads

Some of the common CLI commands are listed below. The
commands can also be used in scripts, which are programs
that automate tasks.

**Navigating the Filesystem**


`cd` - change directory

`ls` - list files

`mkdir` - create directory

`pwd` - print the name of the working directory

`rmdir` - remove directory


**Manipulating the Filesystem**

`chgrp` - change group ownership

`chmod` - change file mode bits

`chown` - change file owner (and group)

`df` - report filesystem disk space usage

`du` - estimate file space usage


**Searching the Filesystem**

`find` - search for file(s) matching a pattern

`locate` - find files by name

`which` - locate a command



**Search File Contents**

`file` - determine a file type

`grep` - print line(s) matching a pattern

`less` - view file contents

`more` - view file contents


**Manipulating File Contents**

`cp` - copy a file

`ln` - create a symbolic link

`mv` - move or rename a file

`rm` - remove a file or files

`touch` - change file time stamps


**Perform Text Processing**

`cat` - concatenate files

`gawk` - pattern scanning and processing language

`head` - display the first part of output

`tail` - display the last part of output

`tee` - read from input, write to STDOUT and a file


## 2.2.1 CLI Commands Lab

In this lab, we will use some of the CLI commands.


1. Use the `man` command to determine some of the
options for the `ls` command and try 4 different
options.

```bash
man ls
```

For example, some of the most useful options are:

`-a, --all` - do not ignore entries starting with .

`-h, --human-readable` - with -l and/or -s, print human readable sizes (e.g., 1K 234M 2G

`-l` - use a long listing format

`-t` - sort by modification time, newest first


2. Make a directory called `mydir1`.

```bash
mkdir mydir1
```

3. Change into `mydir1`.

```bash
cd mydir1
```

4. Make a directory cal led `mydir2`.

```bash
mkdir mydir2
```

5. Change into `mydir2`.

```bash
cd mydir2
```

6. Use the `..` shortcut to change to the parent of `mydir2`.

```bash
cd ..
```

7. Delete `mydir2`.

```bash
rmdir mydir2
```

8. Use the `touch` command to create `file1` in `mydir1`.

```bash
touch file1
```

9. Make a `copy` of `file1` called `file2`.

```bash
cp file1 file2
```

10. Change to the parent of `mydir1`.

```bash
cd ..
```

11. Create a directory called `mynewdir`.

```bash
mkdir mynewdir
```

12. Copy the contents of `mydir1` to `mynewdir`. Check
    the contents of `mynewdir`.

```bash
cp -a mydir1/* mynewdir
```

13. Type the following command: `ls my` and press the
    Tab key twice. What happened?

```bash
ls my
#press tab twice
```

> End of the lab, do not continue with the next topic


## 2.3 Getting Help

**man**

`man` is the system manual’s pager which provides syntax
information on any command used at the command line.
Arguments given to the man command determine the
'page' that is displayed. The arguments should be, at a
minimum, the name of a program, utility, or function. A
manual page may span several sections. If a manpage
exists in more than one section, the default action is to
display the first page in a predefined order of sections.

**man man**

This command provides comprehensive information on the
`man` command. It explains the sections, command line
switches to define how the manpages are displayed, the
exit status of the command, and the environment variables
the man command can use. For helpful pointers, use the
'SEE ALSO' section.

**Manual Page Sections**

```
1 Executable programs of shell commands
2 System Calls (functions provided by the kernel )
3 Library Calls (functions within program libraries)
4 Special files (usually found in /dev)
5 File formats and conventions
6 Games
7 Miscellaneous (including macro packages and conventions)
8 System administration commands
9 Kernel routines (non standard)
```

**man Navigation**

Navigating info pages is not intuitive

```
ENTER     Move down one line
SPACE     Move down one page
g         Move to the top of the page
G         Move to the bottom of the page
h/H       Display man help
q         Quit
ifup/down  Keyboard up/down keys can also be used
```



## 2.3.1 Getting Help Lab

In this lab you will work with the man command.

1. Type `man` at the prompt. What happens?

2. Type `man man` at the prompt. Scroll through and
   read some of the sections.

3. Use the navigation commands listed above to move
   through the page.

4. Type `h` while viewing the page. What happens?

5. Type `man ls` at the prompt and review the syntax
   for the command.

6. What does the `-p` option for the `rsync` command do?

```bash
man rsync
# search for the option
```


## 2.4 Shell Environment Variables

A shell environment variable is a character string to which a
value has been assigned. It is a name you give to refer to a
piece of information. It can be a number, text, filename,
device, or any other type of data.

Shell variables can store credentials used by a program and
a user doesn't have to type them. For example, `PATH` is an
environment variable in Linux that tells the shell which
directories to search for executable files.

The `env` or `printenv` command will list the currently
defined environment variables. Common variables include:

**SHELL** - defines the default shell

**USER** - user who is currently logged

**PWD** - current working directory

**HOME** - current user's home directory


To run a command from a directory not included in `PATH`,
you must precede the command with its full or relative
path, for example:

```bash
. /mycommand.sh
```
or
```bash
/home/myuser/mycommand.sh
```

### Naming Variables

By convention, variable names are UPPERCASE. The name
of a variable can contain only:

* Letters - a to z, A to Z
* Numbers - 0 to 9
* The underscore - _
* They cannot start with a number

Example:

```bash
export TERM=ansi TERM=xterm-256color
```


### Setting and Unsetting Variables

Variables are defined by setting a name=value pair:

```bash
NAME=Marvin
NAMES="Marvin Ford Arthur Trillian"
```

This would be local or valid until the shell exits.

Variables are undefined by using the shell built-in command
`unset`, or by setting the value to null:

```bash
unset NAME
```

```bash
NAME=
```

Variables can be made global using the `export` keyword:

```bash
export NAME=Marvin
```


### Manipulating Variables

To view the value of a variable:

```bash
echo $NAME
```

To add to the value(s) in a defined variable:

```bash
NAMES="$NAMES Zaphod"
```


Mark a variable read-only:

```bash
readonly NAMES
```


### Special Variables

These variables can be referenced only; they can never be
assigned a value.

The exit status of the last command executed

```bash
$?
```

The process ID (PID) of the current shell

```bash
$$
```

The PID of the last background command

```bash
$!
```

## 2.4.1 Shell Environment and Variables Lab

In this lab we will take a look at the environment files.


1. Use the echo command to view the `PATH` variable.
Note that your current directory is not included in
the list.

```bash
echo $PATH
```

2. Type `env` to see a list of the environment variables.

```bash
env
```

3. What do you think is the purpose of the `SHELL` variable?


4. Set a variable called `MYNAME` to your name.

```bash
MYNAME="john"
```

5. View the value of `MYNAME`.

```bash
echo $MYNAME
```

6. Create a variable called `PART1` and assign the value
**My name is**.

```bash
PART1="My name is"
```

7. Create a variable called `PART2` and assign your
name to the value.


```bash
PART2="john"
```

8. Display the value of each variable to ensure you set
them correctly.

```bash
echo $PART1
echo $PART2
```

9. Combine the variables in a third variable by
assigning the 2 variables to the new variable called
`STUDENTNAME`.

```bash
STUDENTNAME=$PART1$PART2
```

10. Display the value of `STUDENTNAME`.

```bash
echo $STUDENTNAME
```

11. Undefine the three variables you created in this
exercise.

```bash
unset PART1
unset PART2
unset STUDENTNAME
```

12. Check to make sure each variable is undefined.

```bash
echo $PART1
echo $PART2
echo $STUDENTNAME
```



## 2.5 Standard Streams

In computer programming, standard streams are
preconnected input and output communication channels
between a computer program and its environment when it
begins execution. The three I/O connections are called
standard input (STDIN), standard output (STDOUT) and
standard error (STDERR).


**Redirection**

Commands normally take input from your terminal or `STDIN`
via keyboard. Commands normally send output back to your
terminal or `STDOUT` via the screen. I/O redirection allows
commands to receive input from other files or commands. It
also allows commands to send output to files or other
commands.

**Output Redirection**

The output of a command can be sent to a file by using the
`>` character followed by a filename. If the filename does not
exist, it will be created. If the filename does exist, it will be
overwritten. The output of a command can be appended to
an existing file by using two `>` characters (`>>`) followed by
a filename.

```bash
ls -al > ~/all_files.txt
ls -al /tmp >> ~/all_files.txt
cat file1.txt file2.txt file3.txt > combined-file.txt
ping -c 2 www.xxx.yyy.zzz > /dev/null
```


**Input Redirection**


A command can receive input from a file by using the `<`
character followed by a file name. If two `<` characters are
used, a delimiter must be defined immediately after the
`<<`. The shell will read input until it reaches aline with the
defined delimiter.

```bash
wc -l < /etc/passwd
wc -l << EOF
```

This will count lines one at a time until it comes to the
delimiter defined above `EOF`.


**Piped Redirection**

The output of one command can be sent directly to another
command as input with the redirection symbol `|` . This is
referred to as a pipe or piping.

```bash
ls -al | less
ls -al | grep string
```

**Advanced Redirection**

There are three types of I/O which have their own identifiers
(file descriptors):

`0` - Standard Input (`STDIN`) - data fed into the program

`1` - Standard Output (`STDOUT`) - data printed by the
program which defaults to the terminal

`2` - Standard Error (`STDERR`) - error messages which
default to the terminal

These can be used to redirect a data stream from its normal
destination. Examples are shown below:

```bash
ls -al /r* > /tmp/list.txt 2>/tmp/errors.txt
ls -al /r* > /tmp/list.txt 2>&1
```

## 2.5.1 Redirection Lab


1. Send the output of the `ls` command to a file called
`mylist` and verify that the file has the expected
content.

```bash
ls > mylist
```


2. Use the pipe to list the last item of the first 5 items
of the `ls` output for the `/usr/bin` directory.

```bash
ls /usr/bin | head -5 | tail -1
```

3. You created a file called `mylist`. Use `grep` to list all
files with list in the name.

```bash
grep list mylist
```

4. Use the `ls` command with the `-lt` option to list the
10 newest files in `/usr/bin`.

```bash
ls -lt /usr/bin | head
```

5. Display the 10 largest directories and the space
they use sorted from largest to smallest.

```bash
du | sort -nr | head
```

6.  Create a file using `<<` to redirect text that you type
into a file until you type `EOF`. Name the file
`redirection.txt`. You can type any text that you
wish into the file. Use cat to check that what you
typed made it into the file.

```bash
cat >redirection.txt <<EOF
...
# your input
...
>EOF
```

7. The `less` command is used to page through text
one screen at a time. You will combine `less` with --
`help` and then pipe the output to `grep` to find all
commands with the word examine in them. Use
the `-i` option to make the search case insensitive.

```bash
less --help | grep -i examine
```


## 2.6 Command Chaining

Command chaining allows you to combine two or more
commands in the same command line execution. Execution
behaviour is determined by the chaining operator between
the commands. Multiple chaining operators can be used in
a single chained command.

**Chaining Operators**

`&` - Make a command run in the background

`;` - When placed between commands will cause each
command to run sequentially

`&&` - Will execute the second command only if the first
command succeeds

`||` - Will execute the second command only if the first
command fails

`!` - Will execute all except the condition provided

`|` - Causes the output of the first command to be
(piped as) the input for the second command

`{}` - Combines two or more commands

`()` - Sets the order of precedence for command
execution

`\` - Allows a command to span multiple lines or escape
special characters from shell interpretation


**Examples of Command Chaining**

Execute `ls`, `pwd`, `whoami` commands in one line
sequentially one after the other.

```bash
ls;pwd;whoami
```


ping `google.com` and display useful information to user if it
is ping-able or not.

```bash
ping -c1 google.com && echo "That's good, able to ping google.com" || \
echo "That's bad unable to ping google.com"
```

## 2.7 Return Values and Expansion


Well-behaved commands, programs, or scripts return an
exit status. A command that runs successfully returns an
exit code `0` . A command that does not run successfully
should return a non-zero exit code. The exit code of the last
command executed is stored in the shell variable `$?`.

**Expansion**

When a command is entered, the shell will split the
command into tokens. These tokens are expanded or
resolved when/where applicable. There are eight types of
expansions that are expanded in a specified order.

**Brace Expansion**

Brace expansion is a mechanism by which arbitrary strings
may be generated. It takes the form of a series of comma-separated
strings between a pair of braces and an optional
preamble and postscript (one is required). The preamble is
prefixed to each string within the braces. The postscript is
appended to each resulting string. Expansion occurs from
left to right. Brace expansion may be nested.

Examples:
```bash
echo sp{el,il,al}
mkdir {2014..2016}-0{0..9} {2014..2016}-{10..12}
touch fi le-{A..Z}{a..z}-{0..9}
touch {A..Z}{a..z}-{0..9}.txt
rm filename.{pl,sh,py,c}
```


**Tilde Expansion**

The tilde expansion is used to expand to several specific pathnames:
`home directories`, `current working directory` and  `previous working directory`.

Tilde expansion is only performed when the tilde construct
is at the beginning of a word or a separate word.
Examples are shown below:

```bash
cd ~
cd ~ubuntu
echo hello >~/hello.txt
```


**Shell Parameter / Variable Expansion**

The **$** introduces parameter expansion, command
substitution, or arithmetic expansion. The parameter name
or symbol may be enclosed in optional braces to protect the
variable to be expanded from characters immediately
following it, which could be interpreted as part of the
variable or parameter name. If the first character of a
parameter in braces is a `!` , then indirect expansion occurs:
the string after the `!` is used as the name, the resulting
variable is expanded, and the expanded value is used in the
rest of the substitution. Variables can be constructed with a
set value if they don't exist.

```bash
echo $SHELL
D=`date`
echo $D
echo ${D}d vs echo $Dd
echo ${!LC*} vs echo $LC*
echo ${TODAY:=`date+%A`}
```


**Command Substitution**
Command substitution allows the output of a command to
be replaced by the standard output of the command itself.
Command substitution occurs in one of two ways:

```bash
$(command)
`command`
```


Non-embedded trailing newlines from the standard output
of the command are deleted during command substitution.
Examples include:

```bash
echo "The date and time is: $(date)"
echo "The date and time is `date`"
echo $(env | grep ^LC_)
printf "%s\n" $(env | grep ^LC_)
```



**Arithmetic Expansion**

Arithmetic expansion allows the evaluation of an arithmetic
expression and substitutes the results. It works with
integers. All tokens within the expression undergo
parameter expansion, command substitution, and quote
removal . Variables within arithmetic expansion can be used
with or without variable expansion (with or without the **$**) in
some cases.

**Arithmetic Expansion Operators and Examples**

Operator Meaning

```
+        Addition
-        Subtraction
++       Increment by 1
--       Decrement by 1
+=       Addition assignment
-=       Subtraction assignment
*        Multiplication
/        Division
*=       Multiplication assignment
/=       Division assignment
%=       Remainder assignment
%        Remainder
!=       Inequality evaluation
Err:520  Equality evaluation
>        Greater-than comparison
<        Less-than comparison
>=       Greater-than or equal comparison
<=       Less-than or equal comparison
,        Expression separator
**       Exponentiation
```


**Wildcard Expansion**

The shell provides special characters that can be used to
specify groups of filenames. These special characters are
called wildcards. Wildcards allow for selecting
filenames/directories based on patterns of characters.
Wildcards can be used with any command that accepts
filename arguments.


**Types of Wildcards**

```
*               Matches any one or more characters
?               Matches any single character
[...]           Matches any one of the enclosed characters
[[:alnum:]]     Matches any alphanumeric character
[[:alpha:]]     Matches any alphabetic character
[[:digit:]]     Matches any numeric character
[[:upper:]]     Matches any uppercase alpha character
[[:lower:]]     Matches any lowercase alpha character
[![:alnum:]]    Matches any non-alphanumeric character
[![:alpha:]]    Matches any non-alphabetic character
[![:digit:]]    Matches any non-numeric character
[![:upper:]]    Matches any non-uppercase alpha character
[![:lower:]]    Matches any non-lowercase alpha character
```

Examples

```bash
ls -ld *
ls -ld *.log
ls -ld [abc]*
ls -ld *.??
ls -ld *.log.[[:digit:]]
ls -ld *.[[:digit:]].??
ls -ld *[![:alnum:]]
```


## 2.7.1 Chaining and Expansion Lab

In this lab we examine other shell commands.

1. Try the following examples of command chaining.

```bash
cd /temp; ls -al
cd /temp && ls -al
cd /temp || echo "Could not change to /temp"
cd /temp && ls -al || echo "Could not change to /temp"
cd /temp && ls -al || { echo "Could not change to /temp. Going home instead"; cd ~; pwd; }
(cd /temp && ls -al) || (cd /tmp && { ls -al | wc -l;})
ls -ld /var/log/!(*.gz)
```

2. List the last 10 items in the `/etc` directory.

```bash
ls -ltr /etc | tail
```

3. Use the `history` command to recall the commands in which you used `ls`.

```bash
history | grep ls
```

4. Execute the following commands for an example of parameter expansion.

```bash
file=data.txt
echo ${file%.*}
echo ${file#*.}
```

5. Try the following arithmetic expansion examples:

```bash
X=5
echo $((X+1))
echo $(($X+1))
echo $((X>15))
```

6. Try the following wildcard expansion examples:

```bash
cd /usr/bin
ls m*
ls *.???
```


## 2.8 Introduction to nano

**What is nano?**


nano is a small text editor that is easy to learn and use. It
also features an interactive search and replace, go to line
and column number, auto-indentation, feature toggles,
internationalization support, and filename tab completion.
nano is installed by default in Ubuntu and works well in
conjunction with sudo.

nano is started in 2 ways:
Use nano without parameters to edit a new file.

```bash
nano
```

Type name followed by a fi lename to edit an existing file.

```bash
nano /path/filename
```

The commands are displayed in the editor window. Press `Ctrl+g` in the editor to
see additional commands.


## 2.8.1 nano Lab

In this lab you will get a chance to practice creating and
editing files with nano. Make sure you review the nano
commands available within the editor.

1. Create a new file with a name of your choice.

```bash
nano fileone.txt
```

2. Type -`The quick brown fox jumped over the lazy dog.`

3. Copy the sentence you just typed and paste it 10 times.

```bash
CTRL + K
CTRL + U
```

4. Practice moving around in your file.

5. Close and save the file.

```bash
CTRL + X
```

6. Create a new file called  `shells.txt`.

```bash
nano shells.txt
```

7. Add the line - `She sells seashells down by the seashore.`

8. Copy and paste the line 3 times.

9. Close and save the file.

10. Open your original file.

11. Indent the first 3 lines.

12. Replace the word `fox` with `rabbit`.

```bash
CTRL + \
```

13. Move forward one word.

```bash
CTRL + space
```

14. Delete a line.

```bash
CTRL + K
```

15. Save and exit the file.

```bash
CTRL + X
```






# 3. Linux Filesystem Hierarchy


**Description:**

In this section you will learn to:
* Define the Linux Filesystem Hierarchy Standard
* Explain the Ubuntu Filesystem Hierarchy
* List and define Linux file types



## 3.1 The Filesystem Hierarchy Standard

**Directory Structure**

The Filesystem Hierarchy Standard (FHS) defines the
directory structure and directory contents in Unix and Unix-like 
operating systems. It is maintained by the Linux
Foundation. Currently, it is only used by Linux distributions.
In FHS, all files and directories appear under the root
directory `/`, even if they are stored on different physical
devices or they appear on different virtual devices.

The root filesystem must be adequate to boot, restore,
recover, and/or repair the system. It should be kept as small
as reasonably possible because:
* It is occasionally mounted from very small media.
* A small root filesystem is less prone to corruption
from a system crash.
* Applications must never create or require files or
subdirectories in the root directory.

To boot a system, the root filesystem must contain enough
software and data to mount other file systems and it must
include utilities, the configuration boot loader, and essential
start-up data.

To recover and/or repair a system, the root filesystem must
contain utilities needed by an experienced maintainer to:
* Diagnose issues
* Repair issues
* Reconstruct a damaged system

To restore a system, the root filesystem must contain
utilities to restore a system from backups.


**The cd command**

The `cd` command is used to change the current directory.
The syntax is:

```bash
cd [option] [directory]
```

The items in square brackets are optional . When used
without specifying a directory name, cd returns the user to
the previous current directory. This is a convenient way to
toggle between two directories.

When a directory name is provided, cd changes to that
directory. The name can be expressed as an absolute
pathname (i.e., a location relative to the root directory) or
as a local pathname (i.e., a location relative to the current
directory). It is usually more convenient to use a local
pathname when changing to a subdirectory of the current
directory.

The following are examples of using an absolute path - the
current directory is changed to the directory specified.

```
cd /           Change to the root
cd /usr/sbin   Change to the /usr/bin directory
```

To change from the current directory into a subdirectory,
use cd followed by the name of the subdirectory.

```
cd gconf       Change to gconf from within etc
```

In Linux, the current directory is represented by a single dot
and its parent directory (i.e., the directory that contains it)
is represented by two consecutive dots. To change to the
parent of the current directory use:

```
cd ..          Change to the parent directory
```

Another shortcut is to return to the home directory by using
a tilde as the argument. A user's personal files, directories,
and programs are found in the home directory.

```
cd ~           Change to the home directory
```
Other options:

```
cd -           Move one directory back
cd ../../      Move 2 directories up from the current
```


## 3.2 Required Root Filesystem Directories

The following directories, or symbolic links to directories,
are required in `/` (root):

```
/bin     - essential command binaries
/dev     - device files
/etc     - host-specific system configuration
/sbin    - essential system binaries
/usr     - secondary hierarchy
/var     - variable data hierarchy
/boot    - static files of the boot loader
/lib     - essential shared libraries and kernel modules
/media   - mount point for removable media
/mnt     - mount point for mounting a filesystem temporarily
/tmp     - temporary files
```

Modern Linux distributions include the following additional
root filesystem directories:

```
/sys     - Kernel system information virtual filesystem
/proc    - Kernel process information virtual filesystem
```

The following are optional root filesystem directories:

```
/home                - User home directories
/lib64 or /lib32     - Essential system libraries
/root                - Home directory for the root user
```


**The /usr Hierarchy**

The second major section of the filesystem is shareable and
should only contain read-only data. It requires the following
subdirectories:

```
/usr/bin       - Most user commands
/usr/lib       - Libraries
/usr/local     - Local hierarchy
/usr/sbin      - Non-vital system binaries
/usr/share     - Architecture independent data
```


It can contain the following optional subdirectories:
```
/usr/games                  - Games and educational binaries
/usr/include                - Header files included by C programs
/usr/l ibexec               - Binaries run by other programs
/usr/lib64 or /usr/lib32    - Alternate format libraries
/usr/src                    - Source code
```

**The /var Hierarchy**

The `/var` directory is designed for files that vary in size:
* Spool directories and files
* Administrative and logging data
* Transient files
* Temporary files

It requires the following subdirectories:

```
/var/cache     - Application cache data
/var/lib       - Variable state information
/var/local     - Variable data for /usr/local
/var/lock      - Lock files
/var/log       - Log files and directories
/var/opt       - Variable data for /opt
/var/run       - Data relevant to running processes
/var/spool     - Application spool data
/var/tmp       - Temporary files preserved between system reboots
```

**The /etc Hierarchy**

The `/etc` directory maintains a lot of files. Some are
described below.

```
/etc/passwd - user database with user information such as username, home directory, etc
/etc/shadow - encrypted file that holds user passwords
/etc/fstab - lists the filesystems mounted automatically at startup by the mount -a command
/etc/group - similar to /etc/passwd, but describes groups instead of users
/etc/inittab - configuration file for init
/etc/mtab - list of currently mounted filesystems
/etc/profile, /etc/bash.rc - files executed at login or startup time by the Bourne, BASH
```

## 3.2.1 Directory Structure Lab

In this lab we wi ll take a look at the filesystem directories
and their contents.


1. Make sure you are in the root directory of the filesystem.

```bash
pwd
```

```bash
cd /
```

2. List the contents of the root directory.

```bash
ls
```

3. Compare what you see on the screen to the list in your lab manual.

4. Change into the `/usr` directory.

```bash
cd /usr
```

5. Look at the contents of the `usr` subdirectories listed on the preceding page.

```bash
ls lib
```

```bash
ls games
```

```bash
ls local
```

6. Change back to the root directory.

```bash
cd /
```

7. Change to the `/var` directory.

```bash
cd /var
```

8. Look at the contents of the var subdirectories listed on the preceding page.

```bash
ls cache
```

9. Change to the local subdirectory.

```bash
cd cache
```

10. Move 2 directories up from the current directory.

```bash
cd ../../
```

11. Change to the Home directory.

```bash
cd ~
```

12. `tree` is a command that lists the directory contents in a tree-like format. Install and run it:

```bash
sudo apt install tree -y
```

```bash
cd ~
```

```bash
tree
```

13. List `/` directory and inspect it for a minute :

```bash
tree -L 1 /
```

14. Go one level deeper by changing the `-L` flag to 2 and inspect it for a minute:

```bash
tree -L 2 /
```


## 3.3 Linux File Types

**Determining File Types**

Use the `ls` to determine a file type command.

```bash
ls -l /etc/hosts
# output
-rw-r--r-- 1 root root 8 Mar 12 2018 /etc/hostname
```

The first character in the output, a `-` in this case, indicates
the file type. The Linux file types are:

| Type          | Symbol           | Description  |
| ------------- |:-------------:   | :-----:       |
| regular       | -                | any file that is not a directory and not one of the special file types|
| directory     | d                | second-most common file type |
| socket        | s                | provides inter-process communication |
| block         | b                | a type of device file which communicates with a driver in the kernel|
| link          | l                | pointers to other files, can be soft or hard links
| named pipe    | p                | similar to sockets, pronide ICP but without using network sockets 
| character     | c                | a type of device file which communicates with a driver in the kernel|


A `regular file` is any file that is not a directory and not one of the special
file types. Regular files are the most common file types such as:
* Binaries or libraries installed on a system
* Configuration files
* Image fi les
* Audio or Video files
* Data files
* Tar files

A `directory` is the second-most common file type. It is used to organize all other
file types. Basically, a file that is a list of other files.

A `socket` provides inter-process communication. If it has a filename, it
can be accessed through the filesystem. TCP/IP sockets can be
accessed over the network.

A `block file` is a type of device file which communicates with a driver in the
kernel . The driver then communicates with a piece of hardware (usually).
Data flow is buffered. It is used with block storage devices like disks or
memory. Block special files include: `/dev/sda` or `/dev/ram0`.

A `pipe` is similar to a socket. It allows processes to communicate with each
other without using network socket symantics (the processes do not have
to be designed to work together.)

The `character` is a type of device file which communicates with a driver in the
kernel . The driver then communicates with a piece of hardware (usually).
Data flow is unbuffered. Each character is consumed as soon as it is
written to the file. Each character is read as soon as it is provided by the
file. Examples are keyboard and mouse files. Character special files
include: `/dev/ttyS0` `/dev/console`.


## 3.3.1 File Types Lab

In this lab you will take a look at file types.
Execute the following commands in a terminal window.

1. Run the following commands. What type of files are they?

```bash
ls -ld /etc
```

```bash
ls -l /etc/hosts
```

```bash
ls -l /dev/tty
```

```bash
ls -l /dev/sda
```

2. Run the following command. There are many file types there. What do you think is in `dev`?

```bash
ls -l /dev/
```

3. Create a file and inspect it, what type is it?

```bash
touch file1.txt
```

```bash
ls -l file1.txt
# output
-rw-rw-r-- 1 ubuntu ubuntu 0 Feb 12 12:41 file1.tx
```

4. Create a directory and inspect it.

```bash
mkdir dirone
```

```bash
ls -l | grep dirone
# output
drwxrwxr-x 2 ubuntu ubuntu 4096 Feb 12 12:43 dirone
```

5. Remove the file and directory:

```bash
rmdir dirone/
```

```bash
rm file1.txt
```

# 4. Identity and Ownership

**Description:**

In this section you will learn to:
* Perform Linux user management tasks
* Use sudo to escalate privileges


## 4.1 User Management

**User Accounts and their purpose**

With user accounts, you can manage access to the system
via a unique login name and password. You can also control
access to specific features and files via permissions
associated with the user account. Finally, you can keep
track of what users do in the system via logging.


**User Account Information**

Information about user accounts is stored in files on the
system, such as `/etc/passwd` and `/etc/shadow`. Passwords
are encrypted and cannot be read directly by accessing
these files.


**Commands to Manage Users**

```
adduser    - create a user account
usermod    - modify it later
passwd     - change a password
deluser    - delete a user account
```


**Permissions**

Permissions are attached to files and directories. They allow
read, write, and execute access. Permissions can be given
to specific user accounts or to groups.

Example:

```bash
ls -l file.txt
# output
-rwxrwxr-x 1 student01 ststrain ... file.txt
```

**Groups**

Groups consist of one or more user accounts. They are used
to assign and remove permissions more easily. For example,
if a new user account needs access to a shared directory for
a project called "Fascinate", the user account can be added
to a group called Fascinate and immediately have the same
access and permissions as other project members.

**Commands to Manage Groups**

```
addgroup    - create a group
groupmod    - modify it later
delgroup    - delete a group
adduser     - add a user to a group
deluser     - remove a user from a group
```

## 4.2 Privilege Delegation

**Types of User Accounts**

Most user accounts by default have access to a small set of
files and directories. This prevents regular users from
changing system files or accessing programs that they
should not. An account with administrative privileges has
access to a wider range of possibilities. These are often
called super users.

**Super Users**

Super users have the ability to access or change any file or
directory on the system. This includes files and directories
owned by other users. Traditionally, this was done using a
special super user account called `root`. Today, there is an
easier way to give, revoke, and use these powers - `sudo`.

**Temporary Admin Power with Sudo**

There is a special group on Ubuntu systems called `sudo`.
Any user account added to this group has access to the
`sudo` command. Placing `sudo` in front of any other command
tells the system to run that command with admin privileges
as a super user. Doing so requires users to enter their
password.


**Partial Admin Access with Sudo**

```bash
sudo visudo /etc/sudoers
```

When you run `visudo`, you open `/etc/sudoers`. Visudo
prevents several people from editing at the same time and
checks the file for proper syntax before saving. If the
sudoers file is currently being edited, you will receive a
message to try again later.

To give a user `ubuntu` access to `/usr/bin/command`, add
this line:

```bash
ubuntu ALL=(ALL) : /usr/bin/command
```

You don't have to give full admin access with sudo.
You can put multiple commands in the same line as you
give access. You can even limit the arguments the user can
use with the permitted command and also not require a
password, like this:

```bash
ubuntu ALL=(ALL) NOPASSWD: /usr/bin/command arg1 arg2
```

**Alternate Sudo Options**


`sudo -i` -  runs a shell with root privileges, using the root
user's environment, basically the same thing as `sudo su`.

`sudo -s` -  runs a shell with root privileges, but using YOUR
user's environment and attached to YOUR user's `/home` directory.

`sudo -`  -  --user=user runs the command as a user other than the default target user.


To see the difference, run each and follow with `cd ~` and
`pwd` as well as `$PATH`.


## 4.2.1 User Management & Privileges Lab

In this lab you will work on user and group management.


1. Create a group named `students`.

```bash
sudo addgroup students
```

2. Check the new entry in /etc/group.

```bash
grep students /etc/group
```

3. Create a user named `labuser` whose primary group is `students`. The password is disabled for now, we'll set it later. For the rest of the information, just hit `ENTER`.

```bash
sudo adduser --ingroup students --disabled-password labuser
```

```bash
# output 
Adding user `labuser' ...
Adding new user `labuser' (1004) with group `students' ...
Creating home directory `/home/labuser' ...
Copying files from `/etc/skel' ...
Changing the user information for labuser
Enter the new value, or press ENTER for the default
    Full Name []:
    Room Number []:
    Work Phone []:
    Home Phone []:
    Other []:
Is the information correct? [Y/n] Y
```



4. Check the new entry in the /etc/passwd file.

```bash
grep labuser /etc/passwd
```

5. Check the new entry in /etc/shadow.

```bash
sudo grep labuser /etc/shadow
```

6. Set the password for the newly created student account. Use `ubuntu` as password.

```bash
sudo passwd labuser
```

7. Check the entry now in /etc/shadow.

```bash
sudo grep labuser /etc/shadow
```

8. Add the new user to the sudo group.

```bash
sudo adduser labuser sudo
```

9. Check the supplementary group in /etc/group.

```bash
grep labuser /etc/group
```

10. Assume the role of the newly created `labuser` user.

```bash
sudo su - labuser
```

11. Print the `HOME` environment variable, should be different from the home dif of ubuntu:

```bash
echo $HOME
```

12. Exit the role of the newly created `labuser` user.

```bash
exit
```

13. Remove the labuser account just created and remove the home directory with it.

```bash
sudo userdel -r labuser
```

14. Assume the role of root with root's environment.

```bash
sudo -i
```

15. Exit the role of root.

```bash
exit
```


## 4.3 Permissions

As far as the system is concerned, everything in the file
system is a file, including directories and devices. Every file
in the system has an accompanying set of access permissions.

Permissions have three levels of granularity:

```
Owner - user who owns the file
Group - group to which the file belongs
Other - specifies whether non-owner, non-group-member user accounts have access
```

There are three permissions assigned to files:

```
Read    - who may read/view its contents
Write   - who may modify/delete the file
Execute - who may execute a file
```

The letters at the beginning of the line denote permissions

```bash
ls -l
# output
-rwxr-xr-x 1 owner group 1088 Jan 21 201 6 backup.sh
```

The initial character is the file type. It is blank(-) for files.
It will be d for directories. Other characters are `c` for
character devices and `b` for block devices. The next nine
characters are permissions for owner, group, and other
(public).

```
r is read.
w is write.
x is execute.
```

The number after the permissions indicates the number of
links to the file. Then, we have the owner and group
associated with the file. Finally, we have file size, the most
recent date it was changed, and the name of the file.
**Note**: You must have executable permission on a directory
to cd into it.


**Assigning Ownership**

The account used to create a file is the file's default owner
and the account's primary group is the default group.
`groups` shows the groups on a system. `chown` is used to
change the file's owner and group.

```bash
chown username:group filename
```


**Default Permissions**

When you create a file, it is created with a default set of
permissions. To adjust what those default permissions are,
you can use `umask` or `chmod`. `umask` sets/changes
default permissions for future new files created and `chmod`
modifies existing permissions.

**chmod Options**

Using chmod with the following notation allows you to add
or remove the three permissions to or from the three entities, as follows:

```
u  user/owner
g  group
o  other (public)
r  read
w  write
x  execute
```

Use `+` to add and `-` to remove permissions.


**Assigning Permissions**

Use `chmod` to change permissions:

```bash
chmod g-w filename
```


g-w removes write permission from group

```bash
chmod o-w filename
```
o-w removes write permission for others(public)


**chmod Using Numbers**
chmod also uses a number notation:

```bash
chmod 755 filename
```

Each digit sums the permissions for user, group, and other.

Read=4, Write=2, Execute=1, All three=7

755 = (rwx)(r-x)(r-x)

644 = (rw-)(r--)(r--)


**umask**

The user file-creation mode mask (umask) is used to
determine the file permission and to control the default file
permission for new files. It, like permissions, is a three or
four-digit octal number that is the opposite of each other.
The octal notations are as follows:

```
umask  meaning                    permission
0      read, write, and execute   7
1      read and write             6
2      read and execute           5
3      read only                  4
4      write and execute          3
5      write only                 2
6      execute only               1
7      no permissions             0
```

`umask` has the permissions subtracted from the `777`. So, to
have all files created with `777` permission, you would type
umask `000`. The default umask is `022`. By default, file
permissions would be `755`. To change the default to `740`, so
the owner has full permissions, group read, and other none,
you would use:

```bash
umask 037
```

**Using chmod and umask**

For full access to files (`rwxrwxrwx`):

```bash
chmod 777 filename
chmod uog+rwx filename
umask 000 filename
```

User only access (`rwx------`):

```bash
chmod 700 filename
chmod og-rwx filename
umask 077 filename
```

Add write access to all:

```bash
chmod ugo+w filename
```

**Who can make changes?**

Depending on the existing file permissions and who owns
the file, you may or may not need admin privileges to
change file ownership and permissions. If you own it, you
can change it. If not, you may or may not be able to
depending on your group membership and other settings.


## 4.3.1 Permissions Lab

In this lab you will work with file permissions.

1. Create an empty file named `file01.txt`.

```bash
touch file01.txt
```

2. Create a directory named `test01`.

```bash
mkdir test01
```

3. List the file to see permissions:

Output will be based on the current umask setting
but should, by default, be something like:

```
-rw-rw-r--  file01.txt
drwxrwxr-x  test01
```

```bash
ls -l
```

4. Change the default permissions so that the only owner has read/write permissions and
directories with only owner read/write/execute permissions.

```bash
umask 077
```

5. Create another empty file named `file02.txt`.

```bash
touch file02.txt
```

6. Create a directory named `test02`.

```bash
mkdir test02
```

7. List the files to see the difference in permissions.
The output should look something like the listing below.

```
-rw-rw-r--   file01.txt
-rw-------   file02.txt
drwxrwxr-x   test01
drwx------   test02
```

```bash
ls -l
```

8. Change permissions on the `file02.txt` to make it match `file01.txt`.

```bash
chmod 664 file02.txt
```

9. Change the permissions on `test02` to make them match `test01`.

```bash
chmod 775 test02
```

10. List the files to verify the changes.

```bash
ls -l
```









# 5. Logging and Initialization

**Description:**

In this section you will learn to:
* Configure system logging
* Explain the Ubuntu boot process
* Explain the Linux kernel initialization process
* Explain systemd features and functionality


## 5.1 System Logging

Logging refers to recording messages about the operation
of the software running on a machine. This occurs whether
it is working properly and simply documenting what it is
doing, or something is going wrong and diagnostics are
being written that might help solve the problem. Logging is
important for a system administrator to be able to
understand and monitor many aspects of a system's
configuration. Standard core logging implementation on
Ubuntu includes rsyslog, some basic configuration for the
rsyslogd logging daemon, and an overview of managing
your logs using logrotate.

Logs are generated by different applications and daemons
at various times, such as when they start, stop, change
state, and undertake whatever their normal operations are.
Kernel and networking events, the authentication and
actions of users, and unexpected system exceptions can
also log messages.


If problems occur in any of these areas, checking on the
logs is often the best first step in troubleshooting.
Logs also play a crucial role in ensuring the security of your
systems by allowing you to detect suspicious or undesired
activity.

On Ubuntu systems, logs from the local machine's software
will be written to a local disk. It is also possible to configure
devices to log to a remote log collector on a different
machine over a network.

The default directory for most logs to be written to is
`/var/log/`. Many application and core system operations are
logged to files within `/var/log` by the rsyslogd daemon,
which can handle messages logged from many different
sources based on configurable rules we will cover later.

Examples of these files are:
```
/var/log/kern.log
/var/log/syslog
/var/log/auth.log
```

Some applications may also write to their own separate logs
in this directory or in subdirectories within `/var/log/`.


**System Logging and Troubleshooting**

Logs are most useful as a first stage of debugging when
something doesn't appear to be working correctly. Although
there is no consistent set of rules for how to troubleshoot an
issue based on log entries, there are a few general
techniques one can use. In logfiles where there are multiple
sources of logs being written, the name of the application or
daemon writing the individual entry is often listed. If you're
looking for errors from a specific source, you can search by
name, for example:

```bash
grep NetworkManager /var/log/syslog
```

Log entries are often written with some indication of
whether it is documenting normal operation (in which case
it may have the word info to indicate this), something
which is potentially problematic (where it might contain the
word warning or warn), or something which indicates a
serious fault (might contain the word error). Searching for
the phrase 'error' within a log file may return relevant
information:

```bash
grep -i "error" /var/log/syslog
```

The absence of normal log entries can also be used to
determine if and when something might have gone wrong
with a certain application or daemon. By checking older log
entries, before an issue started, for entries which may have
subsequently stopped, one can narrow down the window of
time when something changed. This may help to correlate
an issue to a configuration change or update that happened
at that time.

**dmesg**

dmesg displays messages from the kernel ring buffer. It is useful
for inspecting device behaviour or kernel boot logs.



**logrotate**

logrotate is used to rotate old logs out at a specified
interval . This is useful for clearing out logs which may have
been written too far in the past to be relevant to the
administrator or may be taking up space unnecessarily on a
machine's disk.

The process of rotating a log comprises archiving the
existing log file to a different location, then creating a new
one in its place. The rotated log can be compressed for
long-term storage, deleted entirely, or processed in some
other way using a script. Rotating is done on a logfile-bylogfi
le basis and is usually done daily, but depending on the
size of the logs, could be configured to be done weekly,
monthly, or at another interval.

The main logrotate configuration file is `/etc/logrotate.conf`. 
The file contains an option to include
any number of supplementary config files that can override
the defaults set out in the main file such as when dealing
wi th a specific application's logfile. The entire
`/etc/logrotate.d` directory is included by default. It contains
files which correspond to individual applications. The files
may inherit or overwrite information from the main file. The
following command:

```bash
ls /etc/logrotate.d/
```

Will produce the following output:
```
apport apt aptitude dpkg landscape-client ppp
rsyslog ufw unattended-upgrades upstart
```

When installing new software that has logging features, new
configuration entries will be generated in this directory.


**Scheduling logrotate**

logrotate is activated on a schedule, which is determined by
the location of the logrotate script in one of the cron
directories. These are different directories in `/etc`, such as
`/etc/cron.daily` and `/etc/cron.hourly`. cron, which run the
scripts contained in these directories at the interval shown
in their name (daily and hourly respectively). Cron will be
covered in more detail a bit later in the course. The log
rotation command logrotate can also be run from the
command line. An example of this is:

```bash
sudo logrotate /etc/logrotate.conf
```

When logrotate runs, it will undertake the rotations defined
by each of its included configuration files.


**logrotate Configuration Options**

The options within a logrotate configuration file determine
the criteria for when to rotate logs. Here is an example
`/etc/logrotate.conf` with comments:

```
# see "man logrotate" for details
# rotate log files weekly
weekly
# use the syslog group by default, since this is the owning group
# of /var/log/syslog.
su root syslog
# keep 4 weeks worth of backlogs
rotate 4
# create new (empty) log files after rotating old ones create
# uncomment this if you want your log files compressed
#compress
# packages drop log rotation information into this directory
include /etc/logrotate.d
```

Another common option that may be found is size, which
dictates the size a logfile must exceed to be rotated. This is
a useful measure to prevent against sudden unexpected
disk usage from, for example, an error occurring which
spews large volumes of log messages.

In supplementary configuration files, the first line is the
logfile location and precedes a block of options, which will
apply to it. These are always contained within braces - {} -
and may either override default settings in `logrotate.conf` or
be new options that will only apply to this file. Note that
options that are present in `logrotate.conf`, but not in a
supplementary configuration file, will still apply since the
main file's options are the default.


Rotation of multiple log files can be configured in the same
file within the `logrotate.d` directory. Here is an explanation
of the options you might find in an example logrotate
configuration file for dpkg.

`rotate 12` - Determines the number of logfiles to be
archived and kept. Once this number is
exceeded, the oldest archived log will be
deleted.

`monthly` - If this option is present, logrotate will rotate
the logs after this amount of time (other options
include daily and yearly). In this instance, this
option is different than the one I have specified
in my `/etc/logrotate.d`, which is set to 'weekly' -
because the supplementary configuration file is
loaded afterwards, it will override to 'monthly' .

`missingok` - If there is no logfile present, continue
without raising an error.

`notifempty` -  Signifies not to rotate the logfile if it is
empty.

`compress` - Archived logfiles should be compressed
(using gzip compression by default).

`delaycompress` - This option will prevent the archived
logs from being compressed until the next
rotation - so there will always be one rotated
log, which is still uncompressed. This allows the
first rotated log to still be written to if, for
instance, the software doing the logging needs
to continue writing to the same file for a time
after the rotation has taken place.

`create` - `<mode> <owner> <group>` - Specifies the
ownership and permissions of the newly created
replacement logfile after rotation of the previous
one has taken place.

It is also common to find a postrotate option in
supplementary config files. This is a small block ending with
the endscript line and specifies a script, which will run
after rotating the log. It is usually used to restart the
application so that it begins using the newly rotated-in log.

```
postrotate
  invoke-rc.d rsyslog reload >/dev/null 2>&1 || true
endscript
```

**Troubleshooting logrotate**

It is possible to see a recent history of the rotations which
have taken place by reading the contents of `/var/lib/logrotate/status`:

```bash
cat /var/lib/logrotate/status
logrotate state -- version 2
"/var/log/auth.log" 2016-6-19-6:46:8
"/var/log/landscape/package-changer.log" 2016-6-22-6:0:0
"/var/log/landscape/watchdog.log" 2016-6-22-6:0:0
"/var/log/kern.log" 2016-4-8-6:47:51
...
```

This shows the logfiles that were rotated and the date at
which the rotation occurred. When troubleshooting or
testing a new configuration for logrotate, it is useful to run
logrotate from the commandl ine using the `-f` and `-v`
options, for force and verbose:

```bash
sudo logrotate -vf /etc/logrotate.conf
```

This will cause logrotate to rotate logs according to the
specified configuration file (in this case, the main one). It
has the include option to also complete the rest of the
additional config files. This forces rotations even if one was
recently performed and prints output describing all actions
being taken.

## 5.1.1 System Logging Lab

1. Check the Linux kernel message buffer. This will display information
starting with the boot.

```bash
dmesg
```

2. You can also use `less` to nagivate the file. Press `q` to exit.

```bash
dmesg | less
```


3. Look for specific information about a disk device. The name of the device
should start with `sda`.

```bash
dmesg | grep -i sda
```

4. Put the information in a file called `sda.txt`. This way you can share
and pass around the device logs if needed.

```bash
dmesg | grep sda > sda.txt
```

5. Another log file you can check is `/var/log/syslog`. Look in it for any errors.
If you see any errors do not mind them, just continue.

```bash
grep error /var/log/syslog
```

6. You can also check the first or the last `n` lines of the file.

```bash
head -n 10 /var/log/syslog
```

```bash
tail -n 10 /var/log/syslog
```

7. There are also application specific log files. Do a `ls` in `/var/log` to see what log files
are present.

```bash
ls -l /var/log
```


## 5.2 Boot Process Overview

**Initial Boot Activity**

* Power on
* Load boot firmware - BIOS/EFI/UEFI from a chip on
the motherboard. This includes some basic drivers
for keyboard, video, and some way to get an OS
(from USB, DVD, Hard Drive, network).
* Load a boot loader from any one device. Ubuntu
uses GRUB2, discussed in the next section.


**initrd**

Between the boot loader and before the kernel is loaded, a
temporary user space image is loaded called `initrd`.
This replaces many of the functions of the kernel during the
initial boot process such as detecting hardware and
determining which drivers must be loaded wi th the kernel.
This is done in a temporary RAM filesystem.


**The Kernel**

In Ubuntu, the Linux kernel manages system resources.
Users do not typically interact directly with the kernel. The
kernel is the layer of software between the hardware and
the applications users run. The kernel assigns each
application a process ID (PID) and actively monitors and
manages the resources for each process.


**Init Process**

Once the kernel loads, the first process it runs is init.
Ubuntu Server 18.04 uses systemd as its init process. The
init process will load processes according to a defined
configuration. It includes all necessary foundational
processes and tools to load and run user programs. Boot up
is complete.


**The Boot Loader**

The bootloader Ubuntu uses is GRUB2. GRUB stands for
Grand Unified Boot Loader. The bootloader is the first thing
to run after the BIOS or UEFI initializes the hardware. It
serves to load and transfer control of the system to an
operating system kernel, in our case the Linux kernel . Once
this task is complete, the bootloader is done.


**Starting Up the Kernel**

One traditional job of the kernel is to detect hardware and
load device drivers. When the kernel is loaded, the real root
filesystem replaces initrd. Everything that is not the kernel is user space.


## 5.2.1 Boot Process Lab

1. List the `/boot` directory. This is the location where the compressed 
kernel and the initrd are, among other files. 

```bash
ls -l /boot/
# output
-rw------- 1 root root 3.9M Aug 29 04:25 System.map-4.15.0-1019-gcp
-rw------- 1 root root 3.9M Dec  6 16:32 System.map-4.15.0-1026-gcp
-rw------- 1 root root 3.9M Jan 15 11:33 System.map-4.15.0-1027-gcp
-rw-r--r-- 1 root root 1.5M Aug 29 04:25 abi-4.15.0-1019-gcp
-rw-r--r-- 1 root root 202K Aug 29 04:25 config-4.15.0-1019-gcp
-rw-r--r-- 1 root root 202K Dec  6 16:32 config-4.15.0-1026-gcp
-rw-r--r-- 1 root root 202K Jan 15 11:33 config-4.15.0-1027-gcp
drwxr-xr-x 4 root root  512 Jan  1  1970 efi
drwxr-xr-x 6 root root 4.0K Feb 11 12:20 grub
-rw-r--r-- 1 root root  19M Jan 11 13:27 initrd.img-4.15.0-1019-gcp
-rw-r--r-- 1 root root  19M Jan 11 13:27 initrd.img-4.15.0-1026-gcp
-rw-r--r-- 1 root root  23M Feb 11 12:21 initrd.img-4.15.0-1027-gcp
-rw-r--r-- 1 root root 179K Jan 28  2016 memtest86+.bin
-rw-r--r-- 1 root root 181K Jan 28  2016 memtest86+.elf
-rw-r--r-- 1 root root 181K Jan 28  2016 memtest86+_multiboot.bin
-rw-r--r-- 1 root root    0 Aug 29 04:25 retpoline-4.15.0-1019-gcp
-rw------- 1 root root 7.9M Aug 29 04:31 vmlinuz-4.15.0-1019-gcp
-rw------- 1 root root 7.9M Dec  6 16:33 vmlinuz-4.15.0-1026-gcp
-rw------- 1 root root 7.9M Jan 15 12:22 vmlinuz-4.15.0-1027-gcp
```




## 5.3 Systemd

Systemd is the default init system in Ubuntu. It is run as the
first process once the kernel is loaded. All processes that
load subsequent to this are managed by the init system.


**Units**

Systemd calls processes "units". A unit will start if and when
another unit depends on it. During boot, systemd starts a
"root unit" that then expands its dependencies.
Units are defined in files stored in `/etc/systemd/system` or
`/lib/systemd/system` and the subdirectories of these
locations.


**Unit Definition Files**

Files that define systemd units contain information about
the unit, how it is to be run, and any dependencies it has.
For a unit to start during boot, it must be a dependency of
an existing boot target, beginning with the initial root unit,
`default.target`.


**Systemd Interaction**

Scripts interact with systemd using init system agnostic
abstractions like `invoke-rc.d`, which is a traditional way to
run init scripts. Human interaction with systemd is available
using commands, which are unique to systemd.


**Systemd Commands**

Here are some examples, where `$unit` is replaced by the
name of the service you want to control:


`systemctl start $unit`  - to start a unit

`systemctl stop $unit` - to stop a unit

`systemctl restart $unit` - to restart a unit

`systemctl status` - to view the status of services


## 5.3.1 Systemd Lab

In this lab you will use systemctl commands to interact with systemd.


1. Use the man command to research systemctl.

```bash
man systemctl
```

2. Stop the cron service.

```bash
sudo systemctl stop cron
```

3. Show the current status of the cron service.

```bash
sudo systemctl status cron
# output
* cron.service - Regular background program processing daemon
   Loaded: loaded (/lib/systemd/system/cron.service; enabled; vendor preset: enabled)
   Active: inactive (dead) since Wed 2019-02-13 11:13:43 UTC; 13s ago
     Docs: man:cron(8)
  Process: 1291 ExecStart=/usr/sbin/cron -f $EXTRA_OPTS (code=killed, signal=TERM)
 Main PID: 1291 (code=killed, signal=TERM)

Feb 13 10:58:39 ubuntu systemd[1]: Started Regular background program processing daemon.
Feb 13 10:58:39 ubuntu cron[1291]: (CRON) INFO (pidfile fd = 3)
Feb 13 10:58:40 ubuntu cron[1291]: (CRON) INFO (Running @reboot jobs)
Feb 13 11:13:43 ubuntu systemd[1]: Stopping Regular background program processing daemon...
Feb 13 11:13:43 ubuntu systemd[1]: Stopped Regular background program processing daemon.
```

4. Start the cron process.

```bash
sudo systemctl start cron
```

5. Show the current status of the cron service. Note
the difference in status between step 3 and step 5.

```bash
sudo systemctl status cron
# ouput
* cron.service - Regular background program processing daemon
   Loaded: loaded (/lib/systemd/system/cron.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2019-02-13 11:14:10 UTC; 1min 17s ago
     Docs: man:cron(8)
 Main PID: 20326 (cron)
    Tasks: 1 (limit: 4915)
   CGroup: /system.slice/cron.service
           `-20326 /usr/sbin/cron -f

Feb 13 11:14:10 ubuntu systemd[1]: Started Regular background program processing daemon.
Feb 13 11:14:10 ubuntu cron[20326]: (CRON) INFO (pidfile fd = 3)
Feb 13 11:14:10 ubuntu cron[20326]: (CRON) INFO (Skipping @reboot jobs -- not system startup)
```

6. List all the services running on the system, there should be many.

```bash
systemctl list-units -t service
```


7. Show the only the SSH services that are currently running.

```bash
systemctl list-units -t service | grep -i ssh
```

8. Use the man command to research journalctl.

```bash
man journalctl
```

9. Query journalctl for information on the SSH unit.

```bash
journalctl -u ssh.service
```

10. List the properties of the ssh service.

```bash
systemctl show ssh.service
```

11. Restart the ssh service.

```bash
sudo systemctl restart ssh
```




# 6. Storage

**Description:**

In this section you will learn to:
* Explain and use partitioning
* Explain and configure file systems
* Configure and work with LVM


## 6.1 Partitioning

**Partitions**

A partition is the space on a block storage device allocated
to a file system. A single storage device can contain a
single partition that spans the entire device or multiple
partitions each taking up a portion of the device. Each
partition appears in the OS as a logical device. The
operating system will manage the information contained
within each partition separately.

Some newly manufactured block storage devices, such as
USB thumb drives, come with a partition already defined
and a file system already created. Many newly
manufactured block storage devices such as hard disk
drives (HDDs) and Solid State Drives (SSDs) contain
unallocated free space without partitions or file systems.

Generally speaking, a block storage device must be
partitioned and have a file system installed before an OS
can make use of it. The information on the partition layout
is stored within the storage device's partition table. The OS
reads the partition table before reading any other part of
the storage device. The partition table schemes used in
Linux are the Master Boot Record (MBR) and the GUID
Partition Table (GPT).


**Partitioning the Master Boot Record**

MBR is the original format, dating back to 1993, and is
supported by older hardware. It is limited to 4 primary
partitions. It doesn't support disks or partitions > 2TiB. MBR
requires legacy mode with grub-pc. Grub installs to MBR at
the beginning of the disk, the single point of failure for
partition data.


**Partitioning GUID Partition Table**

The GPT has an unlimited number of primary partitions.
There is a redundant copy of partition info for error
recovery. It supports larger disks and partitions.


**Parted**

`parted` is a disk partitioning and partition resizing program.
It allows you to create, destroy, resize, move, and copy
partitions. It is useful for creating space for new operating
systems, reorganizing disk usage, and copying data to new
hard disks.


## 6.1.1 Partitioning Lab


In this lab you will use various utilities to create partitions
on attached block devices.

1. Use `lsblk` to retrieve a list of all block devices to
include SCSI devices. Filter the output to show only
the name and size. **Note**: The first device is usually
used for the host operating system and should not
be used for any other activity.

```bash
sudo lsblk -a -S -p -o name,size
```

2. Use parted to create a GPT partition table on the
second block device `vdb` listed in step 1.

```bash
sudo parted /dev/vdb mklabel gpt
```

3. Inspect the disk and check for the partition table with the `fdisk` command.
Ypu should see a `Disklabel type: gpt` field.

```bash
sudo fdisk -l /dev/vdb
```


4. Use parted to create a single partition that utilizes
the entire disk on the second block device `vdb`.

```bash
sudo parted /dev/vdb mkpart primary 1 100%
```


5. Use `parted` to show details on the newly created
partition on second block device `vdb`.

```bash
sudo parted /dev/vdb print
```

6. Use parted to create a MBR partition table on the
third block device `vdc` listed in step 1.

```bash
sudo parted /dev/vdc mklabel gpt
```

7. Use parted to create a single partition that utilizes
the entire disk on the third block device `vdc`.

```bash
sudo parted /dev/vdc mkpart primary 1 100%
```

8. Use parted to show details on the newly created
partition on the third block device `vdc`.

```bash
sudo parted /dev/vdc print
```

9. Delete the partition on the third block device.

```bash
sudo parted /dev/vdc rm 1
```


## 6.2 File Systems


**What is a file system?**

A file system contains the methods and data structures that
an operating system uses to keep track of files on a logical
disk or partition. In other words, it controls how the files are
organized on the disk. Before a disk can be used as a
filesystem, it must initialized and have the bookkeeping
data structures written to disk. This is accomplished by
making or creating the filesystem.

Most UNIX filesystem types have a similar general
structure, although the exact details vary quite a bit. The
central concepts are superblock, inode, data block,
directory block, and indirection block. The superblock
contains information about the filesystem as a whole, such
as its size (the exact information here depends on the
filesystem). An inode contains all information about a file,
except its name. The name is stored in the directory,
together with the number of the inode. A directory entry
consists of a filename and the number of the inode, which
represents the file. The inode contains the numbers of
several data blocks, which are used to store the data in the
file. There is space only for a few data block numbers in the
inode, however, and if more are needed, more space for
pointers to the data blocks is allocated dynamically. These
dynamically allocated blocks are indirect blocks; the name
indicates that in order to find the data block, one has to find
its number in the indirect block first.


**ext4 Filesystem**

There are other filesystems but we discuss `ext4` in this
course. The ext4 filesystem is included in Linux kernels as
of 2.6.28. It is one of the most widely used Linux
filesystems today. It is the default root filesystem format
through 18.04. It is a Journaled filesystem with a max
filesystem size of 1 exabyte and a max per file size of 16TB.
It is backward compatible with ext2 and ext3. ext4 has preallocation, 
which means it can allocate space without writing to it.

It also has delayed allocation, which means it can cache
allocation and write to disk when data is flushed to disk. It
also has multiblock allocation, which allows for allocation of
contiguous blocks at once (reduces file fragmentation).

**Creating ext4 filesystem**

```bash
sudo mkfs.ext4 /dev/vdb1
```

When creating filesystems on lvm or md volumes,
`mkfs.ext4` chooses an optimal geometry. The
default mount options are optimal for most users.
Default mount options can be also set in the
filesystem superblock using the `tune4fs` utility.

```bash
sudo tune2fs -o acl /dev/vdb1
```

Options, such as `acl`, `noacl`, `data`, `quota`, `noquota`,
`user_xattr`, `nouser_xattr` from ext3 are included for backward compatibility.


**Mounting ext4 filesystem**

```bash
sudo mount /dev/vdb1 /mnt/mymount
```

`/mnt/mymount` should exist or you can create it.


```bash
sudo mkdir /mnt/mymount
```

The partition can be automounted.


**Mounting filesystems on boot with fstab**

The configuration file `/etc/fstab` contains the necessary
information to automate the process of mounting partitions.
In a nutshell, mounting is the process where a raw
(physical) partition is prepared for access and assigned a
location on the filesystem tree (or mount point). The
`/etc/fstab` file lists partitions that will be mounted at startup
and mounted by non-root users. The format is:

```
<file system> <mount point> <type> <options> <dump> <pass>
```

“File system” can be a partition number, UUID, partition
label, or LVM volume.

`/etc/mtab` lists all mounted partitions, includes all fstab
entries and includes manually mounted partitions.

`sudo mount -a` will mount all unmounted filesystems in
fstab.


**fstab examples**

Mount the second partition on SATA/SCSCI drive 2.

```bash
/dev/vdb2 /boot ext4 defaults 0 2
```

Do the same, using `/dev/disk/by-uuid`. UUID doesn’t change, but the disk number can.

```bash
UUID=1ea4363f-f9ab-45d8-af48-067b05d202b2 /boot ext4 defaults 0 2
```

By label, not mounted at root, mountable by any user.

```bash
LABEL=mystuff /mystuff ext4 noauto,user 0 0
```

## 6.2.1  Filesystems Lab

In this lab you will work with the Linux filesystem.

1. Use `lsblk` to retrieve a list of all block devices to
include SCSI devices. Filer the output to show only
the name and size. **Note**: The first device is usually
used for the host operating system and should not
be used for any other activity.

```bash
sudo lsblk -a -S -p -o name,size
```

2. Format partition 1 on the second block device with Linux ext4 format.

```bash
sudo mkfs.ext4 /dev/vdb1
```

3. Create a mount point named `/mnt/myext4fs` (if it does not already exist).

```bash
sudo mkdir /mnt/myext4fs
```

4. Mount the newly formatted partition to the mount point.

```bash
sudo mount /dev/vdb1 /mnt/myext4fs
```

5. Ensure it is mounted.

```bash
mount | grep vdb
```

6. Use the `df` command to view available space.

```bash
df -h
```

7. Add the newly formatted partition to `/etc/fstab`.

```bash
sudo nano /etc/fstab
```
and add at the end 

```bash
/dev/vdb1   /mnt/myext4fs   ext4    defaults    0 0
```

8. Unmount the newly mounted filesystem.

```bash
sudo umount /dev/vdb1
```

9. Mount all filesystems in fstab.

```bash
sudo mount -a
```

10. Verify that newly formatted partition is mounted.

```bash
mount | grep vdb
```

11. Unmount the filesystem.

```bash
sudo umount /dev/vdb1
```


## 6.3 LVM


LVM stands for Logical Volume Management. It is more
flexible than the traditional method of partitioning a disk
into one or more segments and formatting each partition
with a filesystem. At least one volume group is required.
There must be one or more physical volumes or hard disks.
Logical volumes are similar to partitions on a disk. They
have names rather than numbers. They can span across
multiple disks and they do not have to be physically
contiguous.


![Linux Logical Volume  Manager](./images/lvm.png)


You may have one or more physical volumes (/dev/vdb1 -
/dev/sde1 above). On these physical volumes you create
one or more volume groups (server1). In each volume
group, you can create one or more logical volumes. If you
use multiple physical volumes, each logical volume can be
bigger than one of the underlying physical volumes (as long
as the sum of the logical volumes does not exceed the total
space offered by the physical volumes).

It is good practice to leave some space unused. If needed,
you can enlarge one or more logical volumes later.


**Command Line**

Use `fdisk`, `pvcreate`, `vgcreate`, and `lvcreate` to create a logical volume.

`fdisk` - create partition with LVM type

```bash
fdisk /dev/vdb
```

`pvcreate` - add physical drives as volumes

```bash
pvcreate /dev/vda3 /dev/vda4
```

`vgcreate` - create volume group

```bash
vgcreate ubuntu-vg /dev/vda3 /dev/vda4
```

`lvcreate` - create logical volume on top of volume group

```bash
lvcreate -L 100G -n UB2 ubuntu-vg
```

## 6.3.1 LVM Lab

In this lab you will use various commands to create and extend a LVM volume.

Commands: `pvcreate`, `lvcreate`, `vgcreate`

`vg` - Create a volume group

`pv` - Add physical volume (drives) to LVM

`lv` - Add Logical volumes to LVM


1. Use the `lsblk` command to obtain the device names
of all 10GB block devices attached to the VM.

```bash
sudo lsblk -a -S -p -o name,size | grep 10G
```

2. Use the `dd` command to overwrite any (and all)
information that may be on the first and second
10GB block devices.

```bash
sudo dd if=/dev/zero of=/dev/vdb bs=4096
```

```bash
sudo dd if=/dev/zero of=/dev/vdc bs=4096
```


3. Use the `parted` command to add the GPT label to
the first and second block devices.

```bash
sudo parted /dev/vdb mklabel gpt
```

```bash
sudo parted /dev/vdc mklabel gpt
```

4. Use the `parted` command to create a single
partition that spans the entire device on the first
and second block devices.

```bash
sudo parted /dev/vdb mkpart primary 1 100%
```

```bash
sudo parted /dev/vdc mkpart primary 1 100%
```

5. Install `lvm2` package.

```bash
sudo apt install -y lvm2
```

6. Check for existing LVM volumes.

```bash
sudo vgscan
```

7. Add the two newly created partitions as LVM physical volumes.

```bash
sudo pvcreate /dev/vdb1 /dev/vdc1
```

8. Add the same two partitions to a LVM volume group named `ubuntu-vg`.

```bash
sudo vgcreate ubuntu-vg /dev/vdb1 /dev/vdc1
```

9. Create a LVM logical volume named `lvmdata` that
uses all available space in the volume group `ubuntu-vg`.

```bash
sudo lvcreate -l 100%VG -n lvmdata ubuntu-vg
```

10. Display information on the newly created logical volume. Note the value for LV Path.

```bash
sudo lvdisplay
```

11. Install an ext4 filesystem on the logical volume.

```bash
sudo mkfs.ext4 /dev/ubuntu-vg/lvmdata
```

12. Create a mountpoint for the volume and mount it.

```bash
sudo mkdir /lvmdata
```
```bash
sudo mount /dev/ubuntu-vg/lvmdata /lvmdata
```

13. Check its mounted size.

```bash
df -h /lvmdata
```



# 7. Networking

In this section you will learn to:
* Use basic network commands
* Explain network initialization
* Configure a network

## 7.1 Basic network commands

**Using ip and ethtool**

`ip` and `ethtool` are command-line programs used to inspect
and modify basic network configuration. `ip` manages
interfaces, addresses, and routing. `ethtool` manages low level
device settings. Virtual devices are manageable via
`ethtool`, but generally do not support all facilities. For most
cases, any user may inspect configuration information, but
only the superuser may make modifications.

**ip Command**

The `ip` command provides the ability to inspect and modify the following:
* interface addresses (both L2 and L3)
* interface administrative state (up or down)
* interface parameters (MTU, queueing parameters, etc)
* routing configuration (L3)
* neighbour configuration (L2)
* create, link, and unlink virtual devices (bridges, VLANs, tunnels, etc.)

Command Syntax
`usage: ip [ options ] /subcommand/ [ subcommand arguments ]`

Commonly used subcommands include `address`, `neighbour`, `link`, and `route`.

Commonly used options include:
`-4` Limits actions to IPv4 address family

`-6` Limits actions to IPv6 address family

`-s` Prints statistics, if available. Repeat for more.

`-d` Print extra details, if available.

`-r` Resolve host names in output.



**Protocol address management: ip address**

`ip addr show` displays address information for some or all interfaces. For example:

```bash
ip addr show dev ens2
```

`ip addr add` adds an address (and an automatically generated route for the associated subnet) to an interface.
For example:

```bash
sudo ip addr add 10.1.2.3/16 dev ens2
```

`ip addr del` deletes an address from an interface. It has the
same syntax as add. It removes the specified address (and
any automatically created routes).


**ip Link Command**

`ip link` shows all devices on the machine.

`ip link show` shows device information. It is generally a
subset of `ip addr show`, but it can display device statistics
with `-s` (specifying `-s` twice adds details for errors).
For example:

```bash
ip -s link show dev ens2
```

`ip link add`, `ip link delete` adds or removes a virtual
device. The following command will add a VLAN virtual
device configured logically above `ens2`, with VLAN ID 42.

```bash
sudo ip link add dev vlan42 link ens2 type vlan id 42
```

`ip link set` modifies device-level configuration. The
following command will change the device link layer (MAC) address.

```bash
sudo ip link set dev ens2 address 02:42:43:44:45:56
```


## 7.1.1 ip Lab

In this lab you will use the `ip` command to display some
basic information about your network configuration.

1. List all of the network interfaces available on your student VM.

```bash
ip link show
```

2. List the IP address of your student VM.

```bash
ip addr show ens2
```

3. Add an additional IP address to the network interface `ens2`.

```bash
sudo ip addr add 10.1.2.3/15 dev ens2
```

4. List the IP addresses again to see the newly added IP.

```bash
ip addr show ens2
```

5. Show basic device statistics on network interface `ens2`.

```bash
ip -s link show ens2
```

6. Show more device statistics on network interface `ens2`.

```bash
ip -s -s link show ens2
```

7. Remove the ip address set on step 3.

```bash
sudo ip addr delete 10.1.2.3/15 dev ens2
```

8. Display the system routing table.

```bash
ip route show 
```

9. Show neighbouring networks addresses that the system has cached.

```bash
ip neigh show
```

10. Show the route to `8.8.8.8`

```bash
ip route get 8.8.8.8
```


## 7.2 ethtool Command

Ethernet interfaces are identified by the system using the
naming convention of `ethX` or `ensX`, where X represents a numeric
value. The first Ethernet interface is typically identified as
`eth0` or `ens2`, the second as `eth1` or `ens4`, and all others should move up in
numerical order.



The ethtool command provides the ability to inspect and/or modify:
* physical link settings (speed, duplex, etc.)
* device transmit and receive ring sizes
* device driver name, version, and bus information (inspect only)
* access to low-level device statistics (inspect only)
* device protocol offload and other features


**Display or Modify Physical Link Settings**

`ethtool` with an interface name will display various link settings:

```bash
ethtool ens2
```

**Manage Device TX and RX Rings**

To display device packet transmit and receive ring buffer sizes.

```bash
sudo ethtool -s ens2 speed 1000
ethtool -g ens2
```

To change device ring size, example uses TX ring:

```bash
sudo ethtool -G ens2 tx 1024
```


**Driver and Device Information**

The output shows, in order, the name of the device driver
for this interface, the version of that driver, the version of
firmware (if any), the PCI bus location of the device (empty
for virtual devices), and a set of flags indicating support for
various low-level functions.

```bash
ethtool -i ens2
```

**Display Low-Level Statistics**

Display extended-hardware statistics for a device.

```bash
ethtool -S ens2
```

**Device Features**

Ethtool device protocol offload and other features to show current device feature settings:

```bash
ethtool -k ns20
```

To change current device feature settings:

```bash
ethtool -K ens2 gro on
```

**ifup/ifdown Commands**

Interfaces can be enabled and disabled individually with the `ifup` and `ifdown` commands:

```bash
sudo ifdown ens2
sudo ifup ens2
```



## 7.2.1 ethtool lab

In this lab you will use `ethtool` to display some basic information about your network hardware.


1. Install ethtool

```bash
sudo apt install -y ethtool
```

2. Show basic hardware information on network interface `ens2`.

```bash
ethtool ens2
```

3. Show the driver information for the network interface `ens2`.

```bash
ethtool -i ens2
```

4. Show features of the network device.

```bash
ethtool -k ens2
```

5. Show the MAC (hardware) address of the interface.

```bash
ethtool -P ens2
```


## 7.3 Network Troubleshooting Commands

This section will address some of the troubleshooting
commands that will  help a server administrator diagnose
issues.


**ping, traceroute, mtr**

`ping/ping6` any host by name or IP address is a quick and
simple way to determine if a speci fic remote host is
reachable from the local host. Sometimes networks or
firewalls block ping (ICMP) packets.

```bash
ping 10.0.1.98
ping www.yahoo.com
```

`Traceroute/traceroute6` send packets to a specific
remote host and trace the route. It is useful to determine
the path to get to the remote host and the latencies to each
of the “hops” along the way.

`mtr` combines ping and traceroute and continuously
displays the current statistics of the full path to a remote host.

```bash
ping canonical.com
ping -c 3 ubuntu.com
ping -c 3 -n ubuntu.com
ping -w 2 canonical.com
```

**Traceroute**

With no parameters, except the remote host, traceroute
detects the path to a remote host once, with 3 pings to
each hop along the path.

```bash
traceroute google.com
```

By default, traceroute uses ICMP packets to perform the
trace. It can use TCP, when the `-T` parameter is used, or
UDP, with the `-U` parameter. Using TCP or UDP instead of
ICMP can be useful as some hosts/routers block ICMP
packets, and TCP or UDP more closely resemble normal
network traffic.

It also performs reverse address lookups to display the
hostname for each hop’s IP address. The `-n` parameter can
be used to prevent reverse address lookups and only
display the IP address of each hop.


**mtr**

The `mtr` program displays real time information about the
path to a remote host. With no parameters, except the
remote host name, it displays information about the path to
the remote host. It may take some time after initial start up
to detect the path before displaying anything besides its
header text.

```bash
mtr ubuntu.com
```

As with traceroute, the `-n` parameter can be used to
prevent reverse address lookups. And as with ping, the `-c`
parameter can be used to limit the number of cycles (a
cycle consists of a ping to all hops in the route to the
remote host). Also by default, mtr uses ICMP packets to
perform the trace, just like ping and traceroute. It can use
TCP with the `-T` parameter or UDP wi th the `-u` parameter.

**Netcat**

`nc` is a utility that reads and writes data across network connections, using the TCP or UDP protocol.

To run `nc` in server mode on a specified portl istening for incoming connections:

```bash
nc -l 2389
```

To run `nc` in client mode trying to connect on the port (2389) just opened:

```bash
nc localhost 2389
```

**Dig**

`dig` (domain information groper) is a tool for interrogating
DNS name servers. It performs DNS lookups and displays
the answers that are returned from the queried name
server(s). Most DNS administrators use dig to troubleshoot
DNS problems.

To determine the IP address of a site:

```bash
dig ubuntu.com
```

The `QUESTION SECTION`: of the resulting output displays the
host that was being queried for.

The `ANSWER SECTION`: displays the results of the query.

Beneath that will be the query details (how long the query
took, the DNS server queried, the date/time the query was
run and the resulting message size).


## 7.3.1 Networking Lab

In this lab you will use tools to check/test network connectivity.

1. The traceroute package is not installed by default, so it will need to be installed. Install `traceroute` package.

```bash
sudo apt install -y traceroute
```

2. The most basic test is a ping. Send four pings to `www.google.com`.

```bash
ping -c 4 www.google.com
```

3. Trace the network path to `www.google.com`.

```bash
traceroute www.google.com
```

4. Trace with `mtr` the network path to `www.google.com` and display the ping statistics simultaneously. Use the `q` key to exit `mtr`.

```bash
mtr www.google.com
```

5. Use `man` to research the `dig` command. Try the `t`, `m`, and `x` commands on `www.google.com`.

```bash
dig www.google.com
```

# 8. Process management

In this section you will learn to:
* Define and work with Linux processes
* Explain and manipulate process priority with renice
* Explain and use job control
* Schedule processes with cron and at


## 8.1 Process Administration

**Processes Purpose of User Accounts**

Starting with init, everything your computer does is a
process. Some people call these "jobs" or “units”. Your init
system tracks each process using a number it assigns when
the process begins. This number is called the process ID
(PID).

In UNIX and Linux, everything is treated as a file. So, every
running process has its own numerical subdirectory, named
using its PID. These are found as `/proc/[PID]`. Information
about the process is stored in files and subdirectories of
that [PID] directory.

**Listing Processes**

The `ps` command is useful for listing processes. By itself, it
displays only your user's running processes. That tends to
be a short list.

```
PID   TTY   TIME     CMD
11757 pts/1 00:00:00 bash
12853 pts/1 00:00:00 ps
```


The list includes the process ID, the terminal associated
with the command (TTY), total CPU usage time, and the
command executable name.


To list all correct processes with detailed information, use
the following options:

```bash
ps -ef
```

This list can be overwhelming, but can be piped through
grep when searching for something specific in the output.
There is an even easier way to list all processes of a specific
command, in this case Xorg:

```bash
ps -C sshd
# output 
PID TTY TIME CMD
958 ? 00:00:00 sshd
21929 ? 00:00:00 sshd
21952 ? 00:00:00 sshd
```

Perhaps even more useful is `top`, which provides a
constantly updated display of the running processes in
order. By default, the list is based on what is using the most
CPU cycles. The PID is listed with each entry in the top
display. This makes it easy to find processes that are
behaving badly.

```bash
top -s
```

**Stopping a Process**

If a foreground process is running in your terminal and you
want to stop it, use the standard `Ctrl+c`. Background
processes are not so straightforward to stop. This is where
finding a PID for a running process comes in handy.


**Sending a Signal to a Process**

When you know the PID of a process, you can send it a
signal directly using kill.

```bash
kill option PID
```

The option is optional. If no option is stated, kill sends a
signal to terminate the process cleanly, including flushing
data, cleaning up memory, and closing the PID.

**Kill Options**

Typically, kill options would be used in an order like this, although this is not exhaustive:

`kill / kill -15` - sends a SIGTERM for clean shutdown of the process

`kill -2` - sends a SIGINT to interrupt the process, as with `Ctrl+C`

`kill -9` - sends a SIGKILL, which is a last resort


**Daemons**

Daemons are continuously running background processes.
They are not connected to any terminal. Most are processes
with names ending in `d`.

Examples:

`cron` - runs processes on a schedule

`sshd` - accepts remote shell connections



## 8.2 Background Processes and priority

**Running Processes in the Foreground**

When you enter a command in a terminal, the program has
control over the terminal session and you can only interact
directly with that program. This is called running a process,
or job, in the foreground.

**Running Processes in the Background**

You can run jobs in the background by appending an `&` to
the end of the command.

```bash
programname &
```

When you do, the terminal will output the job number in
brackets fol lowed by the PID.

```
[1] 18531
```
The job is running in the background. You are not actively
interacting with it.


**Listing Running Processes**

To list your user's running jobs, use jobs. The command has some useful parameters:

`-l` includes the PIDS in the list

`-r` lists only running jobs

`-s` lists only stopped jobs


**Moving from Foreground to Background**

If you run a job from the terminal and it is taking a long
time, you may want to switch the job to the background so
that you may do other things while the job completes. To do
this, enter `Ctrl+Z`, which will suspend the job. Then enter
bg to move it to the background.


**Moving from Background to Foreground**

When you are ready to return to interacting with a
background job, use `fg`. By default, this restores the most
recent background job to the foreground. If you have
multiple background jobs running and want to restore a
speci fic job, use the number: `fg %3`.


**Ending a Background Job**

To kill a specific background job without restoring it to the
foreground, you can use kill with the PID of the job. You can
also use the job number from the jobs list: `kill %2`.


**Process Priority**

Some processes are more important to users than others.
You can tell the kernel to either limit the percentage of CPU
time or give a greater priority to a specific process. This is
most useful when dealing with multiple concurrent processes.


**Priority Levels**

By default, all processes start with a priority of 0 (zero).
The range of priority settings available goes from -20, the
highest priority, to 19, the lowest priority.

To start a new process using a non-default priority setting, use nice:

```bash
nice -n 19 commandname
```

The number after `-n` sets the priority for the command you
run. In this example, the process is being run with the
lowest priority.


**Change the Priority of a Process**

To change the priority of a running process, first look up the
PID of the process. Then, use renice:

```bash
renice -10 -p 19365
```

Here, the process with the PID 19365 is given a higher
priority of -10 and will now have higher priority access to
system CPU resources.

You can also use renice to change the priority of all
processes owned by a specific user or group:

```bash
renice 15 -u greedyuser
renice -18 -g website
```

Here, priority is being taken away from `greedyuser` and
given to the `website` group.


## 8.3 Scheduling Processes

There are times when you want to run a process, but not
right now. Maybe you have a backup script that would be
best to run in the middle of the night. Or perhaps you need
to perform a CPU-intensive calculation that will run better in
a couple of hours after other users have logged off of the
server.

**Scheduling Repeated Processes**

Use `cron` when you want to run the same task on a regular
basis. cron is a daemon that is started by the init process. It
reads a configuration file and runs any job listed in the file
at the times specified.


**Defining Jobs to Run**

There are multiple ways to schedule a cron job. First,
individual users have the ability to schedule their own jobs
using `crontab -e`, which opens their crontab file. By using
this command, edits to the user's cron jobs are checked for
syntax before saving. Also, the crontab files are not
intended to be edited directly, but only via the crontab
command.


**Crontab File Syntax**

The crontab file defines one job per line, using the following
information about when and what to run, in this order:

```
Minute
Hour
Day of the month
Month
Day of the week
Command to run
# m h dom mon dow command
1 2 6 * * * rm –rf /home/codemonkey/sandbox/*
```


The first line is a comment, reminding you of the proper
order. The command removes the contents of a sandbox
directory from codemonkey's home directory at 6:12AM on
every day of every month on every day of the week.


**Scheduling System/Root Jobs**

For system jobs, an admin can place a script or executable in one of these directories:

```
/etc/cron.daily
/etc/cron.hourly
/etc/cron.monthly
/etc/cron.weekly
```

The `/etc/crontab` file for the system is configured to check
each of these directories on the frequency shown and run
the contents.


**Using at**

You can schedule a task to run later using at. As with cron,
you do not have to be logged in when the scheduled task
runs. You can schedule one or many jobs in advance.

To use at, you must tell it the time you want your task to run.

```bash
at now + 7 hours or at HH:MM
```

Then, an `at>` prompt will appear and everything you type
until you hit `Ctrl+D` will be used as your scheduled task.


**Managing Scheduled Jobs**

Use `atq` to list your user's scheduled jobs.

```
1 Wed Apr 20 11:22:00 2018 a user
```
This lists the job number, the scheduled time, the queue
identifier, and the username under which the job will run.
Use `atrm` and the job number to remove a scheduled job.


## 8.3.1 Process Management Lab

In this lab you will work on process management.

1. We will need a simple script for a portion of this exercise. The content of the script is displayed below. Use nano to create the file on your machine.

```bash
nano ~/loop.sh
```

```bash
#!/bin/bash

while true
do
    echo "hello world" >> hello.txt
done
```

```bash
chmod 750 ~/loop.sh
```

2. List the processes you own.

```bash
ps -ef | grep ubuntu
```

3. List all processes currently running on the system.

```bash
ps -ef
```

4. Show a continuously updated listing of processes. Use the `q` key to exit it.

```bash
top
```

5. Run the script loop.sh in the foreground.
Hint: Use `./` to run the script.

```bash
./loop.sh
```

6. Exit the process `loop.sh`.

```bash
ctrl+c
```

7. Run the script` loop.sh` in the background.

```bash
./loop.sh &
```

8. Show all jobs running as your user ID. Have the command also include the PID.

```bash
jobs -l
```

9. Stop the job loop.sh.

```bash
kill <PID>
```

10. Run the script `loop.sh` in the background with a CPU priority of 15.

```bash
nice -n 15 ./loop.sh &
```

11. Give the SSH daemon a CPU priority of -5. First get the PID of the `sshd` process (`/usr/sbin/sshd -D`).

```bash
ps aux | grep sshd
```
```bash
sudo renice -5 -p <PID>
```

12. Schedule a recurring user-level job to touch the file `/tmp/mytest.txt` every five minutes. First, open the crontab config file with:

```bash
crontab -e
```

Put this line at the end of the file:

```bash
*/5 * * * * touch /tmp/mytest.txt
```

13. Schedule a job to run at `2:00PM` that will remove any files in `/tmp` that have the file `extension.yaml`.


```bash
sudo apt install at
```

```bash
at 14:00
```

```bash
at> rm /tmp/*yaml
```

To exit the `at` shell hit:

```bash
crtl + d
```

14. List your scheduled jobs.

```bash
atq
```

15. Install and use `htop` for a continuously updated listing of processes. It is similar to `top`. Use the `q` key to exit it.

```bash
sudo apt install -y htop
```
```bash
htop
```

# 9. Backup and Recovery

In this section you will learn to:
* Use compression tools
* Use remote file synchronization
* Configure system logging
* Implement backup and recovery


## 9.1 Using Archiving and Compression Utilities

Compression is a way of reducing the size of files using
different algorithms and mathematical calculations. The
idea of making backups or archiving data generally means
saving it to a secure location, often in a compressed format.
An archive on a Linux server in general has a slightly
different meaning. Usually, it refers to a tar file that
includes multiple files and directories. Basically, a tar file is
a file format that creates a convenient way to distribute,
store, back up, and manipulate groups of related files. Tar
archives are often compressed during the archival process
in order to store data in a more efficient manner.


**gzip Compression**

gzip compressions is fast. It compresses and decompresses
data at a higher rate than other technologies. It is resource
efficient in terms of memory usage during compression and
decompression. It also has high compatibility.

To compress a file:

```bash
gzip myfile
```

To decompress a file, you simply pass the `-d` flag:

```bash
gzip -d myfile.gz
```

To find out more information about the compressed file, you can use the `-l` flag:

```bash
gzip -l myfile.gz
```

**bzip2 Compression**

bzip2 provides better compression than gzip. It has a longer
compression time vs gzip, but it has good decompression
time. It also has higher memory requirements than gzip.

To compress a file:

```bash
bzip2 myfile
```

To decompress pass the `-d` flag:

```bash
bzip2 -d myfile.bz2
```

To alter the compression rate, use flags from -1 (less compression) to -9 (higher compression):

```bash
bzip2 -1 myfile
```

**xz Compression**

xz compression is one of the newest formats. It was first
released in 2009 and uses LZMA2 algorithm. It has a
greater compression ratio, but takes significantly longer to
compress files. It has even higher memory requirements,
but is faster at decompressing than bzip2.

To compress a file:

```bash
xz myfile
```

This produces a file called "myfile.xz"

To decompress use the `-d` flag:

```bash
xz -d myfile.xz
```

For xz, -6 flag is the default, it goes from -0 (fastest) to -9 (highest compression). For best compression, use the `-e` and numeric flags:

```bash
xz -e -9 myfile
```

To list statistics about the compression of the file:

```bash
xz -l myfile.xz
```

## 9.2 Tar archiving

The tar command is short for “tape archiving”. It is very
fast. Files in a tar archive are not compressed, just gathered
together in one file. Tarball is a jargon term for a tar
archive, suggesting "a bunch of files stuck together in a ball
of tar. "

To create an archive:

```bash
tar cvfmyfiles.tar myfile1 myfile2
```
`c` - create a new archive

`v` - verbosely list files which are processed

`f` - following is the archive file name

To extract an archive:

```bash
tar xvf myfiles.tar
```

To list files in a tar archive:

```bash
tar tvf myfiles.tar
```

To extract to a different directory:

```bash
tar xvf /mnt/mydir.tar -C /home/alex/temp
```

**Using TAR with gzip Compression**

These follow the exact same format using different flags for each format. For gzip we use the `-z` flag.

To compress a file:

```bash
tar czvf myfile.tar.gz myfile
```

To list the files inside the compressed archive:

```bash
tar tzvf myfile.tar.gz
```

To decompress an archive:

```bash
tar xzvf myfile.tar.gz
```

**Using TAR with bzip2 Compression**

These follow the exact same format using different flags for each format. For bzip2, we use the `-j` flag.

To compress a file:

```bash
tar cjvf myfile.tar.bz2 myfile
```

To list the files inside the compressed archive:

```bash
tar tjvf myfile.tar.bz2
```


To decompress an archive:

```bash
tar xjvf myfile.tar.bz2
```

**Using TAR with xz Compression**

These follow the exact same format using different flags for each format. For xz, we use the `-J` flag.

To compress a file:

```bash
tar cJvf myfile.tar.xz myfile
```

To list the files inside the compressed archive:

```bash
tar tJvf myfile.tar.xz
```

To decompress an archive:

```bash
tar xJvf myfile.tar.xz
```


## 9.3 Using rsync

Rsync (remote sync) is a command used for copying and
synchronizing files and directories remotely as well as
locally. With rsync, you can copy and synchronize your data
remotely and locally across directories, across disks and
networks, and perform data backups and mirroring between
two machines. It copies and syncs files to or from a remote
system. It supports copying links, devices, owners, groups,
and permissions. It uses compression and decompression
methods while sending and receiving data on both ends.

General syntax:

```bash
rsync options SOURCE DESTINATION
```

Common options:

`-v` verbose

`-r` copies data recursively

`-a` archive mode allows copying files recursively,
preserves symbolic links, file permissions, user &
group ownerships and timestamps

`-z` compress file data

`-P` shows progress

rsync can be used to sync files and directories locally.


Sync a file on a local computer:

```bash
rsync -avP file.tar /path-to/backups/
```

Sync a directory on the local computer:

```bash
rsync -avP /home/user/ /path-to/backups/
```

If the directory was synced before and files were changed
on the source directory, you may use the `--delete` option to
make sure that the directories are in sync every time you
run the command:

```bash
rsync -avP --delete /home/user/ /path-to/backups/
```

Sync files and directory to or from a server. Copy a directory from local computer to a remote server:

```bash
rsync -azvP /home/user/ user@10.10.10.10:/pathto/backup/
```

Sync a remote directory to the local computer:

```bash
rsync -azvP user@10.10.10.10:/path-to/backup/ /tmp/local/
```

Since we’re transferring data through the network or over
the Internet, we’ve used the `z` option. This will compress
data during transfer and save some bandwidth.

```
-z, --compress compress file data during the transfer
--compress-level=NUM explicitly set compression level
```


**Using rsync with ssh**

We can use SSH (Secure Shell) with rsync for data transfer
ensuring your data is being transferred in a secured
manner.

To copy a file from a remote server locally: specify a protocol with rsync with `-e`:

```bash
rsync -azvPe ssh user@10.10.10.10:/path-to/file.txt /tmp/
```

To copy a file from the local machine to a remote location:

```bash
rsync -azvPe ssh file.txt user@10.10.10.10:/pathto/backups/
```


**Using --include and --exclude options**

These two options allow to include and exclude files by
specifying parameters. In this example, rsync will include
files and directories that start with 'A' and exclude all other
files and directories:

```bash
rsync -azvPe ssh --include 'A*' --exclude '*' user@10.10.10.10:/path-to/files /tmp
```

**Setting the maximum size of files to be transferred**

In this example, the maximum file size is 500k, so we’ll only transfer logs that are equal or smaller than 500k.

```bash
rsync -azvPe ssh --max-size='500k' 10.10.10.10:/var/log/ /tmp/
```

Automatically delete source files after successful transfer

```bash
rsync -azvP --remove-source-files file.txt /tmp/
```

Dry run with rsync

```bash
rsync -azvP --dry-run file.txt /tmp/
```

Set bandwidth limit transfer (KBytes per second)

```bash
rsync --bwlimit=100 -azvPe ssh /path-to/files/ user@10.10.10.10:/path-to/backup/
```

## 9.4 Backup and Recovery Lab

In this lab you will work wi th backups. You will use the tar
command to create, list, add/update, delete, extract, and
compress using gzip (Create/Extract).


1. Use the man command to see the options for the tar command.

```bash
man tar
```

2. Create a new tar file of the `/etc` directory in `/tmp/myetc.tar`.

```bash
sudo tar cf /tmp/myetc.tar /etc
```

3. List all the files in the just created tar file.

```bash
tar tf /tmp/myetc.tar 
```

4. Add `/root/.bashrc` to the existing tar file `/tmp/myetc.tar`.

```bash
sudo tar uvf /tmp/myetc.tar /root/.bashrc
```

5. Verify `root/.bashrc` is now in `/tmp/myetc.tar`.

```bash
tar tf /tmp/myetc.tar | grep root/.bashrc
```

6. Delete `root/.bashrc` from the tar file `/tmp/myetc.tar` and verify it is no longer contained in the archive.

```bash
sudo tar --delete -f /tmp/myetc.tar root/.bashrc
```
```bash
tar tf /tmp/myetc.tar | grep root/.bashrc
```

7. Extract `etc/hostname` from the tar file `/tmp/myetc.tar` into `/tmp`.

```bash
cd /tmp
```
```bash
tar xvf myetc.tar etc/hostname
```

8. Extract all contents from the tar file `/etc/myetc.tar` into `/tmp/tar`.

```bash
mkdir /tmp/tar && cd /tmp/tar
```
```bash
tar xvf /tmp/myetc.tar
```

9. Compress the tar file `/tmp/myetc.tar` with gzip.

```bash
cd /tmp
```
```bash
sudo gzip myetc.tar
```

10. Create another tarfile of the `/etc` directory compressing the archive as it is created.

```bash
sudo tar czvf /tmp/myetc2.tgz /etc
```

11. Extract the contents of `/tmp/myetc2.tgz` into `/tmp/tgz`.

```bash
mkdir /tmp/tgz && cd /tmp/tgz
```
```bash
tar xzvf /tmp/myetc2.tgz
```

12. Clean up the files and directories created/used in `/tmp`.

```bash
cd /tmp
```
```bash
sudo rm -rf tar tgz etc myetc.tar.gz myetc2.tgz
```
# 10. Software Management

**Description:**

In this section you will learn to:
* Explain & perform Debian package management
* Perform package management with `apt`
* Perform package management with Snappy


## 10.1 Debian Package Management

**Why Packages?**

Providing software using packages makes it easier to:
* Install and remove software
* Manage dependencies
* Prevent conflicts
* Maintain a list of what software exists on the
system along with where and how that software is
configured

Software that is provided in the Ubuntu software archives
exists in two package forms:
* `deb` packages are binary archive files that contain
the executable for the software as well as
configuration files, copyright information, and
documentation.
* `dsc` packages contain original program source code
and information about any changes made to that
source.


**Basic Package Management**

`dpkg` is a tool to install, build, remove, and manage Debian
package files. It reads the install instructions in a created
file and deals with them appropriately to configure the
software and packs/unpacks the files to install, remove, or
manage software. Package management must often be run
with admin privileges, as with root or with sudo.


**Common dpkg Use**

`dpkg -i packagename.deb` - installs the named package

`dpkg -r packagename.deb` - removes the named package and all its files, except for configuration files

`dpkg -P packagename.deb` - purges an installed or already removed file to remove any remaining files, including configuration files

`dpkg -l` - lists packages matching the name pattern

`dpkg -L package-name` - lists files installed to your system from package

`dpkg -S filename-search-pattern` - searches for a filename from installed packages


## 10.2 Advanced Package Tool (Apt)

**Advanced Package Management**

Managing software using packages and `dpkg` is much faster
and easier than compiling from source, which was the
previous method. Even better is `apt`, which adds automatic
dependency management and easier upgrading that takes
care of migrating or updating existing configurations during
the process.


**Software Repositories**

With apt, packages are kept in software repositories.
Ubuntu maintains several software repositories and
systems are easily configured to use or ignore specific
repositories. The list of repositories is kept in
`/etc/apt/sources.list` and in files in
`/etc/apt/sources.list.d/`.


**Ubuntu Repositories**

These are the main four Ubuntu software repositories:

`main` - officially supported free software

`restricted` - officially supported software that does not have a completely free license

`universe` - community-maintained software

`multiverse` - community-maintained software that does not have a completely free license


**Personal Package Archives**

Some developers and companies choose to maintain their
own software repositories. Ubuntu offers the ability to set
up Personal Package Archives (PPAs) on Launchpad. These
can offer more frequent updates to software than official
repositories.


**Adding and Removing Repositories**

While it is possible to edit the sources.list file directly, the
best method of adding and removing repositories and PPAs
from your system's configuration is to use `apt` commands.

```bash
apt-add-repository "https://pathtorepo.net main"
apt-add-repository ppa://shinysoft/devel
apt-add-repository --remove ppa://shinysoft/devel
```


**Updating and managing Packages**

Any time you add or remove a repository, as well as any
time you plan to install or upgrade software packages, you
should first refresh the list of available packages using:


```bash
apt update
```

To install a package, along with any of its dependencies:

```bash
apt install packagename
```

To remove a package (but not package dependencies):

```bash
apt remove packagename
```

To remove package dependencies for removed packages:

```bash
apt autoremove
```

Removing a package does not automatically remove that
package's configuration files. This is intentional in case you
want to reinstall the package later. Remove all files,
including configurations using:

```bash
apt purge packagename
```


**Upgrading Installed Packages**

To upgrade existing installed packages with newer versions
from the repositories use:

`apt upgrade` - installs newer versions of installed
packages so the package manager knows about
available updates for installed software.

`apt dist-upgrade` - performs an apt upgrade and
handles changing dependencies with new versions
of packages. It will attempt to upgrade the most
important packages at the expense of less
important ones if necessary.

`apt full-upgrade` - upgrades, but will remove currently
installed packages if this is needed to upgrade the
system as a whole.


**Package Management with Menus**

For a menu-driven experience when dealing with packages,
use `aptitude`. Aptitude can also be used as a command line
program like apt. Removing a package with Aptitude will
auto-remove the dependencies. On CLI : `apt autoremove`.



## 10.3 Snappy Package Management

**Snap**

Ubuntu has a new package format called "snap". This is not
a replacement for Deb packages and traditional package
management, but rather an additional option. Snaps are
intended to be used by application developers to deliver
software updates directly to users. They also have several
use case advantages.

Snaps help developers with a simple packaging format, a
streamlined process including all dependencies in the
package rather than by linking to shared libraries and other
packages that may change, and transactional updates,
where just the delta needs to be downloaded.


**Contents of Snaps**

A snap package consists of the software and all its
dependencies. This provides isolation between apps,
enables an app store model for finding and managing
software, and eliminates the packaging bottleneck that
exists with traditional software repositories.


**Managing snaps**

To install a snap package use:

```bash
snap install packagename
```


To find what snaps are available use:

```bash
snap find
```

To narrow the list down use:

```bash
snap find searchterm
```

To learn what snaps are currently installed use:

```bash
snap list
```

To update to the latest release of a snap use:

```bash
snap refresh packagename
```

To remove an installed snap use:

```bash
snap remove packagename
```

## 10.3.1 Software Management Lab

In this lab you will use package management tools.

1. Show the versions of the MAAS package that are available.

```bash
apt-cache policy maas
```

2. Add the MAAS development PPA `ppa:maas/next`.

```bash
sudo apt install -y software-properties-common
```

```bash
sudo apt-add-repository ppa:maas/next
```

3. Update the package list.

```bash
sudo apt update
```

4. Show the versions of the MAAS package that are
available. Notice the difference in the available MAAS
versions.

```bash
apt-cache policy maas
```

5. Remove the MAAS development PPA from the source list.

```bash
sudo apt-add-repository --remove ppa:maas/next
```

6. Install and configure the package `nullmailer` (use default values).

```bash
sudo apt install -y nullmailer
```

7. Show the status of the nullmailer package. Notice the ii (Desired/Status) values.

```bash
dpkg -l nullmailer
```

8. Remove (NOT purge) the nullmailer package.

```bash
sudo apt remove -y nullmailer
```

9. Show the status of the nullmailer package. Notice the rc (Desired/Status) values.

```bash
dpkg -l nullmailer
```

10. Purge the nullmailer package.

```bash
sudo apt purge -y nullmailer
```

11. Show the status of the nullmailer package. You should
be informed that no packages were found matching
nullmailer.

```bash
dpkg -l nullmailer
```

12. Let's say you want to search for an available application/package. Search for `lynx`. Lynx is a text-mode web client used to browse the web from CLI.

```bash
apt-cache search lynx
```

13. Check the information about this specific package.

```bash
apt-cache show lynx
```

14. We'll use Snappy from now on, install it. Please note that it may already be installed.

```bash
sudo apt install snapd
```


15. See the installed apps.

```bash
snap list
```

16. Search for the `hello-world` snap. It is a testing app for snaps.

```bash
snap find hello-world
```

17. Install it.

```bash
sudo snap install hello-world
```

18. And now run the `hello-world` program.

```bash
export PATH="$PATH:/snap/bin"
```

```bash
hello-world
```

19. Update the app to the latest version.

```bash
sudo snap refresh hello-world
```

20. Finally, remove the app.

```bash
sudo snap remove hello-world
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







