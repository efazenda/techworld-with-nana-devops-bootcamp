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




