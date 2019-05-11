#Linux

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

