---
layout: post
title: 动态规划
categories: Algorithm
description: 希望自己能够善于总结归纳，温故而知新
keywords: 动态规划，dynamic programming
---

### 简介

动态规划方法通常用来求解**最优化问题**，这类问题可以有很多可行的解，每个解都有一个值，我们希望寻找具有最优值（最小值或最大值）的解。我们称这样的解为问题的一个最优解(an optional solution)，而不是最优解，因为可能有多个解都达到最优解。

> 两种常见求解动态规划的方法：

> **自顶向下**  －－带备忘的

> 1）按照自然的递归形式编写，但过程会保存每个子问题的解（通常保存在一个数据或散列表中）。

> 2）当需要一个子问题的解时，首先检查是否已经保存过此解，若是，则直接返回，否则，按通常方式求解这个子问题，并保存求解结果。

> **自底向上**  －－动态规划

> 1）需要定义子问题“规模”的概念，使得任何子问题的求解都只依赖于“更小的”子问题的求解。

> 2）因而，将子问题按规模排序，按由小到大的顺序求解。

> 3）当求解某个子问题时，它所依赖的那些更小的子问题都已求解完毕，结果已保存。

> 动态规划与分治方法联系与区别

> 1）两者都是通过组合子问题的解来求解原问题的解。

> 2）分治方法，将问题划分为**互不相交**的子问题，递归地求解子问题，然后将它们的解组合起来，求出原问题的解。

> 3）动态规划，应用于**子问题重叠**的情况，即不同的子问题具有公共的子问题（子问题的求解是递归进行的，将其划分为更小的子子问题）。此种情况，分治方法会反复求解公共子问题。


### 原理

> 学了动态规划，我们可能会问：什么情况下应该寻求动态规划方法求解问题呢？接下来介绍，适用于动态规划方法求解的最优化问题应该具备两个要素：**最优子结构**和**子问题重叠**；更深入的讨论自顶向下方法中如何借助备忘机制来充分利用子问题重叠特性。

#### 1 最优子结构

##### 1.1 最优子结构性质遵循的通用模式

> 1）第一步，进行选择，例如，钢条切割问题，需要选择第一次切割位置；矩阵链乘法问题，需要选择矩阵链的划分位置。

> 2）对于给定的一个问题，假定你已经知道哪种选择会产生最优解。你不需要关心（怎么确定这种）选择具体是如何得到的，只是假设已知这种选择。

> 3）给定一个选择，你需要确定这次选择会产生哪些子问题，及如何最好的描术子问题空间特征。

> 4）利用“剪切－粘贴”技术证明：每个子问题的解是构成原问题最优解的组成部分，它们就是其自身的最优解。

##### 1.2 针对不同问题，最优子结构的不同体现在两方面
> 1）原问题的最优解中涉及多少个子问题，例如，钢条切割问题，涉及一个子问题（长度为n-i的钢条的最优切割）；矩阵链乘法问题，涉及两个子问题（对于给定的矩阵链划分位置——矩阵Ak，需要求解两个子问题——AiAi+1...Ak和Ak+1Ak+2..Aj）。

> 2）在确定最优解使用哪些子问题时，需要考察多少种选择，例如，钢条切割问题，在确定最优解使用长度为n-i的钢条的最优切割这个子问题时，必须考察i的n种不同取值，来确定哪一个会产生最优解；矩阵链乘法问题，给定的矩阵链，矩阵Ak——划分位置，在确定AiAi+1...Ak和Ak+1Ak+2..Aj——两个子问题时，必须考察j-i种情况。

#### 2 重叠子问题

> 定义：如果递归算法反复求解相同的子问题，称最优化问题具有**重叠子问题**性质

> 依赖：动态规划问题的求解同时依赖于子问题的无关性和重叠性

#### 3 重构（重复构造）最优解

> 从实际考虑，通常将每个子问题所做的选择存在一个表中，这样就不必根据代价的值来重构这些信息。

> 带备忘的递归算法为每个子问题维护一个表项来保存它的解。每个表项的初值设为一个特殊值，表示尚未填入子问题的解。当递归调用中第一次遇到子问题时，计算其解，并存入对应表项。随后每次遇到同一个子问题，只简单查表，返回其解。
