# Hardware and Software Basics 

## Hardware components. 
On the operatings system in general there are several hardware compoenents that need to be managed.
The components are listed below: 

1. **CPU** - CPU is the brain behind the computer. It performs instructions and manages data flow. 
2. **Memory** - there are two main kinds of memory from a hardware perspective. One is a fast memory
   type that holds data and is used to hold data temporarily and therefore this is called the cache
   memory. Main memory on the other hand is slow and is used for execution of the instructions. 
3. **Buses** - individual components on the computer board communicate or transfer data between the
   components using buses. The buses are divided based on the functions they perform. 
   * Address Bus - are used to specify the address for the data transfer
   * Data Bus - carry the data that needs to be transmitted. 
   * Control Bus - these are used to route timing and control signals. 
4. **Controller and Peripherals** - Peripheral devices are real devices that are attached to the
   computer board. In order to control the peripheral devices there are controller components that
   act as an interface between the CPU and the device itself. E.g. SCSI disks are controlled by a
   SCSI controller chip. The connection between the controller and the CPU is done using buses.
   Controller are like mini CPUs and behave intelligently to allow the peripheral device to work
   well. 
5. **Address Space** - System buses are the ones that connect the CPU to the main memory and is separate
   from the bus connecting the CPU to the system's hardware peripherals. That memory that the device
   has access to is called I/O memory. 
   * Controller can only access the system memory (main) indirectly through the CPU and never
     directly. So a device like the floppy disk can only see the address space that is between the
     device and CPU and not the main memory directly. 
   * CPU has separate instructions to control I/O and the main memory space. 
   * There is a scenario where that peripheral device has to directly push data to system memory.
     e.g. when the user data is being written to hard disk. In such cases DMA (direct memory access)
     is allowed but only under the strict supervision of the CPU. 
6. **Timers** - Computers require a special peripheral called RTC (real time clock). RTC has its own
   power so it can continue even when the computer is shut down. This is used to get right time of
   day and also the right timing interval. 

 
## Software

**What is an Operating System?** 

It is a collection of system programs that which allow the user to run application software.
Operating system abstracts the real hardware system and presents the user and the applications a
virtual machine.

OS has various components and in the case of Linux there is the Kernel, Libraries and the shell that
make it possible for the user to interact with the OS. 

**Memory Management** 

In order to manage memory better and optimize how the processes get the scarce resource the kernel
divides the memory into easily handled pages and swaps these pages onto hard disk as system runs. 

**Processes** 

Process is a program in action. In order to manage the CPU time well kernel uses a technique called
time slicing where each process is given some time slices to do their tasks and are stopped after
the alloted time so that they do not hog all the CPU time. 

**Device Drivers** 

Device drivers are important pieces of software that operate in a previledged environment and can
cause problems if things go wrong. The device drivers control the interaction between the OS and the
device they control. 

**File System** 

A file system is a unified way in which the files and folders can be accessed. There are various
types of file systems that Linux supports and all of them can be accessed using the same interface.
Linux OS allows for different types of file systems to be mounted at different mount points
(locations) on the single system tree (root). 

This way of handling file system allows users not to worry about the file system that the file is
in. The user just needs to know how to use the file. 


