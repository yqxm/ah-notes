- ## Load
	- When the RISC-V computer powers on, it initializes itself and runs a boot loader which is stored in read-only memory.
	- The boot loader loads the xv6 kernel into memory. Then, in machine mode, the CPU executes xv6 starting at `_entry` (kernel/entry.S:7). The RISC-V starts with paging hardware disabled: virtual addresses map directly to physical addresses
	- The loader loads the xv6 kernel into memory at physical address `0x80000000`. The reason it places the kernel at `0x80000000` rather than `0x0` is because the address range `0x0:0x80000000` contains I/O devices.
	- ```asm
	          # qemu -kernel loads the kernel at 0x80000000
	          # and causes each hart (i.e. CPU) to jump there.
	          # kernel.ld causes the following code to
	          # be placed at 0x80000000.
	  .section .text
	  .global _entry
	  _entry:
	          # set up a stack for C.
	          # stack0 is declared in start.c,
	          # with a 4096-byte stack per CPU.
	          # sp = stack0 + (hartid * 4096)
	          la sp, stack0
	          li a0, 1024*4
	          csrr a1, mhartid
	          addi a1, a1, 1
	          mul a0, a0, a1
	          add sp, sp, a0
	          # jump to start() in start.c
	          call start
	  spin:
	          j spin
	  ```
- ## Start
	- The instruction at `_entry` set up a stack so that xv6 can run C code. xv6 declares space for an initial stack, `stack0`, in the file `start.c`
	- The code at `_entry` loads the stack pointer register `sp` with address `stack0 + 4096`, the top of the stack, because the stack on RISC-V grows down. Now that the kernel has a stack, `_entry` calls into C code at `start`(kernel/start.c:21)
	- The function `start` performs some configuration that is only allowed in machine mode, and then switchs to supervisor mode.
		- To enter supervisor mode, RISC-V provides the instruction `mret`. This instruction is most often used to return from a previous call from supervisor mode to machine mode.
		- `start` isn't returning from such a call, and instead sets things up as if there had been one:
			- it sets the previous privilege mode to supervisor in the register `mstatus`,
			- it sets the return address to `main` by writing `main`'s address into the register `mepc`,
			- disable virtual address translation in supervisor mode by writing 0 into the page-table register `satp`,
			- and delegates all interrupts and exceptions to supervisor mode.
		- Before jumping into supervisor mode, `start` performs one more task: it programs the clock chip to generate timer interrups. With this housekeeping out of the way, `start` "returns" to supervisor mode by calling `mret`. This causes the program counter to change to `main`(kernel/main.c:11)
	- ```C
	  #include "types.h"
	  #include "param.h"
	  #include "memlayout.h"
	  #include "riscv.h"
	  #include "defs.h"
	  
	  void main();
	  void timerinit();
	  
	  // entry.S needs one stack per CPU.
	  __attribute__ ((aligned (16))) char stack0[4096 * NCPU];
	  
	  // a scratch area per CPU for machine-mode timer interrupts.
	  uint64 timer_scratch[NCPU][5];
	  
	  // assembly code in kernelvec.S for machine-mode timer interrupt.
	  extern void timervec();
	  
	  // entry.S jumps here in machine mode on stack0.
	  void
	  start()
	  {
	    // set M Previous Privilege mode to Supervisor, for mret.
	    unsigned long x = r_mstatus();
	    x &= ~MSTATUS_MPP_MASK;
	    x |= MSTATUS_MPP_S;
	    w_mstatus(x);
	  
	    // set M Exception Program Counter to main, for mret.
	    // requires gcc -mcmodel=medany
	    w_mepc((uint64)main);
	  
	    // disable paging for now.
	    w_satp(0);
	  
	    // delegate all interrupts and exceptions to supervisor mode.
	    w_medeleg(0xffff);
	    w_mideleg(0xffff);
	    w_sie(r_sie() | SIE_SEIE | SIE_STIE | SIE_SSIE);
	  
	    // configure Physical Memory Protection to give supervisor mode
	    // access to all of physical memory.
	    w_pmpaddr0(0x3fffffffffffffull);
	    w_pmpcfg0(0xf);
	  
	    // ask for clock interrupts.
	    timerinit();
	  
	    // keep each CPU's hartid in its tp register, for cpuid().
	    int id = r_mhartid();
	    w_tp(id);
	  
	    // switch to supervisor mode and jump to main().
	    asm volatile("mret");
	  }
	  
	  // arrange to receive timer interrupts.
	  // they will arrive in machine mode at
	  // at timervec in kernelvec.S,
	  // which turns them into software interrupts for
	  // devintr() in trap.c.
	  void
	  timerinit()
	  {
	    // each CPU has a separate source of timer interrupts.
	    int id = r_mhartid();
	  
	    // ask the CLINT for a timer interrupt.
	    int interval = 1000000; // cycles; about 1/10th second in qemu.
	    *(uint64*)CLINT_MTIMECMP(id) = *(uint64*)CLINT_MTIME + interval;
	  
	    // prepare information in scratch[] for timervec.
	    // scratch[0..2] : space for timervec to save registers.
	    // scratch[3] : address of CLINT MTIMECMP register.
	    // scratch[4] : desired interval (in cycles) between timer interrupts.
	    uint64 *scratch = &timer_scratch[id][0];
	    scratch[3] = CLINT_MTIMECMP(id);
	    scratch[4] = interval;
	    w_mscratch((uint64)scratch);
	  
	    // set the machine-mode trap handler.
	    w_mtvec((uint64)timervec);
	  
	    // enable machine-mode interrupts.
	    w_mstatus(r_mstatus() | MSTATUS_MIE);
	  
	    // enable machine-mode timer interrupts.
	    w_mie(r_mie() | MIE_MTIE);
	  }
	  ```
- ## First process
	- After `main`(kernel/main.c) initializes several devices and subsystems. it creates the first process by calling `userinit`(kernel/proc:226).
		- The first process executes a small program written in RISC-V assembly, make makes the first system call in xv6.
		- `initcode.S`(user/initcode.S:3) loads the number for the `exec` system call, `SYS_EXEC`(kernel/syscall.h:8), into register `a7`m and then calls `ecall` to re-enter kernel.
	- The kernel uses the number in register `a7` in `syscall`(kernel/syscall.c:133) to call desired system call. The system call table(kernel/syscall.c:108) maps `SYS_EXEC` to `sys_exec`, which the kernel invokes.
	- Once the kernel has completed `exec`, it returns to user space in the `/init` process.
	- `Init`(user/init.c) creates a new console device file if needed and the opens it as file descriptors 0, 1, and 2. The it starts a shell on the console.
	- The system is up
	-