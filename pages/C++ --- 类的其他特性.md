- ## 类型成员
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
	-