#Linux

## Important Commands

### Overview

* **ls** - list
* **cat** - display contents of file (concatenate?)
* **cd** - change directory
* **mv** - move
* **man** - manual
* **mkdir** - make directory
* **rmdir** - remove directory
* **touch** - make empty file
* **cp** - copy
* **rm** - remove (file or directory)
* **locate** - find
* **clear** - clears the cli
* **pwd** - path to working directory (current location)
* **nano / vi** - creates or opens file in nano/vi text editor
* **sudo** - super user do
* **tar** - work with compression (tar ball archives)
* **zip/unzip** - work with comperssion (zip archives)
* **chmod** - change mode (change permissions, make file executable, etc.)
* **hostname** - displays hostname
* **wc** - word count (shows number of new lines, words and characters.)

### Deep dive

#### Piping

Piping | command will send data from **one program to another**.

Examples: 

* ls | head -3 - will pipe the output of ls to the head command and only show the first three files 

#### Redirecting

Redirecting >, >> or < commands will send data **from a file to a program or from a program to a file**.

Examples:

* ls > myoutput - will create a file myoutput with the list of files/directories of current location
* ls >> myoutput - will add the output to the existing file 
* wc < myoutput - will supply the content of the file myoutput to the command word count. Is more or less the same as the normal 'wc myoutput' as most commands will accept a file as input as an argument.


#### LS

* **ls -l** - long listing (show details)
* **ls -a** - all (show hidden files)
* **ls -lh** - long listing human readable
* **ls -R** - recursively (show files inside directory tree)
* **ls -lS** - long listing with Size of files

#### CAT

* **cat filename.txt** - show contents of filename
* **cat filename.txt filename.txt** - show contents of both files
* **cat >filename.txt** - create a file with filename, will await input of user. Close with ctrl+d
* **cat filename | more** - shows content but paginated with the more tool
* **cat filename | less** - shows content but paginated with the less tool
* **cat -n filename.txt** - shows content with line numbers
* **cat filename.txt > filename2.txt** - redirect output of filename.txt into new file filename2.txt
* **cat filename.txt >> filename2.txt** - redirect output of filename.txt into existing file filename2.txt
* **cat filename | more** - shows content but paginated with the less tool



## Unix Principles

* Everything is a file
* Make each program do one thing well

## Package management

Two large package managers:

* Debian (Debian distribution)
* RPM (Standard for other distributions - Red Hat) (Name from: Red Hat Package Manager)

### Debian package manager

* Extension: .deb
* Command: dpkg
* Command line front end programs: apt-get, aptitude
* GUI front-ends: synaptic, software-center

#### Commands

* updated list: `sudo apt-get update`
* search by keyword in packages: `sudo apt-cache search 'keyword'`
* install or update: `sudo apt-get install 'package'`
* update all packages: `sudo apt-get upgrade`
* remove package: `sudo apt-get remove 'package' (--purge to also delete config files)`
* list all packages on your system: `dpkg -l`
* list all files of package: `dpkg -L 'package'`
* check if file is part of package: `dpkg -S /path/to/file`


### RPM Package Manager 

* Extension: .rpm
* Command: rpm
* Command line front-end: yum, up2date (automatically resolve dependencies)
* GUI front-end: yumex, gpk-application

#### Commands

* install: `yum install 'package'`
* search: `yum search 'keyword'`
* update: `yum update 'package'`
* update all packages: `yum update`
* remove: `yum remove 'package'` -- also removes all dependencies



## Process management

### BIOS

BIOS (Basic Input/Output System) - software in non-volatile memory (firmware) on a chip on the system board
BIOS loads Bootloader from harddisk to RAM - Bootloader loads the Kernel.
Multi step process is needed to decouple the Operating System from the basic BIOS on the system.

### Kernel

Is the Core of the Computer System. Is the connection between the application software and the hardware of the machine.

* Memory Management
* Resource Management
* Device Management
* System Calls

### Processes

Information about the processes are available in pseudo-filesystems:

* Running processes are visible under `/proc`
* Hardware devices are available `/dev`
* Information about these devices `/sys`

Other commands:

* `pstree` show the Process tree
* `ps` shows current running processes
* `top`shows an overview of all processes which keeps updating (like task manager)
* `free` shows memory management    


## Permissions

[Source]('https://www.computerhope.com/unix/uchmod.htm')
### Viewing permissions

Permissons are managed by  rwx bits. 

* -r read
* -w write
* -x execute

You can view these permissions by following command: `ls -l filename.txt` The permissions are managed for three different users/groups: 

* u: User
* g: Group
* o: Others (all)

Example output:

![Permissions](/img/permission_linux.png)

`-rw-rw-r-- 1 home home` can be interpreted as read/write for home user, read/write for home group, read for all.

**Permissions are only valid for the contents of the file, not the file itself.** Renaming a file for example looks at the permissions for the upper directory, instead of the file itself.

Default permissions for files are not executable, but for directories are executable. For directories, this needs to be set to be able to call the ls command on the file.

### Changing permissions

#### Chmod

`chmod options permissions file name`

* Example symbolic mode: `chmod u=rwx,g=rx,o=r myfile`
* Example numeric mode: `chmod 754 myfile`

Numeric mode:

* 4: "read"
* 2: "write"
* 1: "execute"
* 0: "no permission."

So 7 is the combination of permissions 4+2+1 (read, write, and execute), 5 is 4+0+1 (read, no write, and execute), and 4 is 4+0+0 (read, no write, and no execute).

#### Umask

Umask (user file-creation mode mask) is a default set of permissions for a user when he/she created a file or new directory. The umask is defined by three numbers, and is subtracted from the default settings of 777 for directories and 666 for files.
A umask of 002 would create directories with permission of 775 and files of 664. A high umask means a safer environment.


## TTY

*Early user terminals connected to computers were electromechanical teleprinters or teletypewriters (**TeleTYpewriter**, TTY), and since then TTY has continued to be used as the name for the text-only console although now this text-only console is a virtual console not a physical console.*


## File system

Linux seperates the static files from the dynamic files on different partitions. This prevents memory allocations for dynamic or user created files from interfering with the limited memory of the static file system.  

Root files:

* Windows: C:\
* Linux: /

Linux 'hides' the location of the files. Windows shows this to the end user. In Windows, the structure of your directory and files are determined by the physical location of the files on the disk. In Linux, the location on the disk can be allocated to the file, and can be chosen or allocated by the sysadmin.

[Explanation File System Hierarchy]('https://www.thegeekstuff.com/2010/09/linux-file-system-structure/')
![File System Hierarchy](/img/filesystem-structure.png)

#### Basic level Software

* **/boot**: Contains the Linux kernel and other bootloader software
* **/bin**: Contains binary executables to call (like commands ls, ...)
* **/lib**: System libraries (like System.dll in windows) (GAC?)

#### Higher level software

* **/opt**: This directory is reserved for all the software and add-on packages that are not part of the default installation.


#### Other

* **/dev**: is the location of special or device files. It is a very interesting directory that highlights one important aspect of the Linux filesystem - everything is a file or a directory. /dev/cdrom and /dev/fd0 represent your CD-ROM drive and your floppy drive. This may seem strange but it will make sense if you compare the characteristics of files to that of your hardware. Both can be read from and written to. Take /dev/dsp, for instance. This file represents your speaker device. Any data written to this file will be re-directed to your speaker. If you try 'cat /boot/vmlinuz > /dev/dsp' (on a properly configured system) you should hear some sound on the speaker. That's the sound of your kernel! A file sent to /dev/lp0 gets printed. Sending data to and reading from /dev/ttyS0 will allow you to communicate with a device attached there - for instance, your modem.


### Mounting

Unix systems have a single directory tree. All accessible storage must have an associated location in this single directory tree. This is unlike Windows where (in the most common syntax for file paths) there is one directory tree per storage component (drive).

Mounting is the act of associating a storage device to a particular location in the directory tree. For example, when the system boots, a particular storage device (commonly called the root partition) is associated with the root of the directory tree, i.e., that storage device is mounted on / (the root directory).

Let's say you now want to access files on a CD-ROM. You must mount the CD-ROM on a location in the directory tree (this may be done automatically when you insert the CD). Let's say the CD-ROM device is /dev/cdrom and the chosen mount point is /media/cdrom. The corresponding command is

mount /dev/cdrom /media/cdrom
After that command is run, a file whose location on the CD-ROM is /dir/file is now accessible on your system as /media/cdrom/dir/file. When you've finished using the CD, you run the command umount /dev/cdrom or umount /media/cdrom (both will work; typical desktop environments will do this when you click on the “eject” or ”safely remove” button).

Mounting applies to anything that is made accessible as files, not just actual storage devices. For example, all Linux systems have a special filesystem mounted under /proc. That filesystem (called proc) does not have underlying storage: the files in it give information about running processes and various other system information; the information is provided directly by the kernel from its in-memory data structures.


## System and User Security

### Users and groups

Users are part of one or more groups, to be able to share data and files across the group.

* **/etc**: contains user and group data
* **/etc/passwd**: contains account info but **no** passwords
* **/etc/shadow**: contains the passwords of the users.

#### ETC/PASSWD

name:password placeholder:user id:primary group id:comment:home directory:shell

Example: daemon:x:1:1:daemon:/usr/sbin:/bin/sh 

* **name** (deamon): name of the account
* **password placeholder** (x): used to hold the password, is now replaced by a placeholder
* **user id** (1): the id of the user
* **primary group id** (1): the primary group of the user (every file has an owner and a group owner, which often is the primary group of the user)
* **comment** (deamon): can be used as a comment
* **home directory** (/user/sbin): the location of the users home directory (for normal users)
* **shell** (/bin/sh): the location of the users login shell, this is where the user is 'placed in' on login

#### ETC/SHADOW

name:password:last change:min:max:warn:inactive:expire:reserved

Example: sysadmin:$6$lS6WJ9O/fNmEzrIi$kO9NKRBjLJJTlZD.L1Dc2xwcuUYaYwCTS.gt4elijSQW8ZDp6GLYAx.TRNNpUdAgUXUrzDuAPsYs5YHZNAorI1:15020:5:30:7:60:15050:

* **name** (sysadmin): name of the account
* **password** ($6...): the password encrypted (one-way)
* **last change** (1): the id of the user

* **primary group id** (1): the primary group of the user (every file has an owner and a group owner, which often is the primary group of the user)
* **comment** (deamon): can be used as a comment
* **home directory** (/user/sbin): the location of the users home directory (for normal users)
* **shell** (/bin/sh): the location of the users login shell, this is where the user is 'placed in' on login