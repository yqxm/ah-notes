- #+BEGIN_CAUTION
  避免在不安全的宏里使用带有副作用的参数
  #+END_CAUTION
- 不安全的函数式宏定义是指它的展开结果0次或多次对参数求值。
- 永远不要在参数带有赋值、自增、自减、volatile access、IO或者其他有副作用的操作时调用不安全的宏。
- 建议不要创建不安全的函数式宏定义。
- ### 反例
	- ```C
	  #define ABS(x) (( (x) < 0 ) ? -(x) : (x))
	  
	  int main() {
	    int n = 1;
	    int m = ABS(++n);  // m 等于 3
	  }
	  ```
- ### 正例1
	- 自增操作在调用宏之前
	- ```C
	  #define ABS(x) (( (x) < 0 ) ? -(x) : (x))
	  
	  int main() {
	    int n = 1;
	    ++n;				//自增操作在调用宏之前
	    int m = ABS(n);
	    printf("%d", m);
	  }
	  ```
- ### 正例2
	- 使用`static inline`函数
	- ```C
	  #include <stdio.h>
	  
	  static inline int iabs(int x) {
	    return (((x) < 0) ? -(x) : (x));
	  }
	  
	  int main() {
	    int n = 1;
	    int m = iabs(++n);
	    printf("%d", m);
	  }
	  ```
- ### 正例3
	- 使用`_Generic`来满足多种类型，同样使用了`static inline`函数
	- ```C
	  #include <complex.h>
	  #include <math.h>
	    
	  static inline long long llabs(long long v) {
	    return v < 0 ? -v : v;
	  }
	  static inline long labs(long v) {
	    return v < 0 ? -v : v;
	  }
	  static inline int iabs(int v) {
	    return v < 0 ? -v : v;
	  }
	  static inline int sabs(short v) {
	    return v < 0 ? -v : v;
	  }
	  static inline int scabs(signed char v) {
	    return v < 0 ? -v : v;
	  }
	    
	  #define ABS(v)  _Generic(v, signed char : scabs, \
	                              short : sabs, \
	                              int : iabs, \
	                              long : labs, \
	                              long long : llabs, \
	                              float : fabsf, \
	                              double : fabs, \
	                              long double : fabsl, \
	                              double complex : cabs, \
	                              float complex : cabsf, \
	                              long double complex : cabsl)(v)
	    
	  void func(int n) {
	    /* Validate that n is within the desired range */
	    int m = ABS(++n);
	    /* ... */
	  }
	  ```