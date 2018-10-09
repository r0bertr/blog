---
title: Golang 语言特性总记
date: 2017-10-28
tags:
    - 中文
    - golang
---

* Slice
* Closure
* Interface
* ...

***

<br>看到了就会写上，不定更新。

## Slice

在Gotour里，`slice`是这样解释的：

> An array has a fixed size. A slice, on the other hand, is a
> dynamically-sized, flexible view into the elements of an array. In
> practice, slices are much more common than arrays.

简单翻译一下，`slice`就是一个动态数组，大家平时一般都用它，而不是用定长的数组。

先简单看一下`slice`的使用。

```
var s []int  // s is a nil slice

primesArr := [6]int{2, 3, 5, 7, 11, 13}  // primesArr is an array
primesSlice := []int{2, 3, 5, 7, 11, 13}  // primeSlice is a slice

primesSlice = primesSlice[0:2] // {2, 3}
primesSlice = primesSlice[2:4] // {5, 7}
```

然而，Gotour后还提到了，【`slice`是数组的引用】，那么这就是另外一个故事了。在Golang中，`array`和`slice`都有两个固有的属性`length`和`capacity`，`array`中这两者是恒等的，而`slice`中则不然。

```
var a [5]int
var s []int
var ss = make([]int, 5)
fmt.Println(len(a), cap(a), len(s), cap(s), len(ss), cap(ss))
    // 5 5 0 0 5 5
fmt.Println(len(ss[0:1]), cap(ss[0:1]))
    // 1 5
fmt.Println(len(ss[2:3]), cap(ss[2:3]))
    // 1 3
```

所以对于`slice`正确的理解，是将其看作一个数组的引用，而不是看作一个所谓的【动态数组】。

- 用数组初始化`slice`的时候，可以看作是引用了内存里的一个无名数组。

- `capacity`记录的是这个`slice`所引用的数组的长度，而`length`记录的是当前引用过来的元素个数。

- `slice`可进行随意的切分，但是从左边切是不可恢复的，从右边切是可恢复的，同时一旦这个`slice`的某个元素被更改，那么由这个`slice`切出来的所有`slice`中这个元素都将被更改。

- 直接创建的`slice`空变量是没有容纳能力的，要使用`make`方法；若要进行深复制，也要使用`make`方法来创建一个新的`slice`。

## Closure

闭包可能是学习Golang的第一个难题，但是闭包并不是Golang专属的特性，在很多其他语言中也有闭包，最典型的——Javascript.

闭包的问题难在难以界定——闭包到底是什么？能不能有一言可以蔽之？在Gotour中对闭包的解释是

> A closure is a function value that references variables from outside its body.
> The function may access and assign to the referenced variables; in the sense
> the function is "bound" to the variables.

简而言之，就是在函数内可以保持一个函数外的局部变量的值，且不需要这个值进行任何形式的传递。我认为这段话基本上把闭包最重要的特点描述出来了——利用函数对局部变量作用域进行灵活操作。

Gotour里给的例子是一个简单的返回函数闭包，实际上闭包的用途实际上远不止于此，典型的例子如Javascript中对于循环添加事件的处理，但是其中最核心的想法是不变的，就是将局部变量保持在一个【看似】无法访问它的作用域中。

但是实际上，编成的时候大部分用到闭包的情况都是靠经验判断的，我们平时应该多多积累，在我们编程卡住的时候，不妨想想，这地方是否能用一用闭包？

## Interface

`Interface`可以说是Golang里最最最重要的特点了，在Golang里实现【面向对象】特点全靠它，但它同时也是一个较难理解的概念。

让我们先来看Gotour是怎么说的

> An interface type is defined as a set of method signatures.
> A value of interface type can hold any value that implements those methods.

这两句简短的话道出了`Interface`大量的奥秘。首先，interface是一个方法签名集，即interface可以看作是一个集合，这个集合的元素是函数（函数签名）；其次，`interface`又可以作为任何实现了它方法的值的变量，这句话比较难理解，让我们看几个例子。

1. 首先是我们非常熟悉的`fmt.Println()`函数，可以看到，官方文档中，这个函数接受的参数是一系列的【空接口】。

    ```
    func Println(a ...interface{}) (n int, err error)
    ```

    回想一下我们使用这个函数的场景，任何类型的变量都可以作为参数传入这个函数中，没错，就是因为`interface{}`是一个函数签名集为空的接口，既然它没有任何方法需要实现，那么根据第二句话，任何类型的变量实际上都是`interface{}`类型，或者说都可以转化为`interface{}`类型。在很多处理未知类型的情况下（模板编程），Golang提供了`interface{}`给我们使用。

2. 文件读写

    ```
    file, _ := os.OpenFile(somePath, os.O_RDONLY, os.ModePerm)
    msg := make([]byte, 0)
    file.Read(msg)
    fmt.Println(string(msg))
    ```

    上面是一个简单的读文件过程，`os.OpenFile`返回一个`os.File`结构体，而调用的`os.File`结构体的`Read`方法是接口`io.Reader`里的方法，也就是说`os.File`实现了接口`io.Reader`.但是这并不足以体现接口在这里的作用，我们再来看下一段代码

    ```
	file, _ := os.OpenFile(somePath, os.O_RDONLY, os.ModePerm)
	jsonDecoder := json.NewDecoder(file)
	for jsonDecoder.More() {
		var obj SomeStruct
		jsonDecoder.Decode(\&obj)
        fmt.Println(obj)
	}
    ```
    上面是一个读取json文件的例子，注意到`json`包中的`NewDecoder`方法返回一个JSON解码器，能够将JSON文件中的条目写入一个结构体中，而这个方法接受的参数类型并不是`os.File`，而是`io.Reader`，也就是说任何实现了这个接口的结构体都可以被当作参数传入，因为只要实现了`io.Reader`的方法就【足够】成为一个JSON解码器了。这样一来，无论是`strings.Reader`还是`os.File`，甚至是你自己实现的自定义结构体，都可以成为JSON解码器，一定程度上说，这就是体现了面向对象里的继承和多态的思想，大大提高了开发效率。

接口，是一个函数签名的集合，更是也是一个可自定义的抽象类型，正是它的存在使得在Golang里应用面向对象的设计思想成为可能。

## 参考

[GoTour]

[GoTour]: https://tour.golang.org/
