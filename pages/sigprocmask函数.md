- ```c
  #include <signal.h>
  
  
  int sigprocmask(int how, const *set, sigset_t *oldset);
  int sigemptyset(sigset_t *set);						// 初始化set为空集合
  int sigfillset(sigset_t *set);						// 把每个信号添加到set中
  int sigaddset(sigset_t *set, int signum);			// 把signum添加到set
  int sigdelset(sigset_t *set, int signum);			// 从set中删除signum
  
  int sigismember(const sigset_t *set, int signum);
  ```
- `sigprocmask`函数改变当前阻塞的信号集合。具体的行为依赖于`how`的值：
	- `SIG_BLOCK`：把`set`中的信号添加到`blocked`中(`blocked=blocked | set`)。
	- `SIG_UNBLOCK`：从`blocked`中删除`set`中的信号(`blocked=blocked & ~set`)。
	- `SIG_SETMASK`： `block=set`。
- 如果`oldset`非空，那么`blocked`位向量之前的值保存在`oldset`中。