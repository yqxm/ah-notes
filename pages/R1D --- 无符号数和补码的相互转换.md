note-type:: Reference
source-type:: book
source-id:: csapp3zh

- #+BEGIN_PINNED
  Bryant, R. E., & O’Hallaron, D. R. (2016). 深入理解计算机系统 (龚奕利 & 贺莲, Trans.; Third Edition). 机械工业出版社.p49-52
  #+END_PINNED
- C语言中有类型转换，对于无符号数和补码数来说，它们的类型转换就是对位级别的二进制数进行重新解释。
- ## 补码转无符号数
	- $$
	  T2U_w(x) =
	  \begin{cases}
	  x\ + \ 2^w,\quad x < 0 \\
	  x, \quad x \geq 0
	  \end{cases}
	  $$
- ## 无符号数转补码
	- $$
	  U2T_w(u) =
	  \begin{cases}
	  u\ ,  \qquad u \leq TMax_w \\
	  u-2^w\ ,\quad u > TMax_w
	  \end{cases}
	  $$
-