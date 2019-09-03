# Interrupts and Interrupt Handling 

Systems often use interrupt controllers to group device interrupts together before passing on the
signal to the single interrupt pin on the CPU. The interrupt controller has mask and status
registers that control the interrupts. setting the bits on the mask register enables or disables the
interrupts and status register returns the active interrupt in the system. 

The handling of the interrupt in principle is same in a lot of cases: 

1. When a hardware interrupt occurs CPU stops processing instructions and jumps to locaion in memory
   that holds the interrupts handlers code. 
2. The handler code is run in a special mode called interrupt mode. Normally no interrupts can
   happen at this point in time.
3. However an interrupt can be interrupted when the system support hierarchy in the interrupts as
   well but that is generally not the case with all systems. 
4. Some CPU's have special registers that handle only the interrupt code. 
5. Once the interrupt handler has been executed the CPU returns to the process that it was running
   at the time the interrupt occured. 

## 1. Programmable Interrupt Controller 


