- ```C
  #include <setjmp.h>
  
  int setjmp(jmp_buf env);
  int sigsetjmp(sigjmp_buf env, int savesigs);
  ```
- `setjmp`函数在`env`缓冲区中保存当前调用环境，以供`longjmp`使用，并返回0。调用环境包括程序计数器、栈指针和通用目的寄存器。
- `setjmp`的返回值**不能**赋值给变量，不过可以用在`switch`或条件语句的测试中。
- `setjmp`被调用一次，返回两次
	- 第一次调用`setjmp`，调用环境保存在`env`中时。
	- 被`longjmp`调用时。