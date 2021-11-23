# CMPE283 Assignment 1

Completed as individual

## Step-by-Step Instructions
- Configure Xubuntu 20.04.3 64-Bit on VMware Workstation (4 Processors, 8 GB Memory, 50 GB Hard Disk, Nested Virtualization Enabled)
- Install all necessary requirements in the terminal using the following commands:
  - `sudo apt install git`
  - `sudo apt install build-essential`
  - `sudo apt install flex bison`
  - `sudo apt install libssl-dev`
  - `sudo apt install libelf-dev`
  - `sudo apt install dwarves`
- Fork the Linux GitHub repository and then clone your forked repository into your VM's home directory
- Run the following commands to begin the process of building the Linux Kernel
  - `cd linux`
  - Create a folder, cmpe283, and place Makefile and cmpe283-1.c files provided into this folder within the linux directory
  - Modify cmpe283-1.c in order to obtain required output
    - Additionally, add `MODULE_LICENSE("GPL v2");` to the bottom of cmpe283-1.c to avoid license error
  - `uname -a`
    - Displays the config file that you want to copy, in my case it was 5.11.0-40-generic
  - `cp /boot/config-5.11.0-40-generic .config`
    - If you run into certificate issues, modify the .config file
      - Set the lines, `CONFIG_SYSTEM_TRUSTED_KEYS` and `CONFIG_SYSTEM_REVOCATION_KEYS` to empty strings
  - `make oldconfig`
    - Might get asked a bunch of questions, just press and hold enter to give default answers until all questions are answered
  - `make prepare`
  - `make -j 4 modules`
    - The number "4" represents the number of processors assigned to your VM and the number of Make jobs running at once
  - `make -j 4`
  - `sudo make INSTALL_MOD_STRIP=1 modules_install`
  - `sudo make install`
  - `sudo reboot`
    - This will reboot the VM
  - `uname -a`
    - Now displays the new Linux kernel from GitHub, which as of this writing is 5.15.0+, instead of 5.11.0-40-generic from earlier
  - `cd linux/cmpe283`
  - `make`
  - `sudo insmod cmpe283-1.ko`
    - Insert the module into the kernel
  - `dmesg`
    - ![Capture](https://user-images.githubusercontent.com/2999334/141731903-8fba6a29-2c21-4ad4-a67e-fb0fb507b922.PNG)
  - `sudo rmmod cmpe283-1`
    - Remove the module out of the kernel
  - `dmesg`
    - ![Capture2](https://user-images.githubusercontent.com/2999334/141731919-a5066e6f-4096-4296-8ca7-f13483642a46.PNG)
  - Commit any changes to your forked repository on GitHub





# CMPE283 Assignment 2

Group Members: Martin Duong & Ruchit Patel

Work Distribution:
- **Martin Duong**: Implemented 0x4FFFFFFF according to professor's video with slight modifications to get kernel to build. Handled the building of the kernel, setting up the VM environments, and testing the program.
- **Ruchit Patel**: Implemented 0x4FFFFFFE. Added variables in cpuid.c to calculate the processing time of exits. Modified kvm_emulate_cpuid to fetch the processing time when eax=0x4FFFFFFE. Displayed the high 32 bits of the total time spent processing all exits in %ebx and low 32 bits of the total time spent processing all exits in %ecx. Coded the vmx_handle_exit to calculate processing time of exits in vmx.c.

## Step-by-Step Instructions
Assumption: Linux kernel setup from assignment 1 has been completed, running on a VM with Xubuntu 20.04

Note: In our case, following the professor's video for 0x4FFFFFFF resulted in error which were solved by moving around the variable declarations and using `EXPORT_SYMBOL`

1. Implement CPUID leaf nodes: 0x4FFFFFFF and 0x4FFFFFFE in cpuid.c and vmx.c within the Linux kernel
2. Change your directory to where you placed the Linux kernel
3. `make -j 4 modules`
4. `sudo bash`
5. `make MOD_INSTALL_STRIP=1 modules_install`
	- We don't have to run this command with the additional "make install" since we haven't changed the version of the kernel we're running
6. Make sure kvm module for hypervisor is not already loaded
	- `lsmod | grep kvm`
	- If it is then:
		- `rmmod kvm`
		- `rmmod kvm_intel`
			- Run this first if you are presented with `ERROR: Module kvm is in use by kvm_intel`
	- If nothing is returned then kvm hypervisor is not running, next:
		- `modprobe kvm`
			- If you get missing symbol error then change around where variables are declared in cpuid.c and vmx.c
		- `lsmod | grep kvm`
			- Check if kvm is loaded
		- `modprobe kvm_intel`
		- `lsmod | grep kvm_intel`
		  - This should ensure that kvm and kvm_intel are loaded properly and you should get the result below
		  - ![Capture](https://user-images.githubusercontent.com/2999334/142976420-d320ea22-9eed-4eba-bb1a-e3fcb6ac5bdb.PNG)
		
7. Install kvm on VM running Xubuntu 20.04
8. Create a VM with kvm running Lubuntu 21.10, 2GB Memory, 1 Processor
9. Boot into VM running Lubuntu 21.10 (VM within a VM)
10. Install CPUID package on inner VM
11. On the terminal, run `cpuid -l [Insert leaf node here]` (Example: `cpuid -l 0x4FFFFFFF`)
12. 
