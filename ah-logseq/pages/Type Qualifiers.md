- #+BEGIN_NOTE
  The `_Atomic` type qualifier, available since C11, supports concurrent programs.
  #+END_NOTE
- **`const`**:  things that not modifiable.
	- C does not allow you to cast away the const if the original was declared as a const-qualified object.
- **`volatile`**
	- Static volatile-qualified objects are used to model memory-mapped I/O ports.
	- Static constant volatile-qualified objects model memory-mapped input ports.
	- The values stored in these objects may change without the knowledge of the compiler.
	- volatile-qualified types are used for communications with signal handlers and with setjmp/longjmp.
	- **should not be used for synchronization between threads.**
- **`restrict`**
	- Used to promote optimization.
	- Make compiler know that the  de-referenced object not pointed by other pointer. So compiler can make optimization.
	-