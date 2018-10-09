---
title:  Golang Log
date: 2017-10-31
tags:
    - 中文
    - golang
---

# 为什么我们需要Log？

Log, 意为原木、树干子，把树干子切开，上面有很多一圈一圈、密密麻麻的纹理；程序log也是一个一行一行、密密麻麻的文件，记录着程序运行过程中各种信息。

但这些信息不一定能全部用到，很多时候只是一种备考——发生了某种状况的时候才查阅，那不做log行不行呢？程序肯定不会因此而罢工，然而我们得知道，计算机程序在运行的时候依然是失控的，即使这个程序是我们一行一行敲出来的；由于计算机“感知”到的时间太快了，程序在一个须臾之间就能进行大量变化，我们人类根本无法以毫秒级来跟踪它。一个失控的程序，出错暴走的可能性是极高的，在加之我们更有许许多多失控的用户，下雨天，与这些失控的程序更配噢，出了个什么问题根本无法避免。所以我们得log，得让秉笔直书的logger忠实地记录下关键信息，出现问题时能够发现问题、解决问题、澄清事实和责任关系。一个健全的中大型程序，log当然是必不可少的。

# Package log

扯远了..这是Golang学习...

> Package log implements a simple logging package. It defines a type, Logger,
> with methods for formatting output. It also has a predefined 'standard' Logger
> accessible through helper functions Print\[f|ln\], Fatal\[f|ln\], and
> Panic\[f|ln\], which are easier to use than creating a Logger manually. That
> logger writes to standard error and prints the date and time of each logged
> message. Every log message is output on a separate line: if the message being
> printed does not end in a newline, the logger will add one. The Fatal
> functions call os.Exit(1) after writing the log message. The Panic functions
> call panic after writing the log message.

Golang的log包提供了一个简单的log功能——就是一个用多种格式化输出函数的Logger类，能够输出日期、时间、时区、文件名到标准输出、标准错误，并有一些预置函数用于在输出后进行一些其他操作。我们的重点在怎么定义我们自己的Logger类上。

## 创建 Logger

    func New(out io.Writer, prefix string, flag int) *Logger

使用log包中的New函数创建一个新Logger。

第一个参数`out`表明log要写往何处。多亏了Golang的接口机制（在Golang语言特性总记有一些讨论），我们能很方便的指定目的地。

第二个参数`prefix`是每一条log的前缀，比如我们可以在错误记录前加上前缀“Error:”。

第三个参数`flag`定义了每一条log的形式，说白了就是加上一些日期啊、时间啊、文件名啊之类的信息，用log包里的常量定义。

    E.g.

    logFile, _ := os.Open("myLogFile")
    myLogger := log.New(logFile, "[INFO]", log.Ldate|log.Ltime)

创建一个Logger，输出到文件`myLogFile`中，每一条记录规定加上前缀`INFO`，以及日期和时间。

## 使用 Logger

Logger创建完了，我们得记录啊。记录的时候用Logger自带的方法就行了，最简单的例如

    func (l *Logger) Println(v ...interface{})

复杂点的，像

    func (l *Logger) Fatalln(v ...interface{})

Fatal执行完之后会直接异常退出程序（执行`os.Exit(1)`)

还有什么`Panicln`，会在执行之后执行`panic()`（发生了很恐怖的错误，吓得我赶紧把程序关了）

以及一些`get`和`set`函数就没了，相当轻量级啊，不过能满足需要就是好东西。

实际使用一下：

    E.g.

    myLogger.Println("[r0beRT] Login")

运行之后看起来就像这个样子

    [INFO] 2017/10/31 15:11:39 [r0beRT] Login

log包简单易用，但是我们得自己定义我们的输出，要定义的有用而又优雅，没有多余信息且又面面俱到，输出结果还符合国际一般规范（不成文），才是大头啊，还是得自己去多看多用，积累经验呀。

# 参考

[log - The Go Programming Language]

[log - The Go Programming Language]: https://golang.org/pkg/log/
