- ## Goroutines
	- goroutine是go运行时管理的轻量线程
	- `go f(x,y z)`在当前goroutine解析`f`，`x`，`y`和`z`，在新的goroutine执行。
	- goroutine在相同的地址空间运行，所以访问共享内存必须同步。`sync`包提供了一些有用的基础类型。
- ## Channels
	- Channels是一个可以发送和接收数据的有类型的管道。通过`<-`操作。
		- ```go
		  ch <- v 		//发送 v 到 channel ch
		  v := <-ch 		//从 ch 接收数据并赋值给 v
		  ```
	- 创建channel`ch := make(chan int)`
	- 默认情况下，发送和接收会被阻塞直到另一边就绪。这让goroutine的同步不需要显式的锁或者条件变量。
	- ### Buffered Channels
		- 给`make`函数添加第二个参数可以设置channel的容量。`ch := make(chan int, 100)`
		- 当buffer满的时候，发送会被阻塞。当buffer空的时候，接收会被阻塞。
	- ### Range and Close
		- 发送者可以`close`一个channel来表示不会有数据被发送。接收者通过第二个赋值检测chnnel是否被close。`v, ok := <-ch`
		- 如果`ok`是`false`的话表示chnnel已经关闭。
		- `for i := range c`表示循环从`c`中读取值，直到channel被关闭。
		- #+BEGIN_NOTE
		  只有发送者可以关闭chnnel，而不是接收者。向关闭的chnnel发送数据会造成panic。
		  channel不是文件，并不是很需要关闭。只有当接收者必须被告知channel关闭才需要关闭channel。比如关闭一个`range`循环。
		  #+END_NOTE
		- ```go
		  func fibonacci(n int, c chan int) {
		  	x, y := 0, 1
		  	for i := 0; i < n; i++ {
		  		c <- x
		  		x, y = y, x+y
		  	}
		  	close(c)							//关闭 channel c
		  }
		  
		  func main() {
		  	c := make(chan int, 10)				
		  	go fibonacci(cap(c), c)				//创建goroutine
		    	for i := range c {					//循环读取 channel c
		  		fmt.Println(i)
		  	}
		  }
		  ```
- ## Select
	- `select`语句让一个goroutine等待多个通信操作
	- `select`会阻塞到某个分支可以继续执行为止，然后执行该分支。当有多个分支就绪时，随机选择一个执行。
	- ```go
	  func fibonacci(c, quit chan int) {
	  	x, y := 0, 1
	  	for {
	  		select {
	  		case c <- x:				// 向c中发送数据
	  			x, y = y, x+y
	  		case <-quit:				// 因为gorotine还没运行到21行，quit中没有数据， 被阻塞。
	  			fmt.Println("quit")	
	  			return
	  		}
	  	}
	  }
	  
	  func main() {
	  	c := make(chan int)
	  	quit := make(chan int)
	  	go func() {
	  		for i := 0; i < 10; i++ {
	  			fmt.Println(<-c)		// 当c中没有数据时被阻塞。
	  		}
	  		quit <- 0					// 只有当c中的10个数输出完成后，才有机会执行这条语句。quit中才会有数据。
	  	}()
	  	fibonacci(c, quit)
	  }
	  ```
	- ### Default Selection
		- 当没有其他项就绪时，`select`中的`default`会被运行。
		- ```go
		  func main() {
		  	tick := time.Tick(100 * time.Millisecond)	// 每100ms阻塞
		  	boom := time.After(500 * time.Millisecond)	// 每500ms阻塞
		  	for {
		  		select {
		  		case <-tick:
		  			fmt.Println("tick.")
		  		case <-boom:
		  			fmt.Println("BOOM!")
		  			return
		  		default:
		  			fmt.Println("    .")
		  			time.Sleep(50 * time.Millisecond)
		  		}
		  	}
		  }
		  
		  ```