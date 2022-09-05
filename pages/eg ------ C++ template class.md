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
- ## in mypair.h
	- #+BEGIN_WARNING
	  Must announce every member function is templated
	  #+END_WARNING
	- ```C++
	  
	  template <typename First, typename Second>
	  First Mypair<First, Second>::getFirst() {
	  	return first;
	  }
	  
	  template <typename First, typename Second>
	  Second Mypair<First, Second>::getSecond() {
	  	return second;
	  }
	  ```