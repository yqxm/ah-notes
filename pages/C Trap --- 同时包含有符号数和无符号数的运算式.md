- 有符号数会被强制转换成无符号数
	- ```C
	  #include <stdio.h>
	  
	  int main() {
	      unsigned int i = 2;
	      int j = -1;
	      printf("%d\n", i-j);
	      printf("%d\n", i>j);
	  	return 0;	
	  }
	  
	  /* output
	   * 3
	   * 0
	   */
	  ```
	- 进行加减运算的结果影响不大，因为都是位模式的加减，并根据编码方式重新解释。
	- 对比较式来说，就容易导致错误的结果，如： `2 > -1`为0