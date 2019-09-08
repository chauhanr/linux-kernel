# Kernel Mechanism

## Bottom Half Handling 

There are times during processing where the processor needs to send some tasks to queues to wait
becuase the kernel is busy doing other stuff. The interrupts are good example of such times. For
this very reason the bottom half handling was developed. There are 32 different kinds of bottom half
handlers. 

When the device driver or any other system in Linux needs to schedule work to be done later, it adds
work to appropriate system queues. 

## Task Queues

This queue is kernel's way of doing work later. Here the task is queued in the task queue
(linked list). This structure is used with the timer bottom half handling prodecure to complete the
work later. 

There are 3 type of task queues 

1. timer - this queue is used to keep task that need to run soon after the next system clock tick is
   possible. On every clock thick the timer will look into the timer queue and engage a bottom half
   handler is there is some pending work. 
2. immediate - these are tasks lesser in priority than the timer ones.They will be processed after
   timer tasks have been taken care of. 
3. scheduler - this is managed by the scheduler. 


