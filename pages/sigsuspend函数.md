- ```c
  #include <signal.h>
  
  int sigsuspend(consg sigset_t *mask);
  ```
- `sigsuspend`函数暂时用`mask`替换当前的阻塞集合，然后挂起该进程，直到收到一个信号，其行为要么是运行一个处理程序，要么是终止该进程。
	- 如果它的行为是终止，该进程不从`sigsuspend`返回就直接终止。
	- 如果它的行为是运行一个处理程序，那么`sigsuspend`从处理程序返回，恢复调用`sigsuspend`时原有的阻塞集合。
- `sigsuspend`函数相当于下面代码的原子版本
	- ```C
	  sigprocmask(SIG_SETMASK, &mask, &prev);
	  pause();
	  sigprocmask(SIG_SETMASK, &prev, NULL);
	  ```