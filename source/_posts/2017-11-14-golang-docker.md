---
title: Golang Docker 简易使用
date: 2017-11-14
tags:
    - 中文
    - golang
---

## 什么是Docker？

### 摘要

Docker的概念类似于虚拟机，使用方式上和Git很像。一个虚拟机运行一个操作系统，在这个操作系统上可以运行各种各样的程序，主操作系统上的虚拟机充当一个虚拟硬件的角色，但是有一个问题：程序的运行往往需要很多依赖支持，例如C的运行时、python应用的python环境、java应用的JVM等等，在虚拟机环境下，这些运行环境还是要用户去手动配置的；而Docker更高一层，它充当的不是虚拟硬件，而是虚拟运行环境；只要把应用放入Docker中，运行时Docker会自动配置好所需的运行环境，用户在一个操作系统下就算不安装任何环境，只要装一个Docker，任何应用程序都可以直接跑起来。

### Docker的结构

实际使用过程中，有两个关键性的对象——`Image`和`Container`。

> An image is a lightweight, stand-alone, executable package that includes everything needed to run a piece of software, including the code, a runtime, libraries, environment variables, and config files.
> A container is a runtime instance of an image—what the image becomes in memory when actually executed. It runs completely isolated from the host environment by default, only accessing host files and ports if configured to do so.
> Containers run apps natively on the host machine’s kernel. They have better performance characteristics than virtual machines that only get virtual access to host resources through a hypervisor. Containers can get native access, each one running in a discrete process, taking no more memory than any other executable.

- `Image`是一个轻量级的、独立的、可执行的包，它包含了所有软件运行所需要的组件。当然，我们在制作`Image`的时候并不需要手动去下载那些组件啊依赖什么的，因为那些组件在网络上也已经有现成的`Image`了，我们只管写代码，之后交给Docker去自动合成就好了。
- `Container`是一个`Image`的运行实例。`Image`和`Container`的关系就好像程序和进程，把`Image`跑起来就是一个`Container`了，这个玩意儿里面包括了所有需要的运行环境，程序能毫无阻拦地在里面运行，并且这个东西是运行在主操作系统上的，相较于虚拟机来说肯定有性能上的优势。

## 为什么要使用Docker？

对于一个技术，我们要正确使用它就必须清除它是为了解决什么问题。在linux系统下玩过开发的应该都有体会，安装环境是多么令人头疼的一件事，每次都要去别人官网看一大堆教程，安装的时候还可能遇到这样那样的错误，在stackoverflow里又要逛好久；这倒不是什么事儿，装完了就一劳永逸了。关键是当我们需要在一个别的什么新的操作系统下运行一个程序，难道还得重复一遍之前的痛苦？一个典型的例子就是部署服务器了，在vps服务商那儿高高兴兴地买了个vps，登上去之后把自己的server一拷，发现根本跑不起来，又是缺这又是缺那的，这个时候我们终于意识到了，Docker这个技术是多么的伟大这个事实..所以说为什么现在各大云服务商都推出了容器服务，你根本就接触不到操作系统，把Docker的image给人一传，你的服务器马上就能工作了。（AWS的注册要国际银行卡啊..臣没有呀..）

## Docker的使用

### 安装（Ubuntu 16.04 LTS）

当然了，就算是项羽也不能把自个儿拎起来；Docker本身还是得我们自己手动安装的。实话说，安装过程还是看官方教程比较好，博客的东西都不具有时效性啊。

1. 卸载旧版Docker

    话说开天辟地之时，Docker是不分社区版和企业版的，安装方式也没那么复杂，可是后来社区版和企业版一分，安装方式也变了，还得我们自己卸载旧版本。不过第一次安装的话这一步就不用管了。

        $ sudo apt-get remove docker docker-engine docker.io

2. 安装Docker CE

    我当然是安装社区版了；过程其实很简单，把下面一串命令麻溜地敲进去就好了。

        $ sudo apt-get update

        $ sudo apt-get install \
        apt-transport-https \
        ca-certificates \
        curl \
        software-properties-common

        $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

        $ sudo add-apt-repository \
        "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
        $(lsb_release -cs) \
        stable"

        $ sudo apt-get update

        $ sudo apt-get install docker-ce

    这几个命令大概就是先安装依赖，然后创建一个专属的包仓库，再从这个仓库里安装（这样更新卸载啥的都很方便）；最后一个命令才是真正的安装。BTW，docker的服务器要在国内访问可能有些困难，这个自己想办法解决吧。

    另外，计算机架构不同的话要改一改最后一个命令里的`arch=amd64`这个部分，具体等于啥，我也不懂，不过一般要改这个的人应该都不用看这个博客了..

    当然你也可以从官网下个deb，然后每次更新都重新下新的deb..

3. 卸载Docker CE

        $ sudo apt-get purge docker-ce
        $ sudo rm -rf /var/lib/docker

    一般来说这辈子都没机会输入这两个命令。

### 安装(MAC OS X)

在OSX系统下，docker官方提供了简单好用的图形安装包。访问[Docker-CE下载页](https://www.docker.com/community-edition)，下载对应的MAC版安装包安装即可。

安装完成后，在状态栏中会运行Docker伴随程序，可以进行一些图形化配置，但最主要的还是在伴随程序开启的状态下才能够使用的命令行Docker程序。

### 简单使用

今天我们的目标是把一个Go语言的hello world服务器在docker里运行一波。

首先看一下源代码目录：

    simple_http
    > .git
    > Dockerfile
    > main.go
    > README.md

除了`Dockerfile`，都和docker没啥关系了。Dockerfile是一个配置文件，所有必须的组建和依赖都在这里面进行配置，例如安装python依赖包之类的；除此之外，还有一些功能性的东西，比如导出端口啥的。

鉴于Go语言先天优势，它实在是没啥依赖，Dockerfile里只用写两行就行了

    FROM golang:onbuild
    EXPOSE 8080

- `FROM`条目一般来说是必需的，官方有各种各样的语言镜像，提供了最基础的运行环境。`FROM`后跟着一个镜像名，一个镜像名后加个冒号表示TAG，也就是说`golang`是镜像名，`onbuild`是TAG。至于TAG是干啥的，我也不太清楚..
- `EXPOSE`是导出的端口。Docker里的应用程序实际上仍然是在沙盒里运行的，即使程序里监听了8080端口，也是监听的那个沙盒的8080端口，我们要把沙盒的8080端口和主机的8080端口连在一起，才能访问。

什么？你说你用的是python？那Dockerfile可有的写了，善用官方文档和搜索引擎..

写完Dockerfile之后，就可以创建镜像了，cd到这个目录然后敲

    $ sudo docker build -t xxx:yyy .

- `-t`表示命名，xxx表示镜像名，yyy表示TAG

跑完之后，一个镜像就创建好了，可以敲入如下命令来查看

    $ sudo docker images

创建完镜像之后，就是运行了，敲入如下命令

    $ docker run --publish 8080:8080 --name test --rm xxx

- `--publish xxxx:yyyy` 将主机的xxxx端口绑定到container的yyyy端口。
- `--name xxx` 给我们的container起个名字（重要）。
- `--rm` 在运行完成后删除镜像。
- `xxx` 刚才创建的镜像名

这样我们的程序就在docker里跑起来了，而且监听了主机的8080端口，可以curl一下localhost:8080来试试效果。

## 附录

### main.go

    package main

    import (
    	"fmt"
    	"log"
    	"net/http"
    	"strings"
    )

    func main() {
    	// A simple http server.
    	//
    	// This server send "Hello user" to client based on URL.
    	// Example:
    	//
    	// localhost:8080/foo/bar
    	// > Hello foo bar!
    	//
    	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
    		fmt.Fprintf(w, "Hello %s!\n",
    			strings.Join(strings.Split(r.URL.String(), "/")[1:], " "))
    	})

    	fmt.Println("Listening to port 8080.")
    	log.Fatal(http.ListenAndServe(":8080", nil))
    }

## 参考

[Docker Docs]

[Deploying Go servers with Docker - The Go Blog]

[Docker Docs]: https://docs.docker.com/
[Deploying Go servers with Docker - The Go Blog]: https://blog.golang.org/docker
