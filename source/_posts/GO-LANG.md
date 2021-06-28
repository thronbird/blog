---
title: GO_LANG
date: 2020-06-28 17:51:45
tags:
---

## Language Specification

### Naming Convention

https://golang.org/doc/effective_go#package-names

- **pacakge name**: the importing package can talk about `bytes.Buffer`. It's helpful if everyone using the package can use the same name to refer to its contents, which implies that the package name should be good: short, concise, evocative. By convention, packages are given lower case, single-word names; there should be no need for underscores or mixedCaps. 
- **getter**: If you have a field called `owner` (lower case, unexported), the getter method should be called `Owner` (upper case, exported), not `GetOwner`. The use of upper-case names for export provides the hook to discriminate the field from the method. A setter function, if needed, will likely be called `SetOwner`. Both names read well in practice:
- **noun**: By convention, one-method interfaces are named by the method name plus an -er suffix or similar modification to construct an agent noun: `Reader`, `Writer`, `Formatter`, `CloseNotifier` etc.
- **MixedCaps**  use `MixedCaps` or `mixedCaps` rather than underscores to write multiword names.



### Slice & Array

https://golang.org/doc/effective_go#data

https://blog.golang.org/slices

- Most array programming in Go is done with slices rather than simple arrays.
- The size of an array is part of its type. The types [10]int and [20]int are distinct. 
- Slices hold references to an underlying array . and a length property. A `Read` function can therefore accept a slice argument rather than a pointer and a count: `func (f *File) Read(buf []byte) (n int, err error)`

- Think of a slice as a little data structure with two elements: a length and a pointer to an element of an array. You can think of it as being built like this behind the scenes:

```
type sliceHeader struct {
    Length        int
    ZerothElement *byte
}

slice := sliceHeader{
    Length:        50,
    ZerothElement: &buffer[100],
}
```

```
letters := []string{"a", "b", "c", "d"} //slice
letters := [...]string{"a", "b", "c", "d"} //array
type Transform [3][3]float64  // A 3x3 array, really an array of arrays.
type LinesOfText [][]byte     // A slice of byte slices.
```



### New & Make

https://golang.org/doc/effective_go#data

- New does not *initialize* the memory, it only *zeros* it. That is, `new(T)` allocates zeroed storage for a new item of type `T` and returns its address, a value of type `*T`. 

- `make(T, `*args*`)` serves a purpose different from `new(T)`. It creates slices, maps, and channels only, and it returns an *initialized* (not *zeroed*) value of type `T` (not `*T`). The reason for the distinction is that these three types represent, under the covers, references to data structures that must be initialized before use. A slice, for example, is a three-item descriptor containing a pointer to the data (inside an array), the length, and the capacity, and until those items are initialized, the slice is `nil`. For slices, maps, and channels, `make` initializes the internal data structure and prepares the value for use. For instance,   `make([]int, 10, 100)` allocates an array of 100 ints and then creates a slice structure with length 10 and a capacity of 100 pointing at the first 10 elements of the array. In contrast, `new([]int)` returns a pointer to a newly allocated, zeroed slice structure, that is, a pointer to a `nil` slice value.

- ```
  //unlike in C, it's perfectly OK to return the address of a local variable; the storage associated with the variable survives after the function returns. 
  func NewFile(fd int, name string) *File {
      if fd < 0 {
          return nil
      }
      f := File{fd, name, nil, 0}
      return &f
  }
  // expressions new(File) and &File{} are equivalent.
  ```



### Map

- Slices cannot be used as map keys, because equality is not defined on them. Like slices, maps hold references to an underlying data structure. If you pass a map to a function that changes the contents of the map, the changes will be visible in the caller.
- A set can be implemented as a map with value type `bool`. 



### Zero Values

https://golang.org/ref/spec#Program_initialization_and_execution

When storage is allocated for a [variable](https://golang.org/ref/spec#Variables), either through a declaration or a call of `new`, or when a new value is created, either through a composite literal or a call of `make`, and no explicit initialization is provided, the variable or value is given a default value. Each element of such a variable or value is set to the *zero value* for its type: ***`false` for booleans, `0` for numeric types, `""` for strings, and `nil` for pointers, functions, interfaces, slices, channels, and maps.***

These two simple declarations are equivalent:

```
var i int
var i int = 0
```

After

```
type T struct { i int; f float64; next *T }
t := new(T)
```

the following holds:

```
t.i == 0
t.f == 0.0
t.next == nil
```

The same would also be true after

```
var t T
```



### Defer

The arguments to the deferred function (which include the receiver if the function is a method) are evaluated when the *defer* executes, not when the *call* executes. 

```
for i := 0; i < 5; i++ {
    defer fmt.Printf("%d ", i)
}
// 4 3 2 1 0
```



### Printing

- you can use the catchall format `%v` (for “value”); the result is exactly what `Print` and `Println` would produce. When printing a struct, the modified format `%+v` annotates the fields of the structure with their names, and for any value the alternate format `%#v` prints the value in full Go syntax.
-  `%T`  prints the *type* of a value.



### Concurrency

- Go encourages a different approach in which shared values are passed around on channels and, in fact, never actively shared by separate threads of execution. Only one goroutine has access to the value at any given time. Data races cannot occur, by design.  **Reference counts** may be best done by putting a mutex around an integer variable, for instance. But as a high-level approach, using channels to control access makes it easier to write clear, correct programs.
-  -> Unix pipes(|).  go func()  Run In Background (&)



### Recover

- When `panic` is called, it immediately stops execution of the current function and begins unwinding the stack of the goroutine,running any deferred functions along the way. If that unwinding reaches the top of the goroutine's stack, the program dies. A call to `recover` stops the unwinding and returns the argument passed to `panic`. 
- deferred functions can modify named return values. 





## 语言机制

内存分配 span

Gmp调度模型

chan  同步的写法写异步



## FAQ

- bee工具下载： GO111MODULE=on go get -u github.com/beego/bee@v1.10.0
- bee api根据注解生成commentsRouter不生效：把新增的接口定义去掉，重新生成一遍，然后加上再重新生成
- 版本不一致：多个gopath最好去掉，保持一个gopath，方便管理

