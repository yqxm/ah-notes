- ## mypair.hpp
	- ```C++
	  template <typename First, typename Second> class MyPair {
	    public:
	    	First getFirst();
	    	Second getSecond();
	    
	    	void setFirst(First f);
	    	void setSecond(Second f);
	    private:
	    	First first;
	    	Second second;
	  }
	  ```
- ## mypair.cpp
	- #+BEGIN_WARNING
	  Must announce every member function is templated
	  #+END_WARNING
	- ```C++
	  #include <mypair.h>
	  
	  template <typename First, typename Second>
	  First Mypairr<First, Second>::getFirst() {
	  	return first;
	  }
	  
	  template <typename First, typename Second>
	  Second Mypairr<First, Second>::getSecond() {
	  	return second;
	  }
	  ```