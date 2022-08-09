- ## Initialization
	- Initialization: Provide initial values to variables.
		- Uniform initialization: Curly bracket initialization. Available for all types, immediate initialization at declaration. [[E.g. C++ Uniform Initialization]]
		- #+BEGIN_TIP
		  Use uniform initialization for every field of your non-primitive typed variables. But be careful not to use `vec(n, k)`.
		  #+END_TIP
	- Structured binding: lets you initialize directly from the content of a struct.
		- [[E.g. C++ Structured binding]]
- ## Reference
	- Reference: An alias for a named variable.
		- `=` usually is doing copy. Be aware of it. [[E.g. C++ classic reference-copy bug]]
		- `rvalue` is temporary, it cannot be referenced. [[E.g. C++ classic reference-rvalue error]]
	- `const`: indicates a variable can't be modified.
		- can't declare non-const references to const variables.
			- for `auto`, use `const auto`.
	- #+BEGIN_IMPORTANT
	  C++, by default, makes copies when we do variable assignment! use `&` if we need references.
	  #+END_IMPORTANT
	-