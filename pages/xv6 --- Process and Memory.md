title:: xv6 --- Process and Memory

- A xv6 process consist of user-space memory and per-process state private to the kernel.
- xv6 _time-shares_ processes: It transparently switches the available CPUs among the set of processes waiting to execute.
	- When a process is not executing, xv6 saves its CPU registers, restoring them when it next runs the process.
- xv6 allocates moset user-space memory implicitly:
	- `fork` allocates the memory required for the child's copy of the parent's memory.
	- `exec` allocates enough memory to hold the executable file.
	- A process that needs more memory at run-time (perhaps for `malloc`) can call `sbrk(n) ` to grow its data memory by `n` bytes; `sbrk` returns the location of the new memory.