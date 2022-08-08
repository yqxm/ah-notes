- ## 构建
	- ```bash
	  cmake -B build			# 生成构建目录
	  cmake --build build		# 执行构建
	  ./build/app				# 运行
	  
	  # 在build目录里
	  make clean
	  make VERBOSE=1 			# 可以查看构建的详细目录
	  ```
- ## 概念
	- ### 目录路径
		- ```text
		  CMAKE_SOURCE_DIR
		  CMAKE_CURRENT_SOURCE_DIR
		  PROJECT_SOURCE_DIR
		  CMAKE_BINARY_DIR
		  CMAKE_CURRENT_BINARY_DIR
		  PROJECT_BINARY_DIR
		  ```
	- ### 源文件变量
		- 创建一个变量连接到所有的`cpp`文件，可以方便的对它们进行处理。
		- ```cmake
		  set(SOURCES
		  	src/Hello.cpp
		      src/main.cpp
		  )
		  
		  add_executable(${PROJECT_NAME} ${SOURCES})
		  ```
			- #+BEGIN_NOTE
			  也可以使用`GLOB`命令将源文件添加到`SOURCES`里。
			  `file(GLOB SOURCES "src/**.cpp")` 
			  #+END_NOTE
			- #+BEGIN_TIP
			  现代CMake并不推荐这么做，更推荐的做法是使用`add_XXX()`函数。尤其是当你添加新文件`GLOB`命令没有正确结果时。
			  #+END_TIP
	- ### 包含目录
		- 当要`include`多个文件夹时，可以使用`target_include_directories()`函数。它相当于将`-I/directory/path`传给编译器。
		- ```cmake
		  target_include_directories(target
		  	PRIVATE
		      	${PROJECT_SOURCE_DIR}/include
		  )
		  ```
	- ### 包含库
		-