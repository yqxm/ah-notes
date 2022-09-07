- ## Methods
	- go没有类，但可以对类型定义方法。方法是有一个特殊接收器作为参数的函数，接收器在`func`关键字和方法名之间有它独有的参数列表。
	- #+BEGIN_IMPORTANT
	  方法只是有接收器参数的函数
	  #+END_IMPORTANT
	- ```go
	  type Vertex struct {
	    X, Y float64
	  }
	  
	  func (v Vertex) Abs() {
	    return math.Sqrt(v.X*v.X + v.Y*v.Y)
	  }
	  
	  func main() {
	    var v := Vertex{3, 4}
	    fmt.Println(v.Abs())
	  }
	  ```
	- 可以对非`struct`类型定义方法，你只能对当前包内定义的类型定义方法，不能对其他包内的类型定义一个带有接收器的方法。
		- ```go
		  type MyFloat float64
		  
		  func (f MyFloat) Abs() float64 {
		  	if f < 0 {
		  		return float64(-f)
		  	}
		  	return float64(f)
		  }
		  ```
	- ### 指针接收器
		- 如果要修改接收器所指的值或者避免在每一次调用方法时复制值到接收器，通常使用指针接收器。上面的接收器为值接收器。
		- ```go
		  type Vertex struct {
		    X, Y float64
		  }
		  
		  func (v *Vertex) Scale(k int) {
		    v.X *= k
		    v.Y *= k
		  }
		  ```
	- 通常来说，所有给定类型的方法都应该有值或指针接收者，但并不应该二者混用
- ## Interface
	- 接口是一个有一系列方法签名的类型。接口类型的变量可以保存任何实现了这些方法的值。
	- ```go
	  type Abser interface {
	  	Abs() float64
	  }
	  
	  func main() {
	  	var a Abser
	  	f := MyFloat(-math.Sqrt2)
	  	v := Vertex{3, 4}
	  
	  	a = f  // a MyFloat 实现了 Abser
	  	a = &v // a *Vertex 实现了 Abser
	  
	  	// 下面一行，v 是一个 Vertex（而不是 *Vertex）
	  	// 所以没有实现 Abser。
	  	a = v
	  
	  	fmt.Println(a.Abs())
	  }
	  
	  type MyFloat float64
	  
	  func (f MyFloat) Abs() float64 {
	  	if f < 0 {
	  		return float64(-f)
	  	}
	  	return float64(f)
	  }
	  
	  type Vertex struct {
	  	X, Y float64
	  }
	  
	  func (v *Vertex) Abs() float64 {
	  	return math.Sqrt(v.X*v.X + v.Y*v.Y)
	  }
	  ```
	- 类型通过实现一个接口的所有方法来实现该接口。既然无需专门显式声明，也就没有“implements”关键字。
	- 隐式接口从接口的实现中解耦了定义，这样接口的实现可以出现在任何包中，无需提前准备。
	- ### 底层接口值
		- 接口也是值，它可以像其他值一样传递，可以作为函数的参数或返回值。接口值可以看作包含值和具体类型的元组，接口值保存了一个具体底层类型的具体值，接口值调用方法会执行其底层类型的同名方法。
			- 如果接口的赋值有类型但值是nil，可以通过写一些方法来处理。保存了nil具体值的接口其自身并不为nil
			- ```go
			  func (t *T) M() {
			  	if t == nil {
			  		fmt.Println("<nil>")
			  		return
			  	}
			  	fmt.Println(t.S)
			  }
			  
			  func main() {
			  	var i I
			  
			  	var t *T
			  	i = t
			  	i.M()
			  
			    i = &T{"hello"}
			  	i.M()
			  }
			  ```
	- ### nil接口值
		- nil接口值既不保存值也不保存具体类型，为nil接口调用方法会产生**运行时错误**，因为接口的元组内并未包含能够指明该调用哪个**具体类型**的方法。
	- ### 空接口
		- 指定了零个方法的接口值被称为**空接口**。`interface{}`，空接口可以保存任何类型的值，常被用来处理未知类型的值。
	- ### 类型断言
		- 类型断言提供了访问接口值底层具体值的方式。
		- `t := i.(T)`该语句断言接口值`i`保存了具体类型`T`，并将其底层类型为`T`的值赋予变量`t`。
			- 如果`i`并未保存`T`类型的值，该语句会触发一个panic。
			- 判断接口值是否保存了一个特定的类型可以使用`t, ok := i.(T)`
	- ### 类型选择
		- 类型选择是允许几个类型断言的结构，和`switch`的语法很像，但case为类型，针对给定接口值的类型进行比较。
		- ```go
		  switch v := i.(type) {
		  case T:
		      // v 的类型为 T
		  case S:
		      // v 的类型为 S
		  default:
		      // 没有匹配，v 与 i 的类型相同
		  }
		  // v会保存所在类型的值
		  ```
	- ### Stringer
		- ```go
		  type Stringer interface {
		      String() string
		  }
		  ```
		- 实现`Stringer`接口来描述自己。
	- ### Errors
		- go使用error值来表示错误状态
		- ```go
		  type error interface {
		      Error() string
		  }
		  ```
		- 通常函数会返回一个error值，调用它的代码应当判断这个错误是否等于`nil`来进行错误处理
		- ```go
		  i, err := strconv.Atoi("42")
		  if err != nil {
		      fmt.Printf("couldn't convert number: %v\n", err)
		      return
		  }
		  fmt.Println("Converted integer:", i)
		  ```
		- `error`为`nil`时表示成功；非`nil`的`error`表示失败
	- ### Reader
		- io包指定了`io.Reader`接口，它从数据流的末尾读取数据。
		- `io.Reader`方法包含了`func (T) Read(b []byte) (n int, err error)`方法
		- `Read`用数据填充给定的字节切片，并返回填充的字节数和错误值。当字节流结束时，返回`io.EOF`错误
		-