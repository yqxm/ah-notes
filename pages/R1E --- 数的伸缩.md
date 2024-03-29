note-type:: Reference
source-type:: book
source-id:: csapp3zh

- #+BEGIN_PINNED
  Bryant, R. E., & O’Hallaron, D. R. (2016). 深入理解计算机系统 (龚奕利 & 贺莲, Trans.; Third Edition). 机械工业出版社.p54-58
  #+END_PINNED
- ## 扩展
	- 数字有时候需要从较小的类型转换成较大的类型。
	- ### 无符号数的零扩展
		- 直接在数前面补0。比如:
			- 16位无符号数`0xFFFF`扩展成32位无符号数，结果为`0x0000FFFF`
	- ### 补码数的符号扩展
		- 若补码数为正数，前面补0。比如:
			- 16位补码数`0x0FFF`扩展为32位补码数，结果为`0x00000FFF`
		- 若补码数为负数，前面补1。比如:
			- 16位补码数`0x8FFF`扩展为32位补码数，结果为`0xFFFF8FFF`
			- 对于补码编码来说，负数前面补1并不影响数字的值。原来的符号位变为正数进行表达，其增加的值刚好可以和新增的符号位代表的值进行抵消，前后大小没有发生变化。
- ## 截断
	- 当将一个$w$位的数截断为$k$位的数时，将高位的$w-k$位丢弃，然后重新对其进行解释。
	- ### 无符号数的截断
		- 若将无符号数$x$截断为$k$位，其结果为$x \text{ mod } 2^{k}$。取$x$的低$k$位就行了。
	- ### 补码数的截断
		- 若将补码数$x$截断为$k$位，在解释的时候可以先将它转化为无符号数截断，然后对新生成的数用补码进行解释。
		-
-