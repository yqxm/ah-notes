- `struct`: a group of named variables each with their own type. A way to bundle different types together.
	- you can use `struct` to pass and return group of information.
- initialize `struct`
	- ```c++
	  Student s;
	   s.name = "Frankie";
	   s.state = "MN";
	   s.age = 21;
	   //is the same as ...
	  Student s = {"Frankie"
	  , "MN"
	  , 21};
	  ```
- `std::pair`:  An STL built-in struct with two fields of any type.
	- It's a _template_: specify the types inside <> for each pair you make.
	- The field in `std::pair` are named `first` and `second`.
	- you can use `std::pair` to return status and result.
	- To avoid specifying types, use `std::make_pair(first, second)`