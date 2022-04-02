# Concurrency

## intro

Thread: an abstraction for a running process.

A **multi-thread** program has more than one point of execution.
Each thread is very much like a separate process, except for that they share the same address space and can access the same data.

The state of a single thread is very similar to  that of a process. It has a program counter(PC) and has its own private set of registers it uses for computation.

**context switch**: The register status of T1 must be saved and the register of T2 restored before running T2. With process, we saved state to a process control block(**PCB**);now, we'll need one or more thread control blocks(**TCBs**) to store the state of each thread of a process.

### difference between threads and process

The major difference threads compare to process is that the address space remains the same.

The other major difference is about stack. Each thread has a stack. Any stack-allocated variables, parameters, retrun values and other things that we put on the stack will be placed in what is sometimes called **thread-local** storage, the stack of the relevant thread.

## Why use threads?

- Parallelism
- To avoid blocking program progress due to slow I/O
