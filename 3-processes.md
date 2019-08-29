# Processes 

## 1. Linux Processes 

Each process in Linux is represented by a `task_struct` data structure. Each `task_struct` is linked
to a task vector that holds the reference to all process data structures. Processes are broadly of
two types normal and real time. 

There are various attributes that the `task_struct` data stucture has and they are divided into the
following kind: 

* State - this gives the state in which the current process is in there are 4 states
	* Running - this is the state the process in either when it is running or in ready to run
	  state. 
	* Waiting - the process is in this state when it is waiting for some input or other event to
	  occur. The waiting state are of two kinds: 
	  	* Interruptable - this is when the process can be interrupted by signals from the
		  kernel 
		* Uninterruptable - this occurs when the process is waiting on hardware conditions. 
	* Stopped - this is when a process is stopped due to an interrupt e.g. is a process that is
	  debugged can be in stopped state. 
	* Zombie - this is a halted process that for some reason has a `task_struct` on task. this
	  is a dead process. 
* Scheduling - this information is kept in `task_struct` based on this the kernel decided which
  process to schedule next. 
* Identifiers - these are unique identifiers for not only the process but also the user and groups
  that the process has access to. The user and group ids help with the access to files and folders
  by the process. 
* Inter Process Communication (IPC) - the IPC mechanism of signals, pipes, semaphores and also the
  system v IPC mechanism like shared memory and message queues. 
* Links - each process has a lot of relationships with other processes e.g. parent, siblings,
  children all are stored as part of the `task_struct` each process in Linux apart from the init are
  cloned from the parent and then initialized. All processes in the system are held in a doubly
  linked list whose root is init process. 
* File System - each process keeps reference to all files that is has open. Apart from open files
  the `task_struct` by default has access to two inodes i.e. home directory of the process and the
  current working directory on which the process is currently (pwd) 
* Virtual Memory - each process has virtual memory that is mapped to the physical memory and this
  information is also part of the `task_struct`
* Process Specific context - this portion of the `task_struct` holds information for the CPU to
  context switch. The processor related information that is needed to push the process back to the
  CPU and continue the processing is held at this location. 
* Timers - these are clocks that the process uses to determine how long the process has been running
  and also the time related operations that the process needs to perform. 


## 2. Identifiers 

The Linux OS has a access rights system that is based on user and groups information. The resource
(file) that needs to be accessed is given access to a user and group which will have read, write,
execute permission combinations. 

The files that the process opens up need to be checked for access rights. So that process holds a
group vector which holds all the groups that the process belongs too. If the file that is opened by
the process belongs to the group the process can open the file and work on it. There are 4
indentifiers that the `task_struct` can hold: 

1. uid, gid - the user and group id for the user on behalf of which the process is running. 
2. effective uid and gid - there are processes that change their uid and gid from the values that
   were set at the time of starting use the effective uid and gid values. 
3. file system uid and gid - these are same as the effective uid and gid and is important when we
   have a NFS file mount system. 
4. save uid and gid - these are values saved when the process change the  uid and gid based on
   system calls.

## 3. Scheduling 

The job of the scheduling process in Linux is to figure out the best runnable process to assign a
CPU to work on. A policy based scheduling algorithm is used to figure the process to send to the
CPU. The entire state of the process in the CPU is stored in the `task_struct` structure so that
process can be restored after it gets back into the runnable state. 

The information regarding the scheduling that is kept in the `task_struct` are: 

* Policy - this is scheduling policy that would be applied to the process. There are 2 main types of
  processes normal and real time. The real time process will always take precedence over the normal
  tasks. In the real time process the policy could be either first in first out or round robin. 
* priority - This is the priority that the kernel gives the process. It is also the time (jiffies)
  that this process can run for when it is allowed to run. You can alter the priority by means of a
  system call. 
* rt_priority - this is the priority that is relevant to the real time processes only and it is used
  to define the realtive priority within the real time processes. 
* counter - this is a value of time that a process is allowed to run on the CPU. This value is first
  set to the priority value and the priority attribute is decremented on each clock tick. 


The scheduler runs the following steps each time it runs: 

1. kernel work - scheduler runs the bottom half handlers and work the schedular task queue. 
2. current process - current process is processed on the following rules: 
	* If the scheduling policy of the current process is round robin then it is put at the back
	  of the queue. 
	* If the tasks is interruptable and it has received a signal since the last time it was
	  scheduled then the state of the task is RUNNING. 
	* If process has timed out then the state of the process is immediately marked as RUNNING. 
	* if the current process is in running state then it will be in the same state. 
	* processes that were neither running nor interruptable are removed from the scheduling
	  queue. 
3. process selection - the scheduler looks through the processes on the run queue looking for the
   most deserving process to run. The main criteria of the process selection is the priority. The
   priority of a process is determined by the counter attribute in the begining. For real time
   processes the priority is counter + 1000 which makes them immediately more selection worthy. As
   the processes gets CPU time and the priority value is decremented, other tasks start becoming
   more eligible tasks. If processes have the same priority then the one earlier in the queue is
   chosen for execution. 

4. swap processes - The scheduler process runs as kernel mode process but in the context of the
   current process. If a the current process is not the most deserving process then the scheduler,
   when it runs will save the hardware (CPU) register and other system related info into the
   `task_struct` and then leaves the CPU. In a sense a snapshot of the running process at the time
   of the swap is taken and this is the state in which the CPU will come back once it returns to the
   CPU later. 
   If the processes that were used previously used virtual memory for operations then the system's
   page table will need updation. 

### 3.1 Scheduling in multiprocess systems 


