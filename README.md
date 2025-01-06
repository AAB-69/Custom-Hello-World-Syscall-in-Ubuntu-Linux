# Custom-Hello-World-Syscall-in-Ubuntu-Linux
Overview
This project demonstrates the creation of a custom "Hello World" system call (syscall) in the Linux kernel running on Ubuntu. It provides all the steps involved in modifying the kernel source, adding a new syscall, rebuilding the kernel, and testing the syscall by writing a simple C program to invoke it.

Table of Contents
Prerequisites
Implementation Steps
Setting up the Environment
Modifying the Kernel
Rebuilding the Kernel
Testing the Syscall
File Structure
Contributing
License
Prerequisites
Before you begin, make sure you have the following:

Ubuntu Linux (or compatible Linux distribution)
Root access to modify kernel source
Tools: gcc, make, git, and others
Basic understanding of Linux system calls, kernel modules, and compiling the kernel
To install necessary tools on Ubuntu, run:

Implementation Steps
Setting Up the Environment
Install necessary packages.
Download the Kernel Source.
Modifying the Kernel
Add the Custom Syscall:
Open the syscall_table.S file located at arch/x86/entry/syscalls/syscall_table.S.
Add the entry for your new syscall by appending it to the syscall table:
.long sys_hello_world
Define the Syscall:
Create the syscall handler in the kernel/sys.c file:
asmlinkage long sys_hello_world(void) {
    printk("Hello World from custom syscall!\n");
    return 0;
}
Declare the Syscall in Header File:
Update the include/linux/syscalls.h to declare your custom syscall:
asmlinkage long sys_hello_world(void);
Rebuilding the Kernel
Configure the Kernel:
Run make menuconfig to configure the kernel. This step ensures the configuration of any options you may need, like enabling custom system calls.
make menuconfig
Compile the Kernel and Modules:
Compile the kernel and modules using make:
make -j$(nproc)
sudo make modules_install
sudo make install
Update GRUB and Reboot:
After the kernel is compiled, update the GRUB bootloader and reboot your system:
sudo update-grub
sudo reboot
Testing the Syscall
Write a Test Program:
Create a simple C program that calls your custom syscall:
#include <stdio.h>
#include <sys/syscall.h>
#include <unistd.h>

#define SYS_hello_world 335  // Syscall number for your custom syscall

int main() {
    syscall(SYS_hello_world);
    return 0;
}
Compile the Test Program:
Compile the test program using gcc:
gcc -o test_hello_world test_hello_world.c
Run the Test Program:
Run the compiled program to test if the syscall works correctly:
./test_hello_world
You should see the message "Hello World from custom syscall!" printed to the terminal.
