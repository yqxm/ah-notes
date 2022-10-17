note-type:: Reference
source-type:: book
source-id:: csapp3zh

- #+BEGIN_PINNED
  Bryant, R. E., & O’Hallaron, D. R. (2016). 深入理解计算机系统 (龚奕利 & 贺莲, Trans.; Third Edition). 机械工业出版社.p29
  #+END_PINNED
- 大端法是从最高有效字节到最低有效字节存储对象
	- 假设一个对象的内容是`0x12345678`，存储在`0x100`处。则它的排列是：
	- ```text
	  0x100 12
	  0x101 34
	  0x102 56
	  0x103 78
	  ```