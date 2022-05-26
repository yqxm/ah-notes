- Use `typedef` to declare an alias for an existing type
- It never creates a new type.
- ```C
  typedef unsigned int uint_type;
  typedef signed char schar_type, *schar_p, (*fp)(void);
  ```
	- **First line:** Declare `uint_type` as an alias of `unsigned int`
	- **Second line:**
		- `schar_type` as an alias for `signed char`
		- `schar_p` as an alias for `signed char *`
		- `fp` as an alias for `signed char(*)(void)`