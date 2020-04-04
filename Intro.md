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
The resident portion is loaded in lower memory, above the DOS kernel and its buffers and tables. It contains the routines to process Ctrl-C and Ctrl-Break, critical errors, and the termination (final exit) of other transient programs. This part of COMMAND.COM issues error messages and is responsible for the familiar prompt. The resident portion also contains the code required to reload the transient portion of COMMAND.COM when necessary.

### Initialization section
The initialization section of COMMAND.COM is loaded above the resident portion when the system is started. It processes the AUTOEXEC.BAT batch file (the user's list of commands to execute at system startup), if one is present, and is then discarded.

### Transient Module
The transient portion of COMMAND.COM is loaded at the high end of memory, and its memory can also be used for other purposes by application programs. The transient module issues the user prompt, reads the commands from the keyboard or batch file, and causes them to be executed. When an application program terminates, the resident portion of COMMAND.COM does a checksum of the transient module to determine whether it has been destroyed and fetches a fresh copy from the disk if necessary.

### The different categories of USER commands
The user commands that are accepted by COMMAND.COM fall into three categories:
1. Internal commands
2. External commands
3. Batch files

* Internal commands, sometimes called intrinsic commands, are those carried out by code embedded in COMMAND.COM itself. Commands in this category include COPY, REN(AME), DIR(ECTORY), and DEL(ETE). The routines for the internal commands are included in the transient part of COMMAND.COM.
* External commands, sometimes called extrinsic commands or transient programs, are the names of programs stored in disk files. Before these programs can be executed, they must be loaded from the disk into the transient program area (TPA) of memory. (See "How MS-DOS Is Loaded" in this chapter.) Familiar examples of external commands are CHKDSK, BACKUP, and RESTORE. As soon as an external command has completed its work, it is discarded from memory; hence, it must be reloaded from disk each time it is invoked.
* Batch files are text files that contain lists of other intrinsic, extrinsic, or batch commands. These files are processed by a special interpreter that is built into the transient portion of COMMAND.COM. The interpreter reads the batch file one line at a time and carries out each of the specified operations in order.

In order to interpret a user's command, COMMAND.COM first looks to see if the user typed the name of a built-in (intrinsic) command that it can carry out directly. If not, it searches for an external command (executable program file) or batch file by the same name. The search is carried out first in the current directory of the current disk drive and then in each of the directories specified in the most recent PATH command. In each directory inspected, COMMAND.COM first tries to find a file with the extension .COM, then .EXE, and finally .BAT. If the search fails for all three file types in all of the possible locations, COMMAND.COM displays the familiar message

  `Bad command or file name`

If a .COM file or a .EXE file is found, COMMAND.COM uses the MS-DOS EXEC function to load and execute it. The EXEC function builds a special data structure called a program segment prefix (PSP) above the resident portion of COMMAND.COM in the transient program area. The PSP contains various linkages and pointers needed by the application program. Next, the EXEC function loads the program itself, just above the PSP, and performs any relocation that may be necessary.

Finally, it sets up the registers appropriately and transfers control to the entry point for the program. (Both the PSP and the EXEC function will be discussed in more detail in Chapters 3 and 12.) When the transient program has finished its job, it calls a special MS-DOS termination function that releases the transient program's memory and returns control to the program that caused the transient program to be loaded (COMMAND.COM, in this case).

A transient program has nearly complete control of the system's resources while it is executing. The only other tasks that are accomplished are those performed by interrupt handlers (such as the keyboard input driver and the real-time clock) and operations that the transient program requests from the operating system. **MS-DOS does not support sharing of the central processor among several tasks executing concurrently**, nor can it wrest control away from a program when it crashes or executes for too long.


# How is MS-DOS loaded ?
When the system is powered up, program execution begins at address 0FFFF0H. This is a feature of the 8086 family. Hence, the systems based on these processors are designed so that the address OFFFF0H lies in a portion of the ROM and contains a **jump instruction to transfer the control to system test code and the ROM bootstrap routine**
