- 创建工程
	- ```
	  xmake create -l c -P ./hello
	  ```
- 描述文件
	- ```lua
	  target("hello")
	      set_kind("binary")
	      add_files("src/*.c") -- 添加src文件夹下的所有.c文件
	  ```
- 运行调试
	- ```
	  xmake run hello
	  
	  xmake run -d hello
	  ```