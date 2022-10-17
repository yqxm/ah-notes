note-type:: Reference
source-type:: book
source-id:: csapp3zh

- #+BEGIN_PINNED
  Bryant, R. E., & O’Hallaron, D. R. (2016). 深入理解计算机系统 (龚奕利 & 贺莲, Trans.; Third Edition). 机械工业出版社.p43-44
  #+END_PINNED
- ## 二进制转无符号数
	- $w$表示无符号数的位数，$x_i$表示第$i$位上的值
	- $$
	  B2U_w(\vec x)\doteq\sum_{i=0}^{w-1}x_i2^i
	  $$
-
- ## 属性
	- 介于$0 \sim 2^{w-1}$之间的数都有唯一的$w$位的编码与它对应。
	-