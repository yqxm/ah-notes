- 调用函数一般比调用求等价表达式的值要慢一些，可以使用内联函数避免函数调用的开销。
  title:: C++ --- 内联函数
- 将函数指定为内联函数(`inline`)，通常是将它在调用点上“内联地”展开。例如:
	- ```C++
	  cout << shorterString(s1, s2) << endl;
	  // 展开为
	  cout << (s1.size() < s2.size() ? s1 : s2) << endl; 
	  ```
- 在函数的返回类型前加上关键字`inline`，就可以将它声明为内联函数了。
	- ```C++
	  inline const string & shorterString(const string & s1, 
	                                      const string & s2) {
	    return s1.size() < s2.size ? s1 : s2;
	  }
	  ```
- #+BEGIN_NOTE
  内联说明只是向编译器发出的一个请求，编译器可以忽略这个
  #+END_NOTE