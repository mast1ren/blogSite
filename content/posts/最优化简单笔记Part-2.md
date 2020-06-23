---
title: "最优化简单笔记Part 2"
date: 2020-06-23T15:54:28+08:00
draft: true
tags: ["数学","最优化导论","机器学习"]
categories: ["正经事"]
series: ["笔记","复习"]
description: "学习最优化路上的一些记录"
---

## 集合约束和无约束优化问题

> 例题1 求二元函数 $\mathcal f(x_1,x_2)=5x_1+8x_2+x_1x_2-x_1^2-2x_2^2$ 的一阶和二阶导数。
> $$
> Df(x)=[5+x_2-2x_1, 8+x_1-4x_2] \\
> \triangledown f(x)=Df(x)^\intercal \\
> D^2f(x) = \triangledown f(x)' = 
> \begin{bmatrix}
> 5+x_2-2x_1 \\
> 8+x_1-4x_2
> \end{bmatrix}
> '=
> \begin{bmatrix}
> -2 & 1\\
> 1 & -4
> \end{bmatrix}
> $$
> 可以看到的是，这个函数的二元函数的二阶导数是对梯度求导，而不是直接对一阶导数求导。

$\vec d$ 为 $n$ 元实值函数 $\mathcal f: \mathbb R^n \rarr \mathbb R$ 在 $\vec x \in \Omega$ 的一个可行方向，那么函数在这个点沿 $\vec d$ 方向的方向导数为：$\vec d^\intercal \triangledown \mathcal f(x)$

>$$
>\begin{aligned}
>\frac{\partial \mathcal f}{\partial \mathcal d}(x)&=\lim\limits_{a\rarr0}\frac{\mathcal f(x+ad)-\mathcal f(x)}{a}\\
>&=\frac{d}{da}\mathcal f(x+ad)|_{a=0} \\
>&=Df(x)d\\
>&=\triangledown\mathcal f(x)^\intercal d\\
>&=d^\intercal \triangledown \mathcal f(x)
>\end{aligned}
>$$

