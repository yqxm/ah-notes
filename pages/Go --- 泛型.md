- ## 泛型函数
	- Go函数可以拖过类型参数来对多个类型使用。
	- `func Index[T comparable](s []T, x T) int`这个函数表示`s`是一个所有元素都支持`comparable`约束的`T`类型切片。 `x`也是`T`类型的。
		- `comparable`约束让元素支持`==`和`!=`操作。
		- ```go
		  func Index[T comparable](s []T, x T) int {
		  	for i, v := range s {
		  		// v and x are type T, which has the comparable
		  		// constraint, so we can use == here.
		  		if v == x {
		  			return i
		  		}
		  	}
		  	return -1
		  }
		  
		  func main() {
		  	// Index works on a slice of ints
		  	si := []int{10, 20, 15, -10}
		  	fmt.Println(Index(si, 15))
		  
		  	// Index also works on a slice of strings
		  	ss := []string{"foo", "bar", "baz"}
		  	fmt.Println(Index(ss, "hello"))
		  }
		  ```
- ## 泛型类型
	- ```go
	  package main
	  
	  import "fmt"
	  
	  func MapKeys[K comparable, V any](m map[K]V) []K {
	      r := make([]K, 0, len(m))
	      for k := range m {
	          r = append(r, k)
	      }
	      return r
	  }
	  
	  type List[T any] struct {
	      head, tail *element[T]
	  }
	  
	  type element[T any] struct {
	      next *element[T]
	      val  T
	  }
	  
	  func (lst *List[T]) Push(v T) {
	      if lst.tail == nil {
	          lst.head = &element[T]{val: v}
	          lst.tail = lst.head
	      } else {
	          lst.tail.next = &element[T]{val: v}
	          lst.tail = lst.tail.next
	      }
	  }
	  
	  func (lst *List[T]) GetAll() []T {
	      var elems []T
	      for e := lst.head; e != nil; e = e.next {
	          elems = append(elems, e.val)
	      }
	      return elems
	  }
	  
	  func main() {
	      var m = map[int]string{1: "2", 2: "4", 4: "8"}
	  
	      fmt.Println("keys:", MapKeys(m))
	  
	      _ = MapKeys[int, string](m)
	  
	      lst := List[int]{}
	      lst.Push(10)
	      lst.Push(13)
	      lst.Push(23)
	      fmt.Println("list:", lst.GetAll())
	  }
	  ```