Module Overview 

* Intoduction to Opearting Systems
* Virtualization
* Linux File System
* Main Linux Commands
* Package Manager
* Vim Editor
* Users and Permissions
* Shell Scripting
* Environment Variables
* Networking
* SSH - Secure Shell

Introduction to Virtualization & Virtual Machines 
----------------------------------------------------


What is an Operating System ?

Every computer is made of hardware

* CPU
* Memory
* Storage
* I/O devices

Applications use hardware resources through the Operating System
Operating system can interact and talk to the hardware
The operating system is an translator
It will also manage resources among applications and isolate the content of different applications

The tasks of an OS

1/ Resource Allocation and Management
1 - 1/ Memory Management
1 - 2/ Storage Management
1 - 3/ Manage Filesytem
1 - 4/ Management of I/O devices
1 - 5/ Security & Networking

Operating System Components

* Kernel 
* Device drivers
* Application layer

Operating systems for Servers

* Mostly on Linux
* More light-weight and performant
* no GUI or other user applications

How to interract with Kernel :

* GUI 
* Command Line Interface

Android Example 

* Hardware
* Linux Kernel
* Android
* User Apps

Each OS has many versions
Kernel stay the same
Linux and Windows have server distributions

Over half of the servers are running Linux operating system

Mac OS was originally called OS X

To sum up :

Operating systems is powering all devices
iOS, Android are Operating Systems
Android is based on Linux Kernel
Mac OS and iOS are based on Darwin Kernel

Mac OS versus Linux

* Command Line interface and file system structure is similar
* Whereas Windows is different

Setup a Linux Virtual Machine
----------------------------------------------------

* What virtualization and Virtual machines are
* Why Virtualization is useful 
* Main Concepts of how virtualization works

What is a virtual machine ?

No need separate hardware needed
You will need an hypervisor

e.g : VirtualBox, VMware, Hyper-V

The hypervisor takes hardware resources from Host OS 
Creates virtual CPU, virtual RAM, virtual storage for each virtual machine

Note : you can only give resources you actually have on the hypervisor machine.

Each virtual machines are completely isolated
if something breaks inside the VM, it doesn't affect the host machine

Benefits of using a Virtual Machine

* Great for learning and experiment
* Don't need to buy another computer
* You don't endanger you main OS 
* Test your app on different OS 

Difference between Type 1 and Type 2 Hypervisor

Type 2 : Hardware -> Hosts OS -> Hypervisor -> Guest OS (Personal computer)
Type 1 (Bare metal hypervisor): Hardware -> Hypervisor -> Guest OS (Production use)

Advantage of Hypervisor Type 1 :

* Efficient usage of hardware resources 
* Abstraction of the Operating system from the hardware

Linux File System
---------------------

Linux versus Windows file system

Linux 

* Hierarchical tree structure
* 1 root folder

Windows

* Multiple root folders
* Windows use letters for disks 
* C internal drive
* Next letters for the next drives

On Linux

* Every users have his own directory under /home/
* The root directory is located on /
* The root home folder is located in /root/ path

Programs installed system wide are available to all users on the target computer

The /bin directory 

Contain the binary programs that are system wide

The /sbin directory

Contain the system binary files (superuser permission to execute them)

The /lib directory

Hold the libraries for the binary of the system or applications 

The /usr directory

Contain binary folders , some of them are stored in /bin and some in /usr folder

The /usr/local folder

This contain the application of user that install (binary, config ...) 

The /opt directory (system wide)

Thrid party application can be installed on /opt 
Application that are not spliting their lib, binaries, configuration...

The /boot directory

It contains the boot images of the opearting system.

The /etc directory 

Contain all the configurations files 

The /dev folder

This contain all the device mapping files (mouse, disks, floppy, monitor...)

The /var folder

* Cache data
* Logs data 

The /tmp folder

Stores temporary files needed for applications

The /mnt or /media folders

* /media for external media mount point
* /mnt for manual filesystem mount

usually you will not interact with these folders when installing apps via Package Manager, this will be automatically managed via the Package Manager of the Operating system.

Hiden files ( dot files)

On Linux the fhiden files are usually starting with a "." it works for folders or files

Used to prevent accidently deleting files 

Into to Command Line (Part 1)
------------------------------

GUI vers CLI

GUI : Graphical User Interface
CLI : Command Line Interface

Usually the servers have only the CLI 

The CLI contains the prompt : 

* username
* computer name
* current directory
* $ sign for regular user
* \# sign for super user

Basic Linux Commands (CLI - Part 2)
------------------------------------

Directory operations 

* pwd (Print current directory)
* ls (List folder and files)
* cd (Change directory)
* mkdir (Make Directory)

File operations

* touch (Create a file)
* rm (Remove files and folders)

Everything in Linux is a File.

Navigating in the filesystem

* cd /usr/local/bin
* cd ../.. going up on filesystem 

There are 2 was , we can use relative path from the current directory or absolute path.

To rename files and folders

* mv file1 file2 (Rename files)
* mv folder1 folder2 (Rename folders)

To copy files and folders

* cp file1 file2
* cp folder1 folder2

List files recursivly 

* ls -R folder/ 

List the history of commands lines

* history

List hidden files 

* ls -a

Display the content of a file

* cat file
* zcat file.tar.gz
* less file
* more file
* tail file

Why use CLI over GUI ?

* Work more efficient
* Bulk operations
* More that GUI in term of functionality 

Display OS informations 

* uname (Kernel version, OS)
* cat /etc/os-release
* lscpu
* lshw
* lsmem

Execute commands as superuser

* sudo ls 
* sudo su - root
* sudo -c ls -u username

Package Manager - Installing software
------------------------------------

Using a Package Manager Tools

What is a package manager and a software package

* A compressed archive, containing all the required files
* And also all the depencies managed bw the Package Manager

It will download, installs or updates existing software packages from a repository

It will ensure the integrity and authenticity of a package

It will known where to put files in the system hierarchy

* Ubuntu : APT 
* Debian : APT
* Centos : DNF

Find packages (Ubuntu Apt)

* apt search openjdk

Install a package (Ubunt Apt)

* apt install openjdk-default

Remove a package (Ubuntu Apt)

* apt remove openjdk-default

The packages come from repositories, each distribution have their own repositories, some third party application have also their own repositories

To refresh metadata of the repositories you will need to run apt update

You have other ways of installing a program which is not available in repositories on (Ubuntu) for example

* snap package manager tool (bundle package with all dependencies)
* add-apt-repository (third party repository) -update file /etc/apt/sources.list (PPA Personal Package Archive)

Vi & Vim Text Editor
------------------------------------

Vi & Vim are text editor from command line

Built-in common editor are usually vi or vim

* small modifications can be faster
* faster to create and edit at the same time
* support multiple formats

Use cases : 

* when working in remote servers
* git for writing the git commit message
* display kubernetes configuration files 

To install vim : 

* sudo apt install vim

Vim Editor has 2 modes 

* Command Mode (default)
* Insert Mode (Edition)

From Commande Mode to Insert Mode : <i>

To go back to command mode : <ESC>

To save the file : <:wq>

To not save the file : <:q!>

















