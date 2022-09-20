- **访问说明符**
	- 定义在`public`说明符之后的成员在整个程序内可以被访问，`public`成员定义类的接口。
	- 定义在`private`说明符之后的成员可以被类的成员函数访问，但是不能被使用该类的代码访问，`private`部分封装了类的实现细节。
- **友元**
	- 类可以允许其他类或者函数访问它的非公有成员，方法是令其他类或者函数成为它的友元。如果类想把一个函数作为它的友元，只需要增加一条以`friend`关键字开始的函数声明。
	- #+BEGIN_NOTE
	  友元只能出现在类定义的内部，但在类内出现的具体位置不限。友元不是类的成员也不受它所在区域访问控制级别的约束。
	  一般来说，最好在类定义开始或结束前的为止集中声明友元。
	  #+END_NOTE
	- #+BEGIN_TIP
	  友元声明仅仅指定了访问的权限，而非一个通常意义上的函数声明。如果希望类的用户能够调用某个友元函数，那么必须在友元声明之外再专门对函数进行一次声明。
	  #+END_TIP
- ```C++
  #include <iostream>
  
  using std::string;
  
  class Sales_data {
      friend Sales_data add (const Sales_data&, const  Sales_data&);
      friend std::ostream &print(std::ostream&, const Sales_data&);
      friend std::istream &read(std::istream&, Sales_data&);
  public:
      Sales_data() = default;
      Sales_data(const string &s): bookNo(s) {}
      Sales_data(const string &s, unsigned n, double p) : bookNo(s), units_sold(n), revenue(p) {}
      Sales_data(std::istream &);
      string isbn() const {return bookNo;};
      Sales_data& combine(const Sales_data& );
  private:
      double avg_price() const {return units_sold? revenue/units_sold: 0;}
      string bookNo;
      unsigned units_sold = 0;
      double revenue = 0.0;
  };
  
  Sales_data add (const Sales_data&, const  Sales_data&);
  
  std::ostream &print(std::ostream&, const Sales_data&);
  
  std::istream &read(std::istream&, Sales_data&);
  
  ```