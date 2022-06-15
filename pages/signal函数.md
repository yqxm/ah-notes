- 进程可以通过`signal`函数修改和信号相关联的默认行为。唯一的例外是`SIGSTOP`和`SIGKILL`，它们的默认行为不能修改。
- ```C
  #include <signal.h>
  typedef void (*sighandler_t)(int);
  
  sighandler_t signal(int signum, sighandler_t handler);
  ```
	- 如果`handler`是`SIG_IGN`，那么忽略类型为`signum`的信号
	- 如果`handler`是`SIG_DFL`，那么类型为`signum`的信号行为恢复为默认行为
	- 否则，`handler`就是用户定义的函数的地址，这个函数被称为**[[信号处理程序]]**，只要进程接收一个类型为`signum`的信号，就会调用这个程序。
	-