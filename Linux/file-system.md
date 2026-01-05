**[Linux Storage Layout]{.underline}**

![](media/image1.png){width="6.268055555555556in"
height="1.6194444444444445in"}

![](media/image2.png){width="6.268055555555556in"
height="4.472222222222222in"}

![](media/image3.png){width="6.268055555555556in"
height="3.497916666666667in"}

![](media/image4.png){width="6.268055555555556in" height="3.5375in"}

![](media/image5.png){width="6.268055555555556in"
height="2.9944444444444445in"}

**2. Agenda and Learning Goals**

**This unit will cover:**

- Linux file system fundamentals

- Your home directory

- Absolute and relative paths

- What are block devices, specifically STDOUT, STDIN, STDERR

**At the end of this unit you'll be abe to:**

- Understand Linux files better

- Experimenting with different file types

- Inspect files using common and useful commands

- Improve your orientation in the file system by understanding paths

# 3. File System Fundamentals

**"On a Linux system, everything is a file; if something is not a file,
it is a process."**

In linux, under the hood, everything is actually a **file**. A text file
is a file, a directory is a file, your keyboard is a file (one that the
system reads from only), your monitor is a file (one that the system
writes to only), when you want to send data over the internet, you write
it to a unique file, etc.

How could that statement be true? because there are **special
files** that are more than just files (e.g. sockets, devices etc.).

myuser@hostname:\~\$ ls -l

total 80

-rw-rw-r\-- 1 myuser myuser 31744 Feb 21 17:56 intro Linux.doc

-rw-rw-r\-- 1 myuser myuser 41472 Feb 21 17:56 Linux.doc

drwxrwxr-x 2 myuser myuser 4096 Feb 25 11:50 course

In the above output, the first dash (-) represents the file type. We can
notice that course is a directory, since the first dash is
d: drwxrwxr-x, while Linux.doc is a regular file, since it starts with
-.\
Here are a few common types in Linux OS:

  -----------------------------------------------------------------------
  **Symbol**                 **Meaning**
  -------------------------- --------------------------------------------
  \-                         Regular file

  d                          Directory

  l                          Link

  c                          Character device

  s                          Socket

  p                          Named pipe

  b                          Block device
  -----------------------------------------------------------------------

In this course, we will deal with plain files, executable files,
directories, links, and a bit of sockets.

Another important feature of the linux file system (**fs**): filename is
Case Sensitive, and files has no extension, use the file command to know
the content type:

myuser@hostname:\~\$ touch file1.png

myuser@hostname:\~\$ echo \"hi\" \> file1.png

myuser@hostname:\~\$ ls

file1.png

\...

myuser@hostname:\~\$ file file1.png

file1.png: ASCII text

myuser@hostname:\~\$ file File1.png

File1: ERROR: cannot open \'File1.png\' (No such file or directory)

In the above example, we used the touch command to create an empty file
called file1.png, and the text "hi" was written into it (this command
uses the \> operator which will be discussed later on). Then we use
the file command to inspect the type of the file. We can see that even
though the file extension is .png (which is known for images), linux
recognizes the file type as a regular text file, which is the correct
type. In linux OS, file extensions are meaningless.

# 4. Spot Check

1.  Why did the last command (file File1.png) produce an error?

2.  List at least 2 directories, 2 links, 2 regular files
    in /etc directory. You can list all directory content by ls -l /etc.

Solution

1.  Because Linux is case sensitive, File1.png doesn't
    equal file1.png (mind the capital F).

2.  a\. Directories - /etc/sysctl.d and /etc/apt\
    b. Regular files - /etc/passwd and /etc/group\
    c. Links - /etc/localtime and /etc/resolv.conf

# 5. User Home Directory

In Linux, each user has a HOME directory which serves as their default
working directory when they log in. This directory contains the user's
personal files and settings, and is typically located
at /home/\<username\>, while \<username\> is the username. The HOME
directory is protected by file permissions to ensure that only the user
and authorized system administrators can access it.

Here are a few ways to access to a user's home directory in Linux:

1.  Using the tilde (\~) character. Simply use the command cd
    \~ or cd to change to your own home directory.

2.  Using the absolute path, typically /home/username.

3.  Using the \$HOME variable: Linux also has a built-in **environment
    variable** called \$HOME, which contains the path to the current
    user's home directory. You can use the command cd \$HOME to change
    to your own home directory (Variables will be discussed later on).

# 6. File Path

The file system under linux has an hierarchical structure. At the very
top of the structure is what's called the **root directory**. It is
denoted by a single slash (/). It has subdirectories, they have
subdirectories and so on. Files may reside in any of these directories.\
A **path** is the reference of a particular file or directory on the
system.\
There are 2 types of paths we can use, **Absolute** and **Relative**. We
can refer to a given file either by its Absolute or Relative path, to
our choice.

- Absolute paths specify a location (file or directory) in relation to
  the root directory, they always begin with a forward slash (/).

- Relative paths specify a location in relation to the current working
  directory.

myuser@hostname:\~\$ pwd

/home/myuser

myuser@hostname:\~\$ ls Documents

file1.txt file2.txt file3.txt

\...

myuser@hostname:\~\$ ls /home/myuser/Documents

file1.txt file2.txt file3.txt

\...

The above example shows two different ways to reference
the Documents dir. The first is relatively the current working directory
(which is \~), the second is using absolute path.\
A few notes regarding paths:

- \~ (tilde) - is a shortcut for your home dir. e.g. if your home
  directory is /home/myuser then you could refer to the directory
  Documents with the path /home/myuser/Documents or \~/Documents.

- . (dot) - is a reference to your current working directory. e.g. in
  the example above we could also refer to Documents by ./Documents.

- .. (dotdot) - is a reference to the parent directory.

Let's see it in action:

myuser@hostname:\~\$ pwd

/home/myuser

myuser@hostname:\~\$ ls \~/Documents

file1.txt file2.txt file3.txt

\...

myuser@hostname:\~\$ ls ./Documents

file1.txt file2.txt file3.txt

\...

myuser@hostname:\~\$ ls /home/myuser/Documents

file1.txt file2.txt file3.txt

\...

myuser@hostname:\~\$ ls ../../

bin boot dev etc home lib var

\...

myuser@hostname:\~\$ ls /

bin boot dev etc home lib var

\...

# 7. Spot Check

1.  Type cd \~ to change the current working dir to your home directory.

2.  Use the cat command to print the content of /etc/passwd in 2 ways -
    using absolute path, and relative path.

3.  Try to think about a general principle regarding when to use
    absolute or relative paths.

cat ../../etc/passwd

cat /etc/passwd

Use absolute paths when you need to refer to a file or directory from
anywhere in the file system, when you are not sure what will be the
current working directory where the command is executed from.

Use relative paths when you need to refer to a file or directory that is
located in the same directory or a subdirectory of the current working
directory.

# *8. Important Directories*

Here is a list of important files and directories in Linux that users
should be familiar with:

  -----------------------------------------------------------------------------
  **Directory**   **Meaning**
  --------------- -------------------------------------------------------------
  /bin            Common programs, shared by the system, the system
                  administrator and the users.

  /dev            Contains references to all the CPU peripheral hardware, which
                  are represented as files with special properties.

  /etc            Most important system configuration files are in /etc, this
                  directory contains data similar to those in the Control Panel
                  in Windows.

  /home           Home directories of the common users.

  /lib            Library files, includes files for all kinds of programs
                  needed by the system and the users.

  /proc           A virtual file system containing information about system
                  resources.

  /root           The administrative user's home directory. Mind the difference
                  between /, the root directory and /root, the home directory
                  of the root user.

  /tmp            Temporary space for use by the system, cleaned upon reboot.

  /usr            Programs, libraries, documentation etc. for all user-related
                  programs.

  /var            Storage for all variable files and temporary files created by
                  users, such as log files, space for temporary storage of
                  files downloaded from the Internet.
  -----------------------------------------------------------------------------

# 9. Block Devices and Standards Streams

Let's take a closer look on the /dev directory:

myuser@hostname:\~\$ ls -l /dev

brw-rw\-\-\-- 1 root disk 8, 0 Apr 1 18:30 sda

brw-rw\-\-\-- 1 root disk 8, 1 Apr 1 18:30 sda1

brw-rw\-\-\-- 1 root disk 8, 2 Apr 1 18:30 sda2

...

lrwxrwxrwx 1 root root 15 Apr 1 18:29 stderr -\> /proc/self/fd/2

lrwxrwxrwx 1 root root 15 Apr 1 18:29 stdin -\> /proc/self/fd/0

lrwxrwxrwx 1 root root 15 Apr 1 18:29 stdout -\> /proc/self/fd/1

We will discuss some important files - block devices.

Note the files sda, sda1, sda2. Those are block device file type (the
first dash is b).

Device files do not contain data in the same way that regular files, or
even directories. Instead, the job of a device node is to act as an
interface to a particular device driver within the kernel.

When a user writes to a device node, the device node transfers the
information to the appropriate device driver in the kernel. When a user
would like to collect information from a particular device, they read
from that device's associated device node, just as reading from a file.

Block devices are devices that read and write information a chunk
("block") at a time. Block devices customarily allow random access,
meaning that a block of data could be read from anywhere on the device,
in any order.

In Linux, "sda," "sda1," and "sda2" are devices that refer to
different **partitions** of a storage device, such as a hard drive or
SSD.\
The "sda" device refers to the entire storage device, while "sda1" and
"sda2" are partitions of that device.

- "sda1" is the first partition on the "sda" device

- "sda2" is the second partition on the "sda" device

- etc...

myuser@hostname:\~\$ lsblk

sda 8:0 0 465.8G 0 disk

├─sda1 8:1 0 487M 0 part /boot

└─sda2 8:5 0 465.3G 0 part

We will now discuss other important files: stdin, stdout, stderr.\
Those are the standard input/output streams that are used by programs to
read input (from your keyboard) and write output (to your screen).\
Here's what each stream does:

1.  **Standard Input (stdin)**: This is the stream that carries input to
    a program. By default, it is associated with the keyboard, so when a
    user types something into the terminal, it is sent to the program
    via the standard input file.

2.  **Standard Output (stdout)**: This is the stream that carries normal
    output from a program. By default, it is associated with the
    terminal, so when a program prints something to the console, it is
    sent to the standard output file.

3.  **Standard Error (stderr)**: This is the stream that carries error
    messages and other diagnostic output from a program. By default, it
    is also associated with the terminal, so when a program encounters
    an error or warning, it prints a message to standard error.

By default, these streams are connected to the terminal, but they can be
redirected to files or other streams as well. This is a powerful feature
of the Unix shell that allows programs to be combined and orchestrated
in powerful ways.

Do you see how **"On a Linux system, everything is a file"**. Keep in
mind this statement, it'll help you to understand linux's behavior.

# 10. Exercises

### Exercise 1 - Know Your System

Change directory to /proc.

1.  What CPU(s) is the system running on?

2.  How much RAM does it currently use?

3.  How much swap space do you have?

4.  What drivers are loaded?

5.  How many hours has the system been running?

6.  Which filesystems are known by your system?

Change to /etc

1.  How long does the system keep the log file in which user logins are
    monitored?

2.  How many users are defined on your system? Don't count them, let the
    computer do it for you using wc!

3.  How many groups do you have?

4.  Which version of bash is installed on this system?

5.  Where is the time zone information kept?

myuser@hostname:\~\$ cd /proc

\# a solution

myuser@hostname:/proc\$ cat cpuinfo

\# b solution

myuser@hostname:/proc\$ cat meminfo

\# c solution

myuser@hostname:/proc\$ cat swaps

\# d solution

myuser@hostname:/proc\$ cat modules

\# e solution

myuser@hostname:/proc\$ cat uptime

\# f solution

myuser@hostname:/proc\$ cat filesystems

myuser@hostname:/proc\$ cd /etc

\# g solution

myuser@hostname:/etc\$ cat logrotate.conf

\# h solution

myuser@hostname:/etc\$ cat passwd \| wc -l

\# i solution

myuser@hostname:/etc\$ cat group

\# j solution

myuser@hostname:/etc\$ bash \--version

\# k solution

myuser@hostname:/etc\$ ls -l localtime

### Exercise 2 - Binary Numbers

1.  Convert the following binary numbers to decimals: 111, 100, 10110.

2.  What is the available decimal range represented by an 8 bits binary
    number?

3.  Given a 9 bits binary number, suggest a method to represent negative
    numbers between 0-255.

4.  Suggest a method to represent floating point numbers (e.g. 12.3,
    15.67, 0.231) using 8 bits binary numbers.

> Solution

1.  To convert a binary number to decimal, you can use the following
    method: Starting from the rightmost digit of the binary number,
    multiply each digit by two to the power of its position, counting
    from 0 for the rightmost digit. Then, sum the results of these
    multiplications to obtain the decimal equivalent of the binary
    number.\
    For example:

    - 111 in binary is equal to: 1 \* 2\^2 + 1 \* 2\^1 + 1 \* 2\^0 = 4 +
      2 + 1 = 7 in decimal.

    - 100 in binary is equal to: 1 \* 2\^2 + 0 \* 2\^1 + 0 \* 2\^0 = 4 +
      0 + 0 = 4 in decimal.

    - 10110 in binary is equal to: 1 \* 2\^4 + 0 \* 2\^3 + 1 \* 2\^2 + 1
      \* 2\^1 + 0 \* 2\^0 = 16 + 0 + 4 + 2 + 0 = 22 in decimal.

2.  An 8-bit binary number can represent a range of decimal numbers from
    0 to 255 (2\^8 - 1). This is because there are 8 digits (or bits) in
    the binary number, and each digit can have one of two possible
    values (0 or 1), giving a total of 2\^8 (256) possible combinations
    of digits.

3.  One common method for representing negative numbers is using the
    leftmost bit to represent the sign - 0 for positive, 1 for negative.

4.  First 4 bits for the integer part and the rest 4 bits for the
    fractional part.

### Exercise 3 - Kernel System Calls

The Linux Kernel was presented in our first linux lecture - the main
component of a Linux OS which functions as the core interface between a
computer's hardware and its applications.

![](media/image6.png){width="2.7395833333333335in" height="1.875in"}

Then we moved to learn how to use the Terminal and communicate with the
OS using commands such as ls or chmod.

But how does it really work? We type a command and hit the Enter key,
then what happens? This question tries to investigate this point.

Under the hood, linux commands are compiled C code (get to know what a
compilation process is if you don't know...). The C code
contains **system calls**. The system call is the fundamental interface
between an application and the Linux kernel.

In simple words, when your application wants to use the hardware (e.g.
calculate something in the CPU, or write data to the disk), you create a
system call to the Linux Kernel, and the linux kernel talks with the
hardware on your behalf. Read more about what System Calls are.

strace is a Linux command, which traces system calls and signals of a
program. It is an important tool to debug your programs in advanced
cases. In this assignment, you should follow the strace output of a
program in order to understand what exactly it does (i.e. what are the
system calls of the program to the kernel). You can assume that the
program does only what you can see by using strace.

To run the program, open a linux terminal in an empty directory and copy
the file strace_ex/whatIdo from our shared repository, to your current
working directory. :

1.  Give the whatIdo file an exec permission (make sure you don't get
    Permission denied when running it).

2.  Run the program using strace: strace ./whatIdo.

3.  Follow strace output. Tip: many lines in the beginning are part of
    the load of the

4.  program. The first "interesting" line comes only at the end of the
    output.

5.  Try to get a general idea of what this program does by observing the
    sys calls and the directory you've run the program.

Solution

The program creates a directory called welcomeToDevOpsBootcampElevation.
Under this directory, the program creates the file goodLuck and writes
"There you go... tell me what I do..." in it.

# 

# 

# *11. Comprehension Check*

Welcome to the Comprehension Check. In this section, you will be
applying what you learned and choosing the best answer.

### Question 1

Which of the following is an absolute directory reference?

![](media/image7.wmf)

/home/student

![](media/image8.wmf)

../etc

![](media/image9.wmf)

..

![](media/image10.wmf)

home/myuser

Answer : 1

### Question 2

Which of the following is a relative directory reference?

![](media/image11.wmf)

/home/student

![](media/image12.wmf)

/etc

![](media/image13.wmf)

..

![](media/image14.wmf)

\~

Answer : 3

### Question 3

Which of the following could have been displayed by pwd?

![](media/image15.wmf)

home/student

![](media/image16.wmf)

/etc

![](media/image17.wmf)

..

![](media/image18.wmf)

\~

Answer : 2

### Question 4

Following the command cd \~, which is the most likely result from pwd?

![](media/image19.wmf)

/home/myuser

![](media/image20.wmf)

/etc

![](media/image21.wmf)

..

![](media/image22.wmf)

\~

Answer : 1

### Question 5

The file named.conf is a system configuration file. This file belongs in

![](media/image23.wmf)

/tmp

![](media/image24.wmf)

/etc

![](media/image25.wmf)

/bin

![](media/image26.wmf)

/home/myuser

Answer : 2

### Question 6

The file e2fsck is a privileged command that must always be available to
the system and being used by the system admin. This file would be found
in

![](media/image27.wmf)

/tmp

![](media/image28.wmf)

/etc

![](media/image29.wmf)

/var/lib

![](media/image30.wmf)

/sbin

Answer : 4

### Question 7

Which of the following commands could not be used to create a file
in /tmp?

![](media/image31.wmf)

touch /newfile

![](media/image32.wmf)

touch /tmp/newfile

![](media/image33.wmf)

touch ../newfile

![](media/image34.wmf)

touch ../tmp/newfile

Answer :1

### Question 8

Use the file command to help answer the question.

What type of file is /dev/log?

![](media/image35.wmf)

A character special file

![](media/image36.wmf)

A block special file

![](media/image37.wmf)

A socket

![](media/image38.wmf)

A symbolic link

![](media/image39.wmf)

A compiled ELF object

Answer : 4

### Question 9

Which of the following commands would display the first 5 lines of the
file /etc/passwd?

![](media/image40.wmf)

head -5 /etc/passwd

![](media/image41.wmf)

head -n /etc/passwd

![](media/image42.wmf)

head \--five /etc/passwd

![](media/image43.wmf)

head /etc/passwd \> 5

![](media/image44.wmf)

head /5 /etc/passwd

Answer : 1

### Question 10

What type of file is /usr/bin/md5sum?

![](media/image45.wmf)

A compiled ELF object

![](media/image46.wmf)

An Awk script

![](media/image47.wmf)

A Bash (Bourne-Again) shell script

![](media/image48.wmf)

A Symbolic link

![](media/image49.wmf)

A /usr/bin/perl script

Answer : 1

### Question 11

Given the path /home/username/secret.txt, choose the correct sentence:

![](media/image50.wmf)

secret.txt is a file

![](media/image51.wmf)

secret.txt is a directory

![](media/image52.wmf)

secret.txt can be either a file or a directory

Answer : 3
