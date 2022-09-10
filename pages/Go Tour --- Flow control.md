- `For`
	- go里只有一个循环结构`for`
	- ```go
	  for i := 0; i < 10; i++ {
	    fmt.Println(i)
	  }
	  ```
	- 初始语句是可选的和更新语句
	- ```go
	  sum := 10;
	  for ; sum < 1000; {
	    sum += sum
	  }
	  
	  for sum < 10000 {
	    sum += sum
	  }
	  ```
- `if`
	- `if`也可以使用短声明
	- ```go
	  if i:= 5; i < randIntn(10) {
	    fmt.Println("less than rand number")
	  } else {
	    fmt.Println("bigger than rand number")
	  }
	  ```
- `switch`
	- go的`switch`运行到第一个符合条件的`case`
	- `switch`没有设置条件的化相当于`switch true`
- `defer`
	- `defer`推迟一个函数的执行，直到周围的函数返回。
	- func main() {
	    defer fmt.Println("world")
	    fmt.Println("hello")
	  }
	  
	  // hello
	  // world
	- `defer`将函数推入一个栈中，遵循后入先出的原则
	- ```go
	  func main() {
	    for i := 0; i < 10; i++ {
	      defer fmt.Println(i)
	    }
	  }
	  ```