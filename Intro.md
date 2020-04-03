# The Structure of MS-DOS
MS-DOS is partitioned into several layers that serve to isolate the kernel logic of the operating system, and the user's perception of the system, from the hardware it is running on. 
The Layers are
1. The BIOS (Basic Input/Output System)
2. The DOS kernel
3. The command processor (shell)

Let us discuss each of them one by one.
## The BIOS Module
The BIOS is specific to the individual computer system and is provided by the manufacturer of the system. It contains the default resident hardware-dependent drivers for the following devices:
1. Console display and keyboard (CON)
2. Line printer (PRN) - A general printer that can print a whole line at once. 
3. Auxiliary device (AUX)
4. Date and time (CLOCK$)
5. Boot disk device (block device) - A boot device is any piece of hardware that can read or contains the files required for a computer to start.

The MS-DOS kernel communicates with these device drivers through I/O request packets; the drivers then translate these requests into the proper commands for the various hardware controllers. In many MS-DOS systems, including the IBM PC, the most primitive parts of the hardware drivers are located in read-only memory (ROM) so that they can be used by stand-alone applications, diagnostics, and the system startup program.

The terms **resident** and **installable** are used to distinguish between the drivers built into the BIOS and the drivers installed during system initialization by DEVICE commands in the **CONFIG.SYS** file. 

**Note:**
Developed by Microsoft, the SYS file format was designed to store compiled system resources like configuration variables and settings required by Windows and DOS (Disk Operating System) for performing its intended operations. Basically, these are the output files that we get after compiling asm files in this repository.

The **BIOS is read into random-access memory (RAM)** during system initialization as part of a file named **IO.SYS.** 

## The DOS Kernel
The DOS kernel implements MS-DOS as it is seen by application programs. The kernel is a proprietary program supplied by Microsoft Corporation and provides a collection of hardware-independent services called system functions. These functions include the following:
1. File and record management
2. Memory management
3. Character-device input/output
4. Spawning of other programs
5. Access to the real-time clock

Programs can access system functions by loading registers with function-specific parameters and then transferring to the operating system by means of a software interrupt. The DOS kernel is read into memory during system initialization from the **MSDOS.SYS** file on the boot disk.
## The Command Processor

The command processor, or shell, is the user's interface to the operating system. It is responsible for parsing and carrying out user commands, including the loading and execution of other programs from a disk or other mass-storage device.


The default shell that is provided with MS-DOS is found in a file called **COMMAND.COM**. Although COMMAND.COM prompts and responses constitute the ordinary user's complete perception of MS-DOS, it is important to realize that COMMAND.COM is not the operating system, but simply a special class of program running under the control of MS-DOS. COMMAND.COM can be replaced with a shell of the programmer's ow design by simply adding a SHELL directive to the system-configuration file (CONFIG.SYS) on the system startup disk.


The default MS-DOS shell, COMMAND.COM, is divided into three parts:
1. A resident portion
2. An initialization section
3. A transient module

### Resident Portion
The resident portion is loaded in lower memory, above the DOS kernel and its buffers and tables. It contains the routines to process Ctrl-C and Ctrl-Break, critical errors, and the termination (final exit) of other transient programs. This part of COMMAND.COM issues error messages and is responsible for the familiar prompt. The resident portion also contains the code required to reload the
  transient portion of COMMAND.COM when necessary.
