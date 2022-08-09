- `stream`: an abstraction for input/output. Streams convert between _data_ and the _string_ representation of data.
- ## Output Streams
	- Have type `std::ostream`
	- It can only _send_ data using the `<<` operator
		- It converts any type into string and _sends_ it to the stream.
	- `std::cout` is an output stream that goes to the console.
		- It's an _global constant object_ that you get from `#include <iostream>`
- ## Output File Streams
	- Have type `std::ofstream`
	- Only _send_ data using the `<<` operator
		- Converts data of any type into string and _sends_ it to the **file stream**.
	- Must initialize your own `ofstream` object linked to your file.
		- ```C++
		  std::ofstream out("out.txt");
		  // out is an ofstream that outputs to out.txt
		  out << 5 << std::endl;
		  ```
		- To use any other output streams, you must first initialize it.
- ## Input Streams
	- Have type `std::istream`
	- Can only _receive_ strings using the `>>` operator
		- _Receives_ a string from the stream and convert it to data.
	- ### `std::cin`
		- `std::cin` is the input stream that gets input from the console. It's a _global constant object_ that you get from `#include <iostream>`.
		- **Nitty Gritty Details**: `std::cin`:
			- First call to `std::cin >>` creates command line prompt that allows the user to type until they hit enter.
			- Each `>>` **Only** read until the next _whitespace_
				- Whitespace = tab, space, newline
			- Everything after the first whitespace gets saved and used the next time `std::cin >>` is called. The place its saved is called a **buffer**.
			- If there is nothing wating in the buffer, `std::cin >>` creates a new command line prompt.
			- Whitespace is eaten.
		- **When things go wrong**
			- Once an error is detected, the input stream's fail bit is set, and it will no longer accept input.
				- ```c++
				  string a; double b;
				  cin >> a >> b; // input is "blah blah"
				  cout << a << " " << b; // blah 0
				  ```
			- Read things until it's not required type.
				- ```C++
				  int a; double b;
				  cin >> a >> b; // input is 2.17
				  cout << a << " " << b; // 2 0.17
				  ```
			- **`std::cin` is dangerous to use on its own.**
	- ### `std::getline`
		- **Usage**
			- `std::getline(istream& stream, string &line)`
			- ```C++
			  std::string line;
			  std::getline(cin, line);
			  std::cout << line << std::endl;
			  ```
				- #+BEGIN_WARNING
				  Don't mix `>>` with `getline`!
				  - `>>` reads up to the next white space character and does not go past that white space character.
				  - `getline` reads up to next delimiter, and does go past that delimiter.
				  - Don't mix the two or bad things will happen!
				  #+END_WARNING
- ## Input File streams
	- Have type `std::ifstream`
	- Only receives strings using the `>>` operator.
		- Receives strings from a file and converts it to data of any type.
	- Must initialize your own `ifstream` object linked to your file.
- ## Stringstreams
	- Input stream: `std::istringstream` give any data type to the `istringstream`, it'll store it as a string!
	- Output stream: `std::ostringstream` make an `ostringstream` out of a string, read from it word/type by word/type.
-