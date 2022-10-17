note-type:: Reference
source-type:: book
source-id:: csapp3zh

- #+BEGIN_PINNED
  Bryant, R. E., & O’Hallaron, D. R. (2016). 深入理解计算机系统 (龚奕利 & 贺莲, Trans.; Third Edition). 机械工业出版社.p43-44
  #+END_PINNED
- ## 补码编码
	- 最高有效位表示补码是否为负数。
	- $$
	  B2O_w(\vec x) \doteq -x_{w-1} 2^{w-1} + \sum_{i = 0}^{w-2} x_i 2^i
	  $$
- ## 属性
	- $-2^{w-1} \sim 2^{w-1}-1$的数都有且只有一个w位的二进制表示与它对应。
	-