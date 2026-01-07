###Linux Storage Layout

Black Storage Layout

The standard linux file systems organize storage on hard disk drives. Disks are usually accessed in physical blocks, rather than a byte (8 bits) at a time. Block sizes may range from 512 bytes to 4K or larger.
A file is represented by an inode, a kind of a serial number containing information about the actual data that makes up the file: to whom this file belongs, and where is it located on the hard disk.

In Linux, files are stored on disk using **inodes** and **data blocks**.
The filename itself does **not** contain the file data — it only points to an inode.

###Example Commands

bash
>>ls -i file1
Output:
2372244 file1

The number 2372244 is the inode number of file1.

>>stat file1
File: file1
Size: 13            Blocks: 8       IO Block: 4096   regular file
Device: fd01h/64769d  Inode: 2372244   Links: 1
Access: (0664/-rw-rw-r--)  Uid: (1000/myuser)  Gid: (1000/myuser)

"On a UNIX system, everything is a file; if something is not a file, it is a process"

This unit will cover:
•	Linux file system fundamentals
•	Your home directory
•	Absolute and relative paths
•	What are block devices, specifically STDOUT, STDIN, STDERR

At the end of this unit you’ll be abe to:
•	Understand Linux files better
•	Experimenting with different file types
•	Inspect files using common and useful commands
•	Improve your orientation in the file system by understanding paths

--------------------------------------------------------------------------------------

File System Fundamentals

“On a Linux system, everything is a file; if something is not a file, it is a process.”
In linux, under the hood, everything is actually a file. A text file is a file, a directory is a file, your keyboard is a file (one that the system reads from only), your monitor is a file (one that the system writes to only), when you want to send data over the internet, you write it to a unique file, etc.
How could that statement be true? because there are special files that are more than just files (e.g. sockets, devices etc.).
>>myuser@hostname:~$ ls -l
total 80
-rw-rw-r--  1 myuser     myuser   31744 Feb 21 17:56 intro Linux.doc
-rw-rw-r--  1 myuser     myuser   41472 Feb 21 17:56 Linux.doc
drwxrwxr-x  2 myuser     myuser   4096  Feb 25 11:50 course

In the above output, the first dash (-) represents the file type. We can notice that course is a directory, since the first dash is d: drwxrwxr-x, while Linux.doc is a regular file, since it starts with -.
Here are a few common types in Linux OS:

Symbol	Meaning
-	      Regular file
d	      Directory
l	      Link
c	      Character device
s	      Socket
p	      Named pipe
b	      Block device

--------------------------------------------------------------------------------------

Another important feature of the linux file system (fs): filename is Case Sensitive, and files has no extension, use the file command to know the content type:
>>myuser@hostname:~$ touch file1.png
>>myuser@hostname:~$ echo "hi" > file1.png
>>myuser@hostname:~$ ls
file1.png
...
>>myuser@hostname:~$ file file1.png
file1.png: ASCII text
>>myuser@hostname:~$ file File1.png
File1: ERROR: cannot open 'File1.png' (No such file or directory)

In the above example, we used the touch command to create an empty file called file1.png, and the text “hi” was written into it (this command uses the > operator which will be discussed later on). 
Then we use the file command to inspect the type of the file. We can see that even though the file extension is .png (which is known for images), linux recognizes the file type as a regular text file, which is the correct type. In linux OS, file extensions are meaningless.

--------------------------------------------------------------------------------------

Spot Check
1.	Why did the last command (file File1.png) produce an error?
2.	List at least 2 directories, 2 links, 2 regular files in /etc directory. You can list all directory content by ls -l /etc.
Solution
1.	Because Linux is case sensitive, File1.png doesn’t equal file1.png (mind the capital F).
2.	a. Directories - /etc/sysctl.d and /etc/apt
b. Regular files - /etc/passwd and /etc/group
c. Links - /etc/localtime and /etc/resolv.conf

------------------------------------------------

User Home Directory






































































