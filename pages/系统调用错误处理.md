- 调用Unix `fork()`时的错误检查
	- ```c
	  if ((pid = fork()) < 0) {
	      fprintf(stderr, "fork error: %s\n", strerror(errno));
	      exit(0);
	  }
	  ```
- 定义错误报告函数对上述代码简化
	- ```C
	  void unix_error(char *msg) {
	      fprintf(stderr, "%s: %s\n", msg, strerror(errno));
	      exit(0);
	  }
	  
	  void fun {
	    // ...
	    unix_error("fork error");
	    // ...
	  }
	  ```
- 使用错误处理包装函数，进一步简化
	- ```C
	  pid_t Fork(void) {
	      pid_t pid;
	  
	      if ((pid = fork()) < 0)
	          unix_error("Fork error");
	      return pid;
	  }
	  ```
	-
	-