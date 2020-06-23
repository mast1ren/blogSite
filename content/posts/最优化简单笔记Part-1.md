---
title: "最优化简单笔记Part-1"
date: 2020-06-21T16:53:44+08:00
draft: false
tags: ["数学","最优化导论","机器学习"]
categories: ["正经事"]
series: ["笔记","复习"]
description: "学习最优化路上的一些记录"
---

## 一些吐槽

由于之前上最优化导论没有认真听，导致现在快考试了两眼一摸黑，在加上临近期末的各种事务的堆积，只能挤时间提前开始重新学习这门数学，成为时间管理带师。

## 需要了解的部分储备知识

### 有关向量的线性相关

对于给定的向量组 $A: \alpha_1, \alpha_2, ... \alpha_n$，如果如果存在一组不全为零的实数 $k_1, k_2, ...k_n$，使得 $\sum\limits_{i=1}^{n}k_i\alpha_i = 0$，则认为向量组 $A$ 线性相关，否则为线性无关。

可以这样判断，对于向量组 $A$，如果存在一个向量 $\alpha_i$，可以用 $A$ 中 $\alpha_i$ 以外的有限个向量表示出来，那么这个向量组为线性相关，否则为线性无关。

$$
if \quad \sum\limits_{i=1}^{n}k_i\alpha_i=0 \\
then \quad \alpha_i = -\frac{1}{k_i}\sum\limits_{j=1,j \ne i}^{n}k_j\alpha_j
$$

### 子空间

如果一个空间 $v$ 是 $\mathbb{R}^n$ 的子空间，那么这个空间对于向量的加法和数乘是封闭的。

如果有 $\alpha_1,\alpha_2,...\alpha_n \in v$，那么有 $span[\alpha_1, \alpha_2,...\alpha_n]=v$ 。

如果 $\forall \alpha_i \in v$，$\alpha_i$ 可用 $v$ 中的一组线性无关的向量表示出来，那么这组线性无关的向量为这个子空间的基，这个子空间的维数 $dim\ v$ = 基的向量个数。对于一个基，$\alpha_i \in v$ 可用这个基唯一地表示出来。

### 矩阵

对于一个矩阵，其秩为列向量或行向量的极大无关组中的向量个数。

对于一个矩阵 $A$，如果进行非零数乘、列交换或者加入可由其中的列向量表示出来的新列向量，矩阵的秩不变。

#### 行列式

如果一个行列式中有两列或者两行完全一致，那么这个行列式为 0。

如果一个矩阵的行列式为 0，那么这个矩阵被称为奇异矩阵。

对于一个线性方程组 $A\vec{x}=\vec{b}$，$A$ 被称为系数矩阵，$[A,\vec{b}]$ 为增广矩阵。

$$
a_{11} x_1+a_{12} x_2+\cdots+a_{1n} x_n=b_1  \\
a_{21} x_1+a_{22} x_2+\cdots+a_{2n} x_n=b_2 \\
\vdots \\
a_{m1} x_1+a_{m2} x_2+\cdots+a_{mn} x_n=b_m \\
A = [a_1,a_2,\cdots,a_n],\vec b = [b_1,b_2,\cdots,b_m]^T
$$

如果一个线性方程组的增广矩阵和系数矩阵的秩相等，那么这个方程组有解。

$$
\begin{aligned}
&\bf{证明：} \\
&如果 \quad A\vec x=\vec b \quad 有解 \\
&那么有 \quad \vec b = \sum\limits_{i=1}^n A_ix_i \\
&一定有\space rank(A)=rank(A,b) \\
&反之也可证明
\end{aligned}
$$


如果 $A^{n×n}$ 是非奇异矩阵，那么方程组有唯一解。如果 $rank(A) < n$，那么这个方程组有无穷多个解。

### 范数

对于向量 $\vec x, \vec y$，$ \|\textbf{x}+\textbf{y}\| \le \|\textbf{x}\| + \|\textbf{y}\|$
$$
\begin{align*}
\bf{证明：} \\
\|\textbf{x}+\textbf{y}\|^2 &= <\textbf{x}+\textbf{y},\textbf{x}+\textbf{y}> \\
		&= \|\textbf{x}\|^2 + \|\textbf{y}\|^2 + 2<\textbf{x},\textbf{y}> \\
		&\leq \|\textbf{x}\|^2 + \|\textbf{y}\|^2 + 2\|\textbf{x}\|\cdot\|\textbf{y}\| \\
		&= (\|\textbf{x}\| + \|\textbf{y}\|)^2 \\
\end{align*}
$$

向量的 P 范数：$\|\textbf{x}\|_p = (|\textbf{x}_1|^p+\cdots+|\textbf{x}_n|^p)^\frac{1}{p}$。当 $p\rarr \infin$ 时，$\|\textbf{x}\|_p = \max\limits_i\{|\textbf{x}_i|\}$

### 线性变换

如果在空间 $\mathbb{R}^n$ 和 $\mathbb{R}^m$ 中的一个函数 $\mathcal{L}: \mathbb{R}^n \rarr \mathbb{R}^m$ 

- 对于任意的 $\textbf{x} \in \mathbb{R}^n$ 和 $a \in \mathbb{R}$，有 $\mathcal{L}(a\textbf{x})=a\mathcal{L}(\textbf{x})$
- 对于任意 $\textbf{x}, \textbf{y} \in \mathbb{R}^n$，有 $\mathcal{L}(\textbf{x}+\textbf{y})=\mathcal{L}(\textbf{x})+\mathcal{L}(\textbf{y})$

那么这个函数可被称为一个线性变换。

$A=P^{-1}BP$，$B$ 是 $A$ 的相似矩阵。

线性变换可以用矩阵表示，相似矩阵表示相同的线性变换在不同的基底下的表示

### 正交投影

$\mathcal V$ 是 $\mathbb{R}^n$ 的子空间，那么 $\mathcal V$ 的正交补 $\mathcal V^\perp = \{\textbf{x}:\textbf{x}^T \mathcal v=0, \forall \mathcal v \in \mathcal V\}$，其也是 $\mathbb{R}^n$ 的子空间。对 $\mathbb{R}^n$ 上的任意向量 $\textbf{x}$，都有唯一的 $\textbf{x} = \textbf{x}_1+\textbf{x}_2$，其中 $\textbf{x}_1 \in \mathcal V, \textbf{x}_2 \in \mathcal V^\perp$

> 需要注意的是，这里的 $\mathbb R^n = \mathcal V \oplus \mathcal V^\perp$，即 $\mathbb R^n$ 由 $\mathcal V$ 和 $\mathcal V^\perp$ 张成，而不能理解为 $\mathbb R^n = \mathcal V \cup \mathcal V^\perp$

$∀x∈R^n$，都有 $Px ∈V$ 且 $x- Px ∈V^⊥$，线性变换 $P$ 称为正交投影算子。$A∈R^{m×n}$

像空间 $R(A) ≜\{Ax:x∈R^n\}$，零空间 $N(A)≜\{x∈R^n:Ax=0\}$，像空间和零空间都是子空间。

定理：矩阵 $P$ 是子空间 $\mathcal V = \mathcal R(P)$ 上的正交投影算子，当且仅当 $P^2 = P = P^\intercal$
$$
\begin{aligned}
&证明： \\
&必要性：\\
&由于 P 是 \mathcal R(P) 上的正交投影算子，Px \in \mathcal R(P)，x-Px \in \mathcal R(P)^\perp=\mathcal N(P^\intercal) \\
&\forall x, P^\intercal(I-P)x =0\\
&即\space \forall x, (P^\intercal-P^\intercal P)x=0，P^\intercal=P^\intercal P \\
&(P^\intercal)^\intercal=P^\intercal P=P=P^\intercal=P^2 \\
&充分性：\\
&\forall x,y, (Py)^\intercal(I-P)x=y^\intercal P^\intercal(I-P)x=y^\intercal(P^\intercal-P^\intercal P)x=0\\
&(I-P)x=0 \in \mathcal N(P^\intercal)=\mathcal R(P)^\perp\\
&证毕
\end{aligned}
$$

### 二次型函数

对于一个函数 $\mathcal f: \mathbb R^n \rarr \mathbb R, \mathcal f(x) = x^\intercal Qx$，$Q$ 为对称实矩阵，被称为二次型函数，如果 $\forall x, x^\intercal Qx > 0$，则二次型是正定的，$\ge0$ 为半正定，否则为负定，半负定或不定。

如果一个二次型是正定的，当且仅当 $Q$ 的每一个顺序主子式是正定的或 $Q$ 的每一个特征值是正数。

### 仿射函数

对于一个在 $\mathbb R^n \rarr \mathbb R^m$ 上的线性函数 $\mathcal L$，如果有 $x \in \mathbb R^n, y \in \mathbb R^m, \mathcal A(x) = \mathcal L(x) + y$，那么这个函数 $\mathcal A$称为仿射函数。如果一个函数 $\mathcal f: \Omega \rarr \mathbb R^m$，在 $x_0$ 处存在一个仿射函数能够模拟 $\mathcal f$，即
$$
\lim\limits_{x\rarr x_0} \frac{|\mathcal f(x) - \mathcal A(x)|}{|x-x_0|}=0\\
\mathcal A(x_0)= \mathcal f(x_0)=\mathcal L(x_0)+y \\
y=\mathcal f(x_0)-\mathcal L(x_0)\\
\mathcal A(x)=\mathcal L(x) + y =\mathcal L(x) + \mathcal f(x_0) - \mathcal L(x_0) = \mathcal L(x-x_0) + \mathcal f(x_0)
$$
那么说在 $x_0$ 上可微。