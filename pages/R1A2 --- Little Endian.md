note-type:: Reference
source-type:: book
source-id:: csapp3zh

- #+BEGIN_PINNED
  Bryant, R. E., & O’Hallaron, D. R. (2016). 深入理解计算机系统 (龚奕利 & 贺莲, Trans.; Third Edition). 机械工业出版社.p29
  #+END_PINNED
- 小端法是从最低有效字节到最高有效字节存储对象
	- 假设一个对象的内容为`0x01234567`，存储的地址为`0x100`。则它的排列是：
	- ```text
	  0x100 67
	  0x101 45	
	  0x102 23
	  0x103 01
	  ```