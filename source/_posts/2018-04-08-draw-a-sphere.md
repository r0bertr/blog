---
title: 绘制球体
date: 2018-04-08
toc: false
tags:
    - 中文
    - computer graphic
---

三角形、线、点是OpenGL3提供的基本图元，其他图形，包括再平常不过的矩形，都要通过这些基本图元构建而成。球体是3D中具有代表性图形，您可以通过以下的算法绘制一个任意大小、优美的球体。

这个算法的基本思想是，“膨胀”一个正八面体。八面体是由八个正三角形构成的空间几何体，我们令八面体的中心位于球心，八面体内切于球体，那么八面体表面上的任意一点可以延伸到球面上。这样做的好处是，我们可以在正三角形的内部绘制子三角形，以让三角形作为基本单位扩散至球体表面，方便以三角形作为基本图元进行绘制。

我们起始于一个6个顶点，8个三角形的正八面体，对于面上每一个三角形，进行递归分解——依次连接三条边的中点形成小三角形。当分解的小三角形越，进行接下来扩散得到的球体效果也会越好。

{% asset_img 1.png %}
{% asset_img 2.png %}
{% asset_img 3.png %}
{% asset_img 4.png %}
{% asset_img 5.png %}

## References

[c++ - Drawing Sphere in OpenGL without using gluSphere()? - Stack Overflow](https://stackoverflow.com/a/7687312/8457016)