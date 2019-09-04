# Device Drivers 

The CPU is not the only component on the computer that is intelligent. All physical devices on the
computer have a controller with some bit of memory and register to do some processing on its own.
E.g. SCSI has a SCSI controller and keyboard too has its own controller. 

Each of the physical devices on the system are managed by the software which is part of the Linux
kernel which is called the device driver. 

Every device on the Linux system is represented by a device special file which looks similar to a
file on the OS. All devices controller by the same device driver have the same major device number
but are differ in minor revision number. Linux supports 3 kinds of hardware devices: 

1. character - these devices are written to or read from directly without the involvement of the
   buffer. e.g. is serial ports on the computer. 
2. block devices - these are devices that are written to or read from in blocks of 512 or 1024 byte
   chunks. These are devices that generally go through a buffer in order to do their operations.
   The example being hard disks. These devices are accessed via the file system. Also only block
   devices can support a mounted file system. 
3. network devices - these are accessed using BSD sockets and the networking sub system. 

The main attributes of device drivers are: 

* kernel code - the code of the device driver becomes part of the kernel therefore it must be
  written carefully to avoid it from harming the entire OS and hardware. 
* kernel interface - each device driver must provide a standard interface that the kernel will use
  to interface with it. Interface can be either a file i/o or a buffer cache interface. 
* Kernel mechanism and services - the device drivers use kernel services and data structures like
  wait queues and memory allocation service etc to work efficiently. 
* Loadable - the device drivers are loaded into the kernel as modules. This architecture allows the
  kernel to get ride of device driver module that are not used by the kernel. 
* dynamic - As the system boots up it loads the device driver which simply checks whether the device
  is present or not, if the device is absent the kernel just ignores the device and the driver and
  continues it work. 

## 1. Polling and Interrupts 

When a command is given to a device. The device driver has a choice as to how it finds out that the
command is complete. There are two ways this can be done: 

* polling - here the device drivers, at regular intervals (determined by timers), will poll the
  status register on the device to figure out the completion status. This mechanism is approximate
  at best because of the use of timers which can be delayed based on how the OS is processing.
* interrupts - another mechanism is to use interrupts. Here the hardware device will raise an
  interrupt everytime it needs to be serviced. It is up to the kernel to call the device driver as a
  response to the interrupt. This is achieved by register an interrupt that the device will throw
  with the kernel and the handler must call the device driver to go the needful. The interrupts that
  are registered on the system can be seen under /proc/interrupts. 

An example of this approach is when the ethernet controller will raise an interrupt each time it
recevies an internet packet and that interrupt is processed by a routine on the device driver end
which is specialized code to handle pakcets form the internet. 

## 2. Direct Memory Access 

Using interrupts as a mechanism to handle data transfer with a device driver can be a good strategy
when the transfer rate is slow. When the data transfer rate is a lot higher either on the ethernet
side or the SCSI disk level there is a need to have a different strategy as the interrupt based
approach will be prohibitive from a performance standpoint. 

The alternate is called direct memory acess (DMA). The DMA controller allows the system to transfer
data to and from the device to the system memory without it ever being sent to the CPU. The data
transfer is initiated when the device driver sets up DMA channel and count registers along with the
direction of the data read or write. Once the data transfer is complete the device will interrupt
the PC. During the data transfer the CPU is free to do other stuff. 

Points to remember with DMA controller: 
1. DMA controller knows nothing of the virutal memory, it only has access to physical memory.
   Therefore memory that is being DMA'd to or from must be a contiguous block of physical memory.
   This means you cannot DMA directly to a processes virtual address space. You can however lock the
   processes physical pages so that they are not swapped out till the DMA operations are done. 
2. DMA cannot access the entire memory. The DMA can access the botton 16 Mbytes of memory. 

Linux tracks the DMA channels using the dma_chan data structure. This has two fields: 
1. pointer to the name of the owner of the DMA channel. 
2. flag indicating if the channel is in use or not. 


## 3. Memory 
