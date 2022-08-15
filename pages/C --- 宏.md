- ## 简单的宏
	- `#define 标识符 替换列表`
	- #+BEGIN_WARNING
	  不要在宏定义中放置额外的符号，否则它们会被当作替换列表的一部分。
	  如 `=`, `:`, `;`
	  #+END_WARNING
- ## 带参数的宏
	- `#define 标识符(x1, x2, ..., xn) 替换列表`
	- #+BEGIN_WARNING
	  在宏的标识符和左括号之间一定不能有空格，否则预处理器会认为在处理一个简单的宏。
	  #+END_WARNING
	- #+BEGIN_WARNING
	  多次计算宏的参数而导致的错误非常难以发现，所以尽量避免使用带有副作用的参数。
	  #+END_WARNING
		- ```C
		  #define MAX(x, y) ((x) > (y)? (x) : (y))
		  
		  int main() {
		    int i = 2;
		    int j = 1;
		    int c = MAX(++i, j); // int c = ((++i)>j? (++i):j)
		    // c 等于 4 
		  }
		  ```
	- #+BEGIN_IMPORTANT
	  圆括号是非常必要的。
	  #+END_IMPORTANT
		- 如果宏的替换列表有运算符，那么始终要将替换列表放在括号内。
		- 如果宏有参数，每个参数每次在替换列表中出现都要放在圆括号中。
- ## 编写技巧
	- 如果宏内有多个语句，需要使用`do {/* ... */} while(0)` 来包裹成一个语句。
	-