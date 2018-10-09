---
title: 领域建模基础
date: 2018-04-27
tags:
    - 中文
    - software engineering
    - uml
---

> A domain model is a system of abstractions that describes selected aspects of a sphere of knowledge, influence or activity (a domain). The model can then be used to solve problems related to that domain. The domain model is a representation of meaningful real-world concepts pertinent to the domain that need to be modelled in software. The concepts include the data involved in the business and rules the business uses in relation to that data.

领域模型是您在使用UML解决软件需求问题时最有力的武器之一，它不仅能够梳理业务逻辑，而且与数据库模型设计直接相关。一个好的领域模型能够使您在设计、编码阶段事半功倍。

## 构建领域模型

还记得在[使用UML工具进行业务分析](/2018/04/13/applying-uml)一文中的典例——早期预定旅馆业务`Asg_RH`，本例将会对此业务中主要的用例进行领域建模。

先回顾一下用例模型，

![](/assets/images/2018-04-13-applying-uml/20180413-181658.png)

对应文档中的Task2，我们对其中主要的4个用例`Get a list of specified hotels`，`Choose a hotel`，`Submit a reservation request to shopping basket`和`Pay for the reservations in the shopping basket by credit cards`进行领域建模。

|{% asset_img 20180427-105312.png %}|{% asset_img 20180427-110924.png %}|
|:-:|:-:|
|{% asset_img 20180427-111013.png %}|{% asset_img 20180427-111102.png %}|

## 构建E-R模型

根据领域模型，构建E-R模型就相当容易了。数据库模式设计是一个令人头大的问题，尤其是需求频繁更改的时候，更改数据库表的模式往往牵一发而动全身。良好的领域建模，能够规避一部分此类问题。

如下是根据上述领域模型，使用`MySQL Workbench`建立的E-R模型，

{% asset_img 20180427-112201.png %}

E-R模型与数据库之间相关，您可以直接由E-R模型生成本例中对应的{% asset_link Asg_RH.sql SQL语句 %}，实现领域模型-ER模型-SQL代码的流水线。

## 比较

领域模型和E-R模型是完全不同的两回事，领域模型是高层次的模型，具有许多功能；E-R模型重点关注具体的数据库模式。除了进行数据库模式设计，您还可以通过领域模型来

- 梳理业务逻辑
- 驱动开发
- 解释用例
- ...

这些都是数据库逻辑模型无法做到的。