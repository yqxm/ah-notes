- ## Pointers
	- 指针类似于C，但go的指针没有地址运算
- ## `struct`
	- `struct`是字段的集合，通过`.`运算符获取值
	- 指向结构的指针也可以操作结构
	- 通过一系列结构字面值可以依次给结构的字段赋值，指明字段名的话可以不在乎赋值的顺序
	- ```go
	  type Vertex struct {
	    X int
	    Y int
	  }
	  
	  func main() {
	    av := Vertex{X: 1, Y: 2}
	    v := Vertex{1, 2};
	    a := v.X
	    p := &v
	    p.X = 10
	  }
	  ```
- ## Arrays
	- 数列的长度是它的类型的一部分，所以它不能被`resize`
	- `var a [10]int`
- ## Slices
	- `slice`是一个动态大小，元素视图灵活的数列。通常都使用`slice`而不是数列
	- 使用`[]T`声明一个T类型的slice
	- `a[low, high]`形成了一个`[low, high)`的切片
	- slice像是对数列的引用，它不存储数据，它只是对底层数组的一个描述。一个切片对数组的修改，其他对统一数组的切片也能看见。
	- 切片字面值像没有长度限制的数列字面值
		- ```go
		  package main
		  
		  func main() {
		    var v = []int {1, 2, 3, 4,6}
		    var ss = [] struct{int, int} { 
		      {1, 2},
		      {3, 4}
		    }
		  }
		  ```
	- 切片有`length`和`cap`两个属性，使用`len(s)`和`cap(s)`获取
	- 通过重新切片可以延长切片的长度
	- 切片的零值是`nil`，一个`nil`slice没有`length`和`capacity`，也没有底层数列
	- 可以使用`make`声明切片。`a := make([]int, 5)`，长度为5的切片。向`make`传递第三个参数来指定`cap`
		- ```go
		  func main() {
		  	a := make([]int, 5) // len 5, cap 5
		  
		  	b := make([]int, 0, 5) // len 0, cap 5
		  
		  	c := b[:2] // len 2, cap 5
		  
		  	d := c[2:5] // len 3, cap 3
		  }
		  ```
	- 可以声明切片的切片
		- ```go
		  func main() {
		  	board := [][]string {
		      	[]string{"_", "_","_"),
		          []string{"_", "_","_"),
		          []string{"_", "_","_")
		          }
		      }
		  }
		  ```
	- 使用`append`函数可以给切片添加元素
	- 可以使用`range`形式的`for`循环。每一次循环返回两个值，第一个值是`index`，第二个值是索引的值的复制。
		- ```go
		  var pow = []int {1, 2, 4, 8, 16, 32, 64, 128}
		  
		  func main() {
		    for i, v := range pow {
		      fmt.Println(i, v)
		    }
		  }
		  ```
		- 可以使用`_`来忽略`index`或者值。
- ## Maps
	- map的零值是`nil`，一个`nil`map没有键，也不可以添加键
	- 使用`make`函数初始化map。
		- ```go
		  
		  type Vertex struct{
		    Lat, Long float64
		  }
		  
		  var m map[string]Vertex
		  
		  func main() {
		    m = make(map[string]Vertex)
		    m["Bell labs"] = Vertex{
		      40.68433, -74.39967
		    }
		  }
		  ```
	- 可以使用字面值声明并初始化一个map
		- ```go
		  type Vertex struct {
		    Lat, Long float64
		  }
		  
		  var m map[string]Vertex{
		    "Bell labs": Vertex{40.68, -74.399},
		    "Google": Vertex{37.43, -122.08}
		  }
		  ```
		- 如果顶层的type只是一个名字，可以省略它。
		- ```go
		  var m map[string]Vertex{
		    "Bell labs": {40.68, -74.399},
		    "Google": {37.43, -122.08}
		  }
		  ```
	- 改变map
		- 插入或更新键的值`m[key] = elem`
		- 获取键的值`elem = m[key]`
		- 删除键`delete(m, key)`
		- 检查键是否有键`elem, ok := m[key]`，有键`ok`为true，无键`ok`为 false。如果无键，则`elem`的值是所在类型的零值。
			- ```go
			  if elem, ok := m[target]; ok {
			    	...
			  }
			  ```
- ## Function Values
	- 函数可以作为变量的值，参数和返回值
	- go的函数可以作为闭包，闭包是指函数可能从外部引用值。每个闭包绑定它自己的变量，不会影响别的闭包的绑定的值