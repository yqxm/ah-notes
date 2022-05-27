- #+BEGIN_NOTE
  Every type in C is either an _object_ type or a _function_ type
  #+END_NOTE
- **Object:** An *Object* is storage in which you can represent values.
- **Variable:** *Variables* have a declared *type* that tells you the kind of object its value represents.
- **Function:** *Functions* are not objects but do have types. A function type is characterized by both its return type as well as the number and types of its parameters.
- **Pointer:** *Pointers* can be thought of as an *address*--a location in memory where an object or function is stored.
	- A pointer type is derived from a function or object type called the _referenced type_.
- **Scope:**
	- **Four types of scope:**
		- **File:** The declaration is outside any block or parameter list, then it has _file scope_, meaning the scope is the entire text file in which it appears as well as any files included after that point.
		- **Block:** The declaration is inside a block or within the list of parameters.
		- **Function Prototype:** The declaration appears within the list of parameter declarations in a function prototype(not part of function definition), which terminates at the end of the function declarator.
		- **Function:**  The area between the opening \{ of a function definition and its closing \}
	- Scope can be *nested*, _inner_ scope can access _outer_ scope, but not vice-versa.
- **Storage duration**
	- Four storage duration are available: automatic, static, thread, and allocated.
		- **automatic**: Objects declared within a block or as a function parameter.
		- **static:** Objects declared in file scope.
			- The lifetime of these objects is the entire execution of the program.
			- The stored value is initialized prior to program startup.
			- Use `static` specifier to make a variable in block scope have static storage duration.
		- **allocated:**
			- [[Dynamically Allocated Memory]]
- **Object Types**
	- **Boolean:** `<stdbool.h>`, use `bool`.
	- **Character:**
		- Different compilers will make `char` to be same with `unsigned char` or `signed char`.  But `char` is a separate type from the other two and is incompatible with both.
		- `wchar_t` have a larger character set.
	- **Numerical:**
		- Integer
		- **enum:** `enum` allows you to define a type that assigns names to integer values in cases with an enumerable set of constant values.
		- Floating-Point
		- **void:** The keyword `void` (by itself) means "cannot hold any value". `void *` means that the pointer can reference _any_ object.
- **Function Types**
	- _Function type is derived from the return type and the number and types of its parameters. The return type of a function cannot be array type.
	- ```C
	  int f(void);				// can not pass parameter.
	  int *fip();					// can pass any parameters. Don't use it.
	  void g(int i, int j);
	  void h(int, int);
	  ```
	- Specifying parameters with identifiers can be problematic if an identifier is a **macro**. But, it's a good practice for self-documenting code, so recommend it.
- **Derived Types**
	- _Derived types_ are types that are constructed from other types. These include **pointers**, **arrays**, **type definitions**, **structures**, and **unions**.
		- **Structures:** A _structure type_ contains sequentially allocated member objects. Each object has its own name and may have a distinct type.
			- ```C
			  //创建了 sigrecord 类型，并声明了 sigline对象，和指向这种类型的指针sigline_p
			  struct sigrecord {
			      int signum;
			      char signame[20];
			      char sigdesc[100];
			  } sigline, *sigline_p;
			  ```
			- To reference a member of an object of the structure type, use `.`
			- To reference a member of an pointer point to, use `->`
		- **Unions:** _Union types_ are similar to structures, except that the memory used by the member objects overlaps. Unions can contain an object of one type at one time, and an object of a different type at a different time, but **never** both objects at the same time.
			- `union` can make the same storage to be used for the members.
- **[[Type Definitions]]**
- **[[Type Qualifiers]]**