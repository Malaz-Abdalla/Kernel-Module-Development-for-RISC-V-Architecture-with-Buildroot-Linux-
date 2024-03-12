# Kernel-Module-Development-for-RISC-V-Architecture-with-Buildroot-Linux
This repo is prepared to explain how to create the development environment as Buildroot running on QEMU emulating RISC-V architecture. We basically create a kernel on our Ubuntu host machine and use cross-compilation which is a toolchain you add to your host to translate the kernel module you develop on your Ubuntu host to the target Buildroot. 

This project is done on an Ubuntu host (version 23.10), which is a virtual machine created on the Virtual Box software, by Oracle. For instructions on setting this environment visit ubuntu.com and virtualbox.org. The commands here are compatible with Ubuntu/Debian-based platforms, that's why make sure you also use the same system. 

The first step is installing Qemu from the git repository and boeforehand make sure you have necessary toolchanics and dependencies:
Installing dependencies:
`sudo apt-get update
sudo apt-get install -y build-essential git ncurses-dev bison flex libssl-dev make gcc
`
In case you encounter any dependency errors, read the error message and install the required dependent tool indicated.

Installing QEMU:
`git clone https://github.com/qemu/qemu.git`
`cd qemu`
`git checkout v6.0.0`
Configure the system for risc-v architecture by the following command:
`./configure --target-list=riscv64-softmmu`
`make`

Then you have to install Buildroot to the QEMU, you should be inside /qemu directory. 

`git clone https://github.com/buildroot/buildroot.git`
`cd buildroot/`
`make qemu_riscv64_virt_defconfig`
`make`

The last mae command might take some long time , possibly more than 30 minutes since it compiles the entire kernel of target.

After the kernel compilation 'make' you will see the image files iside /buildroot/output directory path. Go to /images directory and execute the file named start-qemu.sh by adding ./ to the beginning: `./start-qemu.sh`

This will boot the system with OpenSbi, and your Buildroot system will start. Keep in mind that this systems does not have a GUI(Graphical User Interface), meaning you will have to use command line to interact with the new system we just created.

Cross-compilation of a kernel module:
It is highly suggested that you start with a simple kernel module to test your system, in our case we will create a kernel module that gives a message as 'Hello World'. First you test it on your host machine Ubuntu. 

