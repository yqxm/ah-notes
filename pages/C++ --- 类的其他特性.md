- ## 类成员
	- ### 类型成员
		- 类型也可以是一个成员。由类定义的类型名字和其他成员一样存在访问限制，可以是`public`或者`private`。**类型成员必须先定义后使用**。
		- ```C++
		  class Screen {
		  public:
		      typedef std::string::size_type pos; //定义了类型pos作为public成员
		    	// using pos = std::string::size_type
		  private:
		      pos cursor = 0;
		      pos height = 0, width = 0;
		      std::string contents;
		  };
		  ```
	- ### 可变数据成员
		- 可以通过在变量的声明中加入`mutable`关键字使一个数据成员永远可变，即使在一个`const`成员内。
		- ```c++
		  class Screen {
		  public:
		    void some_member() const;
		  private:
		    mutable size_t access_ctr;	// 即使在一个const成员内也能被修改
		  }
		  
		  void Screen::some_member() const {
		    ++access_ctr
		  }
		  ```
	- ### 类数据成员初始值
		- 如果类数据成员需要使默认初始化的，最好的方式就是把这个默认值声明成一个类内初始值。
		- 类内初始值必须使用`=`的初始化方式或者花括号括起来的直接初始化形式。
		- ```C++
		  class Window_mgr {
		    private:
		    	std::vector<Screen> screens{Screen(24, 80, ' ')}
		  }
		  ```
- ## 返回`*this`的成员函数
	- ```c++
	  class Screen {
	  public:
	     	...
	      Screen &move(pos r, pos c);
	      Screen &set(char);
	      Screen &set(pos, pos, char);
	  private:
	     	...
	  };
	  
	  inline Screen &Screen::set(char c) {
	      contents[cursor] = c;
	      return *this;
	  }
	  
	  inline Screen &Screen::set(Screen::pos r, Screen::pos col, char c) {
	      contents[r*width + col] = c;
	      return *this;
	  }
	  ```
	- `set`成员的返回值是调用`set`的对象的引用。返回引用的函数是左值的，意味着这些函数返回的是对象本身而非对象的副本。可以进行如下的链式调用
		- ```C++
		  myScreen.move(4,0).set('#');
		  ```
	- 如果定义的返回类型不是引用，则`move`返回值将是`*this`的副本，接着调用`set`只能改变临时副本而不能改变`myScrenn`的值。
	- ### 从`const`成员返回`*this`
		- 假设定义了一个返回`*this`的`const`成员函数`display`。它像上面那样进行和其他普通成员进行链式调用时将发生错误。
			- #+BEGIN_NOTE
			  一个`const`成员函数如果以引用的形式返回`*this`，那么它的返回类型将是常量引用。
			  #+END_NOTE
			- ```c++
			  Screen myScreen;
			  // 如果display返回常量引用，则调用set将发生错误
			  myScreen.display(cout).set('*');
			  ```
	- ### 基于`const`的重载
		- 非常量版本的函数对于常量对象是不可用的，所以只能在一个常量对象上调用`const`成员函数。
		- #+BEGIN_TIP
		  对于公共代码使用私有功能函数
		  #+END_TIP
- ## 类类型
- ## 友元再探
	- ### 类之间的友元关系
		- 如果一个类指定了友元类，则友元类的成员函数可以访问此类包括非公有成员在内的所有成员。
		- ```C++
		  class Screen {
		    	friend class Window_mgr;
		  }
		  ```
		- #+BEGIN_NOTE
		  友元关系不存在传递性，每个类负责控制自己的友元类或友元函数。
		  #+END_NOTE
	- ### 成员函数作为友元
		- ```C++
		  class Screen {
		    	friend void Window_mgr::clear(ScreenIndex);
		  }
		  ```
		- 要想令某个成员函数作为友元，必须仔细组织程序的结构以满足声明和定义的彼此依赖关系。在这个例子中，必须按如下方式设计程序:
			- 首先定义`Window_mgr`类，其中声明`clear`函数，但不能定义它。在`clear`使用`Screen`的成员之前必须先声明`Screen`。
			- 定义`Screen`，包括对于`clear`的友元声明
			- 最后定义`clear`
	- ### 友元声明和作用域
		- 类和非成员函数的声明不是必须在它们的友元声明之前(它们作为别人的友元的声明之前)。当一个名字第一次出现在一个友元声明中时，我们隐式地假定该名字在当前作用域中是可见的。然而，友元本身不一定真的声明在当前作用域中。
		- 就算在类的内部定义该函数，我们也必须在类的外部提供相应的声明从而使得函数可见。换句话说，即使仅仅是用声明友元的类的成员调用该友元函数，它也必须是被声明过的。
		- ```C++
		  struct X {
		    friend void f() { }
		    X() { f(); } 						// 错误:f还没有被声明
		    void g();
		    void h();
		  };
		  
		  void X::g() { return f(); } 		// 错误:f还没有被声明
		  void f();							// 声明
		  void X::h() { return f(); }			// 错误:f还没有被声明
		  ```
-