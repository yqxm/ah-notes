- ## Do we need an operating system?
	- ### If there is no OS.
		- _Benifit_
			- One could implement the system calls as a libriary, with which applications link. Then each applications can have its own library tailored to its needs.
			- Applications can directly interact with hardware resources and use those resources in the best way.
		- _Downside_
			- If there is more than one application running, the applications must be well-behaved.
				- One app have to trust other app have no bugs or malice code.
				- Need a good cooperative time-sharing scheme.
	- ### If there is an OS
		- _Convenience_: abstract hardware resources as services.
		- _Isolation_: Forbit applications from directly accessing sensitive hardware resources.
		- _No need to care about time-sharing_: Unix transparently switches hardware CPUs among processes, saving and restoring register state as neccessary.
- #+BEGIN_PINNED
  The Unix system-call interface is carefully designed to provide both programmer convenience and the possibility of strong isolation.
  #+END_PINNED