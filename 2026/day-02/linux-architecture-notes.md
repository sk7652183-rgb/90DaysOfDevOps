*******The core components of Linux (kernel, user space, init/systemd)************************************************
The kernel is the heart of the Linux operating system. It acts as a bridge between the shell and the system's hardware, enabling seamless communication between software and physical components. Additionally, the kernel is responsible for managing all processes running within the Linux operating system
userspace 
User Space (also called userland) is the area of memory where all user applications and programs run, separate from the kernel.
Systemd is the first process launched by the Linux kernel during system startup. It is assigned PID 1 (Process ID 1), making it the parent of all other processes in the system. It is responsible for initializing the user space and managing system services throughout the operating system's lifecycle
********************How processes are created and managed***********************************************************************************
When the system is powered on, electricity reaches the motherboard, which triggers the BIOS/UEFI firmware. The BIOS/UEFI acts as the bridge between hardware and software — it performs a POST (Power-On Self Test) to check hardware components. Once the hardware check is complete, it locates and loads the bootloader (such as GRUB) from the disk. The bootloader then loads the Linux kernel into memory. Once the kernel is fully initialized, it starts Systemd as the very first process, assigned PID 1, which then manages all remaining system startup tasks
************************************What systemd does and why it matters******************************************
Systemd is the first process launched by the Linux kernel during system startup. It is assigned PID 1 (Process ID 1), making it the parent of all other processes in the system. It is responsible for initializing the user space and managing system services throughout the operating system's lifecycle
*****************************Explain process states (running, sleeping, zombie, etc.)************************************************
Running means the process is currently being executed by the CPU — this includes any active jobs, scripts, or commands being processed at that moment.
Sleeping means the process is waiting for something to happen — such as user input, a scheduled time, or a resource to become available. Once the condition is met, it resumes running.
Zombie means the process has finished execution but still has an entry in the process table because its parent process has not yet collected its exit status. It is neither running nor alive — it is essentially a dead process waiting to be cleaned up.
************************List 5 commands you would use daily***********************************
ls --- Listing directory 
pwd --- Present working directory
cd ------change directory 
htop -----Active Process Monitor
df -h ------disk space usage 
