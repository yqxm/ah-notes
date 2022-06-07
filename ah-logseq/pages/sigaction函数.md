- `sigaction`函数允许用户在设置信号处理时，指定他们想要的信号处理语义。
- ```C
  #include <signal.h>
  
  int sigaction(int signum, struct sigaction *act, struct sigaction *oldact);
  ```
- `sigaction`函数运用不广泛，更简单的方式是定义使用包装函数`Signal`，它调用`sigaction`。
- `Signal`包装函数设置一个信号处理程序，语义如下：
	- 只有这个处理程序当前正在处理的那种类型的信号被阻塞
	- 和所有信号实现一样，信号不会排队等待
	- 只要可能，被中断的系统调用会自动重启
	- 一旦设置了信号处理程序，它就会移植保持，直到`Signal`带着`handler`参数为`SIG_IGN`或者`SIG_DFL`被调用。