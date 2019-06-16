#Linux

## Important Commands

### Overview

* **ls** - list
* **cat** - display contents of file (concatenate?)
* **cd** - change directory
* **grep** - search
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
* **id** - shows current user
* **who** - displays the list of current users who are logged in on the system
* **w** - same as who but more detailed + system status info

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

#### GREP

* **grep 'word' filename** - search file for word
* **grep -i 'word' filename** - case-insensitive search
* **grep -R 'word'** - searches all files in current directory and subdirectories for word
* **grep -c 'word' filename** - search and display number of times word is found

## Unix Principles

* Everything is a file
* Make each program do one thing well

## Basic scripting

Hashbang: #! as first line of a text file creates an executable.
`#!/bin/bash -u`
The function of the hashbang is to tell the kernel what program to run as the script interpreter when the file is executed.

* Declaring a variable: VARIABLENAME="value"
* Using a variable: echo $VARIABLENAME
* Read: prompts the user for input.
* Output of a script: 0 on succesfull, 1 on error.
* Exit 1 or Exit 0 will manually create output.
* '#': comment
* if: 
    if command; then
    fi 

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
* **last change** (15020): number of days since the last change (counting from 01-01-1970)
* **min** (5): Password aging field - after the user changes his password, he/she can't change it again after this amount of days (prevents changing the password immediately back to original)
* **max** (30): forces the user to change password after this amount of days
* **warn** (7): warns the user in x amount of days before password expiration
* **inactive** (60): grace period after which the user is locked out of the account (starts after the max) (if set to inactive, admin needs to reset password)
* **expire** (15050): sets the date when to expire the account (lock). Possible to reset the password by admin.

### System Accounts

**Root account** is admin and has a id of 0.
**Normal accounts** have an id greater then 500.
**System accounts**:
* id between 1 and 499
* designed to provide accounts for services that run on the system.
* will have * instead of password

### Group membership

* /etc/passwd defines primary group membership
* /etc/group defines supplemental group memberships

#### ETC/GROUP

group_name:password_placeholder:GID:user_list

Example: mail:x:12:mail,postfix

user_list will display all members who are assigned to this group as a **secondary** membership. Primary membership is only defined in the /etc/passwd file.

#### File access

By default, any new file that a user creates will be owned by the user's primary group. -> this is probably the reason why a user will often have a group with the same name.

Group ownership of a file can be changed by `chgrp groupname filename`


### Root User

Root user has admin access. To change to root user, use `su`. 
In ubuntu, root account is disabled. Admin commands can then be run with `sudo`.

To add users with admin priviledges, you need to edit the `/etc/sudoers` file with the command `visudo` as the root user.  
sysadmin ALL=(ALL) ALL will mean: the user sysadmin can on ALL machines act as ALL users to execute ALL commands.

### Creating Groups and Users

#### Creating a Group

`groupadd groupname`

Reason: mostly for file sharing.

Groupname considerations (to prevent issues):

* The first character: underscore _ or a lower-case letter a-z.
* Max 16 (can be up to 32 but may give issues on some distributions)
* remaining characters can be alphanumeric, the dash - or an underscore_.
* The last character should not be a hyphen -.

example: hannes

#### Modifying a group

* `groupmod`
* `groupmod -n newname oldname` -- change group name
* `groupmod -g newid groupname` -- change group id

Changing the group name will have no impact on file access because the id is used as the identifier for the group. Changing the GID however will cause all associated files to become 'orphaned'. They will have a reference to a GID and no Group Name.

Orphaned files can be found with `find / -nogroup`

#### Deleting a group

`groupdel groupname`

This will also cause 'orphaned' files. 
Only supplemental groups can be deleted.

#### Creating a User

* Command `useradd` 
* Example `useradd -u 1000 -g users -G wheel,research -c 'Jane Doe' jane` 
Can be done manually, but is unsafe due to errors.

In some distributions, creating a new users also creates a **User Private Group** (UPG) with the same name as the user.

`useradd` will use some default settings to create new users.
`useradd -D` view or change these default settings. (/etc/default/useradd file)

Example output: 
GROUP=100
HOME=/home
INACTIVE=-1
EXPIRE=
SHELL=/bin/bash
SKEL=/etc/skel
CREATE_MAIL_SPOOL=yes

* **GROUP**: default primary group (in distributions not using UPG's. Is normally the 'users' group)
* **HOME**: home directory
* **INACTIVE**: number of days after the password expires. (-1 means disabled) 
* **EXPIRE**: expiration date
* **SHELL**: default shell
* **SKEL**: skeleton directory, can be used to populate default files and folder in the home directory of the user.
* **CREATE_MAIL_SPOOL**: local file where incoming email is placed.

Other default values can be found in /etc/login.defs such as the mail directory, password max days, password min days, password min length, pasword warning age, uid min, uid max, gid min, gid max, **umask** etc.

#### Advantages for multiple users

* selective access to certain files for certain users
* sudo command can be configured for selective commands and will log the usage by each user
* group membership for greater manamgement flexibility

#### Changing a password

`passwd` -> for own account
`passwd username` -> for other account 

#### Changing an existing user account

`usermod username`

* `usermod -c comment username`: comment
* `usermod -d directory username`: home directory
* `usermod -f days username`: inactive
* `usermod -e expire_date username`: expiration date
* `usermod -g groupname username`: set primary group
* `usermod -G groupname,groupname username`: set supplemental groups
* `usermod -a groupname username`: add supplemental groups
* `usermod -l loginname username`: set new login name
* `usermod -L username`: lock the user account
* `usermod -s shell username`: set the login shell
* `usermod -u user_id username`: set net userid
* `usermod -U username`: unlock the account

#### Deleting a user

* `userdel username`: deletes the user but keeps the user's home directory
* `userdel -r username`: deleted user and home directory


## Ownership and permissions

Each file has a user owner and a group owner.
By default the creator of the file and the primary group of this account are the owners of the file.

* `groups` -- shows the groups of which the current user is part of
* `newgrp groupname` -- temporarily change the primary group to one of the supplemental groups (to create a file with different group permissions)
* `chgrp groupname filename` -- changes group ownership of an existing file. -R option for recursive change of directory
* `stat filename` -- shows detailed information about a file (group ownership etc.) 
* `chown username filename` -- change user ownership
* `chown username:groupname filename` -- change user and group ownership
* `chown :groupname filename` -- change group ownership


## Permissions

[Source]('https://www.computerhope.com/unix/uchmod.htm')
### Viewing permissions

Permissons are managed by  rwx bits. 

* -r read 
* -w write
* -x execute

* Read: files can be read and copied. Directories can be listed without details.
* Write: Files can be written to and saved. Files can be added or remove from a directory. Read is necessary for correct usage.
* Execute: file can be executed or run. Directory can be changed by cd and directory can be used in path for commands.

**The permissions of all parent directories must be considered before considering the permissions on a specific file.** If a user only has read access to a parent directory of a file, he cannot access the file.
The w permission allows a user to delete files from a directory, but only if the user also has x permission on the directory.

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

Symbolic mode:

* u: "user"
* g: "group"
* o: "others"
* a: "all (user, group, other)"

* +: add permission
* -: remove permission
* =: set exact permission

* r: read
* w: write
* x: execute
* s: setuid/setgid

Example: add read permission for user on file abc.txt
`chmod u+r abc.txt`
`chmod ug+r,o-w abc.txt` -- add read for user owner and group owner, remove write from others

#### Umask

Umask (user file-creation mode mask) is a default set of permissions for a user when he/she created a file or new directory. The umask is defined by three numbers, and is subtracted from the default settings of 777 for directories and 666 for files.
A umask of 002 would create directories with permission of 775 and files of 664. A high umask means a safer environment.

`umask` -- shows the umask of the current user, ex. 0002
The first number indicates that the umask is given as an octal number.

Permanently changing a user's umask requires modifying the **.bashrc** file located in that user's home directory.

### File type

ls -l /etc/passwd
-rw-r--r--. 1 root root 4135 May 27 21:08 /etc/passwd

First character points to the file type:
* - :regular file
* d :directory file
* l :link (pointer to other file)
* b :block file - relates to block hardware device where data is read in blocks
* c :character file - read one byte at a time
* p :pipe file
* s :socker file - allows two processes o communicate


## Special permissions, Links and File locations

#### SETUID permission (s or 4)

Setuid allows a binary file to be run as the owner of the file instead of the user who executes the file.
Some system utilities use this setting to allow a user to run a file with root priviledges.

`chmod u+s filename` -- adds the setuid permission
`chmod 4*** filename` -- adds the setuid numerically (replace * by the numeric permissions the file already has)

#### SETGID permission

Similar as the setuid permission, but for groups. Behavior depends on if the permission is set to a file or a directory.

setgid on a **file** will allow a user to run an executable binary as temp member of the group. 

Example: /usr/bin/wall as executable with permissions:
`-rwxr-sr-x. 1 root tty 10996 Jul 19 2011 /usr/bin/wall`
The s permission on the group allows the user to execute this executable as member of the tty group and thus also access all necessery files to execute this command.

setgid on a **directory** causes files that are created in the directory to be owner by the group that owns the directory. Normally they are owned by the primary group of the user. The newly created directories will also have the same setgid permission.
This is necessary for team work if users with different primary groups need to create new files and directories in a shared directory.

#### Sticky Bit permission (t or 1)

Prevents other users from deleting your files in a shared directory. Only the owner will be able to delete the file.

* Symbolic: `chmod o+t directoryname`
* Numeric: `chmod 1775 filename/directoryname` (add 1 before the existing permission)

lowercase t - both sticky bit and execution are set for 'others'
uppercate T - only sticky bit is set for 'others'

#### Links

Hard to access files (with long url's, deeply buried in directories) can be 'copied' to a link file. This file acts as a shorcut to the file.

`ls -li filename` - shows number of existing links for a file
`ln existingfilename newfilename` - creates a hard link
`ln -s existingfilename newfilename` - creates a soft link

* Hard link will create file which points to same inode.
* Soft link will point to filename.
* Deleting 'hard link' file will only work if all files which point to the same inode are deleted. -> safer
* Soft link works with directories.
* Soft link can work between different file systems (partitions).

Difference between hard and soft links:
*Underneath the file system files are represented by inodes. A file in the file system is basically a link to an inode. A hard link then just creates another file with a link to the same underlying inode. When you delete a file it removes one link to the underlying inode. The inode is only deleted (or deletable/over-writable) when all links to the inode have been deleted. A symbolic link is a link to another name in the file system. Once a hard link has been made the link is to the inode. deleting renaming or moving the original file will not affect the hard link as it links to the underlying inode. Any changes to the data on the inode is reflected in all files that refer to that inode. Note: Hard links are only valid within the same File System. Symbolic links can span file systems as they are simply the name of another file.*

