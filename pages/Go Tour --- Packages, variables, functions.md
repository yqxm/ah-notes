- ### `main`
	- 程序从`package main`开始运行
- ### `import`
	- 用`import`导入其他的包，使用参数化的方式是一个好风格
	- ```go
	  package main
	  
	  import (
	    "fmt"
	    "math/rand"
	  )
	  
	  func main() {
	    fmt.Println("Hello, ", rand.Intn(10))
	  }
	  ```
- ### Exported names
	- 大写字母开头的名字可以被导出，如`math.Pi`
- ### Functions
	- 函数可以传递零个或多个参数，参数类型在参数名之后
	  collapsed:: true
		- ```go
		  package main
		  
		  import (
		    "fmt"
		    "math/rand"
		  )
		  
		  func add (a int, b int) int{
		    return a+b
		  }
		  
		  func main() {
		    fmt.Println("sum: ", add(1, 2))
		  }
		  ```
	- 如果连续的函数参数是一个同一个类型，可以省略类型声明并只在最后一个参数声明
	  collapsed:: true
		- ```go
		  func add (a, b int) int {
		    return a+b
		  }
		  ```
	- 返回类型可以有多种
		- ```go
		  func nameAndAge() (string, string) {
		    return "Huang", "20"
		  }
		  ```
	- 命名返回值
		- 可以对返回值进行命名
		- ```go
		  func namedReturn() (fr string, sr string) {
		    fr = "first return"
		    sr = "second return"
		    return
		  }
		  ```
- ### Variables
	- 使用`var`声明变量，变量声明可以在包作用域或者函数作用域
		- ```go
		  package main
		  
		  import (
		    "fmt"
		  )
		  
		  var c, python, java bool
		  
		  func main() {
		    fmt.Println("Package:", c, python, java)
		    
		    var i, k int
		    fmt.Println("function:", i, k)
		  }
		  
		  
		  ```
	- 声明和初始化可以同时进行`var i, j int = 1 , 2`
	- 在函数内可以使用短声明`:=`代替`var`。在函数外每个语句都必须以一个关键字开头，所以在函数外不能使用短声明。
		- ```go
		  package main
		  
		  import (
		    "fmt"
		  )
		  
		  func main() {
		    s := "hello world"
		    fmt.Println(s)
		  }
		  ```
- ### Basic types
	- `int`，`uint`和`uintptr`在32位系统上有32位，64位系统上有64位。如果没有特殊原因，尽量用`int`来表示整形。
	- ```text
	  bool
	  
	  string
	  
	  int  int8  int16  int32  int64
	  uint uint8 uint16 uint32 uint64 uintptr
	  
	  byte // alias for uint8
	  
	  rune // alias for int32
	       // represents a Unicode code point
	  
	  float32 float64
	  
	  complex64 complex128
	  ```
	- 零值
		- 变量没有初始值的话将会被赋予零值。
			- 数字类型被赋0
			- 布尔类型被赋`false`
			- `string`类型被赋`""`(空字符串)
	- 类型转换
		- 在go中所有类型转换都有显式声明
	- 类型推断
		- go会根据右值进行类型推断。如果右值是没有声明类型的数字字面值，则根据精度判断类型
	- 常量
		- 使用`const`声明常量，常量可以是 字符，`string`，`boolean`，或者数字类型。
		- 不能使用短声明声明常量
		- 数字常量是高精度类型，没有声明类型的常量