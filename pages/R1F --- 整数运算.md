note-type:: Reference
source-type:: book
source-id:: csapp3zh

- #+BEGIN_PINNED
  Bryant, R. E., & O’Hallaron, D. R. (2016). 深入理解计算机系统 (龚奕利 & 贺莲, Trans.; Third Edition). 机械工业出版社.p60
  #+END_PINNED
- 计算机整数运算具有局限性，当运算结果超出一定范围后会发生溢出。不过运算结果都是对位级运算结果的重新解释，可以根据位级运算推测结果。
- ## 无符号加法
	- 将$+^u_w$描述为$w$位的无符号加法。
		- $$
		  x+^u_w y=
		  \begin{cases}
		  x+y,\quad x+y<2^w \\
		  x+y -2^w, \quad 2^w \leq x+y < 2^{w+1}
		  \end{cases}
		  $$
	- ### 无符号数求反
		- $$
		  -^u_w x =
		  \begin{cases}
		  x, x = 0 \\
		  2^w - x, x > 0 \\
		  \end{cases} 
		  $$
- ## 补码加法
	- 将 $+^t_w$描述为$w$位的补码加法。
		- $$
		  x+^t_wy =
		  \begin{cases}
		  x+y-2^w, \quad 2^{w-1} \leq x+y \\
		  x+y, \quad -2^{w-1} \leq x+y \leq 2^{w-1} \\
		  x+y + 2^w, \quad x < -2^{w-1}
		  \end{cases}
		  $$
- ## 补码的非
	- 对$w$位的补码加法来说，$TMin_w$本身是它自己的加法逆元，其它的$x$都有$-x$作为它的加法逆元。
	- $$
	  -^t_wx=
	  \begin{cases}
	  Tmin_w, \quad x=Tmin_w \\
	  -x, \quad x > Tmin_w
	  \end{cases}
	  $$
	- 对$x$来说，位级的补码非表示为$\sim x + 1$
- ## 无符号乘法
	- 将$*^u_w$描述为无符号乘法
		- $$
		  x *^u_wy=(x \cdot y)\ mod \ 2^w
		  $$
- ## 补码乘法
	- 将$*^t_w$描述为补码乘法
	- 补码乘法和无符号乘法在位级等价，所以可以将先以无符号乘法计算，再将结果转成补码。
		- $$
		  x *^t_wy = U2T_w((x\cdot y)\ mod \ 2^w)
		  $$
- ## 乘以常数优化
	- 在大多数机器上整数乘法指令都相当慢。所以编译器对乘法进行了优化，用移位和加法运算的组合来代替常数因子的乘法。比如 $x*14$就可以重写为 $(x<<3)+(x<<2)+(x<<1)$。不过这种方法不能推广到除法。
- ## 除以2的幂
	- 除法运算比乘法运算更慢，所以当可以进行优化的时候，编译器一般都会对其进行优化。对于除以2的幂的运算，可以通过右移来进行优化。
	- ### 无符号数
		- 对于无符号数来说，直接进行逻辑右移就行。
	- ### 补码
		- 对于补码数来说，需要使用算数右移来保证符号位，并使用偏置量来保证舍入到0。
			- 当$x \geq 0$时， 和无符号数一样进行逻辑右移就行。
			- 当$x<0$时，使用
				- $$(x + (1 << k) -1) >> k$$
				- 产生向上舍入的值。它利用了$(x + (y-1))/y$可以产生向上舍入的值的属性。
-