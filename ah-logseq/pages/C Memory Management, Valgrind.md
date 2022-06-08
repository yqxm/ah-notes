- **Big operations**
	- `get_bit`: 返回无符号数的第`n`位，`0 <= n <= 31`
		- 考虑先左移去除高位，再右移得到最低位。
			- 左移多少？ `31-n`
			- 右移多少？ `31`
		- 若先右移去除低位，左移后还需要右移一次，重复了一次操作。
	- `set_bit`: 将无符号数`x`的第`n`位设置为`v`
		- 考虑`v`变为`n`位数，然后与`x`进行和操作。
			- 错误: 和操作不能将0设置为1
		- 考虑若`v = 1`，则进行或操作可保证结果为1。若`v = 0`，则进行和操作可保证结果为0。
			- 这需要根据`v`的值分情况讨论，但题目不允许使用`if`等语句。
		- 考虑消除分情况讨论。因为目标是将第`n`位的值设置为`v`，可以不管原来第`n`为的值是什么，先给它设置一个值。
			- 将原值第`n`位设置为1，然后与`v`进行和操作。但对于不是只有一位的数来说，只是进行和操作明显不对。
				- 与移位后的第`n`位为`v`的数进行异或。然后再与0进行异或。
					- 错误: 若`v = 0`，两次异或后第`n`位为1。
			- 将原值第`n`位设置为0，然后与移位后的`v`进行或操作。成功！
	- `flip_bit`: 反转无符号数`x`的第`n`位
		- 利用前两个函数，先找到第`n`位的值，再设置为它的非，但不能用`!`操作符，则现将它左移31位去除高位，再右移31位。
- **Valgrind**
	- 使用Valgrind来寻找段错误
		- ```shell
		  valgrind ./linked_list
		  valgrind --leak-check=full ./linked_list
		  ```
		- `--leak-check`检查内存泄漏
- **Memory Management**
	- `bad_vector_new()`的错误
		- ```C
		  vector_t *bad_vector_new() {
		      vector_t *retval, v;
		      retval = &v;
		  
		      retval->size = 1;
		      retval->data = malloc(sizeof(int));
		      if (retval->data == NULL) {
		          allocation_failed();
		      }
		  
		      retval->data[0] = 0;
		      return retval;
		  }
		  ```
		- 创建的`vector_t`在栈上，当函数返回时，声明的`retval`会被立刻回收
	- `also_bad_vector_new()`的错误
		- ```C
		  vector_t also_bad_vector_new() {
		      vector_t v;
		  
		      v.size = 1;
		      v.data = malloc(sizeof(int));
		      if (v.data == NULL) {
		          allocation_failed();
		      }
		      v.data[0] = 0;
		      return v;
		  }
		  ```
		- 没有错误，但返回值很“重”。C是按值传递的，返回的`vector_t`进行了一次复制，对这种类型的返回值应当使用指针。
	- 正确的`vector_new()`
		- 在堆上创建`vector_t`，返回一个指针
			- 先`malloc`向量`vector_t`，再`malloc`向量里的“数组”`data`
	- `vector_delete()`
		- 先`free`里面的数组，再`free`整个向量
	- `vector_set()`
		- 如果设置的位置大于向量的大小，则需要将向量的`data`变大
			- 使用`calloc`来创建新的`data`。因为要求未被设置的位置全为0。使用`calloc`可以满足这一点。否则使用`malloc`后，访问未设置值的地方valgrind会提示以下错误
				- `Conditional jump or move depends on uninitialised value(s)`
				- `Use of uninitialized value of size *`
			- 变大之后需要将原来的值复制到新的`data`里
			- 将旧的`data`给`free`掉。