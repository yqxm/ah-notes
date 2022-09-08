- **例子**
	- ```C++
	  // Sales_data.h
	  struct Sales_data {
	    std::string isbn() const {return bookNo;}
	    Sales_data& combine(const Sales_data&);
	    double avg_price() const;
	    std::string bookNo;
	    unsigned units_sold = 0;
	    double revenue = 0.0;
	  }
	  
	  Sales_data add(const Sales_data&, const Sales_data&);
	  std::ostream &print(std::ostream&, const Sales_data&);
	  std::istream &read(std::istream&, Sales_data&);
	  ```
- **成员函数**
	- **声明和定义**
		- 成员函数必须在类的内部声明，但是成员函数体可以定义在类内也可以定义在类外。(例子中`avg_price`和`combine`函数定义在类外)
			- 当在类的外部定义成员函数时成员函数的定义必须与它的声明匹配。如果成员被声明成常量成员函数，那么它的定义也必须在参数列表后明确指定`const`属性。
			- 类外部定义的成员的名字必须包含它所属的类名。
		- ```C++
		  double Sales_data::avg_price() const {
		      if(units_sold) {
		          return revenue/units_sold;
		      }
		      return 0;
		  }
		  
		  Sales_data &Sales_data::combine(const Sales_data &rhs) {
		      revenue += rhs.revenue;
		      units_sold += rhs.units_sold;
		      return *this;
		  }
		  ```
	- **访问数据**
		- 成员函数通过一个名为`this`的隐式参数来访问调用它的对象。当调用一个成员函数时，`this`被请求该函数的对象地址初始化。
		- 在成员函数内部，可以直接调用该函数的对象的成员。任何对类成员的直接访问都被看作`this`的隐式引用。
		- 返回`*this`来返回调用函数的对象。
	- **`const`成员函数**
		- `const`的作用时修改隐式`this`指针的类型。把它的类型改变为`const Sales_data *const`。
		- 默认情况下，`this`的类型是指向类 类型非常量 版本的常量指针。比如它的类型是`Sales_data *const`。这意味着`this`不能绑定到一个常量对象上，因为常量对象的地址只能由指向常量的指针存放。
		- #+BEGIN_NOTE
		  常量对象，以及常量对象的引用或指针都只能调用常量成员函数。
		  #+END_NOTE
- **类相关的非成员函数**
	- **声明和定义**
		- 函数的声明和定义分开，如果函数在概念上属于类但不定义在类中，则它一般应与类声明(而非定义)在同一个头文件内。
	- ```C++
	  istream &read(istream &is, Sales_data& item) {
	      double price = 0;
	      is >> item.bookNo >> item.units_sold >> price;
	      item.revenue = item.units_sold * price;
	      return is;
	  }
	  
	  ostream &print(ostream &os, Sales_data& item) {
	      os << item.isbn() << " " << item.units_sold << " "
	      << item.revenue << " " << item.avg_price();
	      return os;
	  }
	  ```
		- IO类属于不能被拷贝的类型，只能通过引用传递。读取和写入的操作会改变流的内容，两个函数都是普通引用，而不是对常量的引用。
	- ```C++
	  Sales_data add(const Sales_data &lhs, const Sales_data &rhs) {
	      Sales_data sum = lhs; 		// 拷贝lhs给sum
	      sum.combine(rhs);
	      return sum;
	  }
	  ```
- **构造函数**
	- 类通过一个或几个特殊的成员函数来控制其对象的初始化过程，这些函数就是构造函数。只要类的对象被创建，就会执行构造函数。
	- **特点**
		- 构造函数名字和类名相同
		- 构造函数没有返回类型
		- 类似于重载，不同的构造函数必须在参数数量或参数类型上有所区别。
	- **默认构造函数**
		- 默认构造函数控制默认初始化过程，它无须任何实参，由编译器创建。
			- 初始化规则:
				- 如果存在类内的初始值，用它来初始化成员
				- 否则默认初始化该成员
		- 某些类不能依赖于合成的默认构造函数的原因:
			- 编译器只有在发现类不包含任何构造函数的情况下才会生成一个默认的构造函数。
			- 合成的默认构造函数可能执行错误的操作。因为内置类型或复合类型的对象被默认初始化可能会得到未定义的值。
			- 编译器无法为某些类合成默认的构造函数。比如，类中的某个其他类型的成员没有默认构造函数，编译器无法将它初始化，就无法合成默认构造函数。
		- 如果需要默认的行为，可以通过在参数列表后面加`= default`来要求编译器生成构造函数。
	- ```C++
	  // 默认行为的构造函数
	  Sales_data() = default;
	  // 构造函数初始值列表
	  Sales_data(const string &s): bookNo(s) {}
	  Sales_data(const string &s, unsigned n, double p) : bookNo(s), units_sold(n), revenue(p) {}
	  
	  // 在外部定义的构造函数
	  Sales_data(std::istream &);
	  ```
		- `= default`
			- 如果需要默认的行为，可以通过在参数列表写上`default`来要求编译器生成构造函数
		- 外部定义的构造函数
			- ```C++
			  Sales_data::Sales_data(istream &is) {
			      read(is, *this);
			  }
			  ```
- ### 拷贝、赋值和析构
	- 类还需要控制拷贝、赋值和销毁对象时发生的行为。
		- _发生拷贝_: 初始化变量以及以值的方式传递或返回一个对象
		- _发生赋值_: 使用赋值操作符会发生对象的赋值操作。
		- _发生销毁_: 当对象不再存在(一个局部对象会在创建它的块结束时被销毁)，当`vector`对象被销毁时存储在其中的对象也会被销毁。
	- 如果不主动定义这些操作，编译器会替我们合成。编译器生成的版本会对对象的每个成员执行拷贝、赋值和销毁操作。
	- #### 某些类不能依赖合成的版本
		- 当类需要分配类对象之外的资源时，合成的版本常常会失效。(如[[C++ --- 动态内存]])
		- 不过很多需要动态内存的类能(而且应该)使用`vector`对象或者`string`对象管理必要的存储空间。使用`vector`或者`string`的类能避免分配和释放内存带来的复杂性。
			- 也就是说，如果类包含`vector`或者`string`成员，则其拷贝、赋值和销毁的合成版本能够正常工作。
-