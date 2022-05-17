- ## 概述
	- `x86-64`有256种异常类型。0~31号由Intel架构师定义，因此对于任何`x86-64`系统都是一样的。32~255号由操作系统定义。
	-
	- | 异常号 | 描述               | 异常类别   |
	  | ------ | ------------------ | :--------- |
	  | 0      | 除法错误           | 故障       |
	  | 13     | 一般保护故障       | 故障       |
	  | 14     | 缺页               | 故障       |
	  | 18     | 机器检查           | 终止       |
	  | 32~255 | 操作系统定义的异常 | 中断或陷阱 |
- ## Linux/x86-64 的故障或终止
	- 除法错误
		- 试图除以0，或者除法指令的结果对目标操作数来说太大。
	- 一般保护故障
		- 许多原因导致这个不为人知的故障。通常是程序引用了一个未定义的虚拟内存区域，或者程序试图写一个只读的文本段。
		- Linux不会尝试恢复这类故障。Linux Shell通常会把这类故障报告为*Segmentation fault*
	- 缺页
		- 处理程序将适当的磁盘上虚拟内存的一个页面映射到物理内存的一个页面，然后重新执行这条产生故障的指令。
	- 机器检查
		- 在导致故障的指令执行中检测到致命的硬件错误时发生。机器检查处理程序从不返回控制给应用
- ## Linux/x86-64系统调用
	- Linux提供了几百个系统调用。每个系统调用都有唯一的整数号，对应一个到内核中跳转表的偏移量。
	- C程序用`syscall`函数可以直接调用任何系统调用，但一般使用标准库的包装函数。
	-
	- | 编号 | 名字   | 描述                                  |
	  | ---- | ------ | ------------------------------------- |
	  | 0    | read   | Read file                             |
	  | 1    | write  | Write file                            |
	  | 2    | open   | Open file                             |
	  | 3    | close  | Close file                            |
	  | 4    | stat   | Get info about file                   |
	  | 9    | mmap   | Map memory page to file               |
	  | 12   | brk    | Reset the top of the heap             |
	  | 32   | dup2   | Copy file descriptor                  |
	  | 33   | pause  | Suspend process untill signal arrives |
	  | 37   | alarm  | Sechedule delivery of alarm signal    |
	  | 39   | getpid | Get process ID                        |
	  | 57   | fork   | Create process                        |
	  | 59   | execve | Execute a program                     |
	  | 60   | _exit  | Terminate process                     |
	  | 61   | wait4  | Wait for a process to terminate       |
	  | 62   | kill   | Send signal to a process              |