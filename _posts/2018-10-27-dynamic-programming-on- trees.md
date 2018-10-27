---
layout: post
title:  "Dynamic Programming On Trees"
date:   2018-10-27 10:28:00 +0800
featured-img: dp-tree
categories: dynamic programming
summary: This article explained the dynamic programming on trees with some examples.
---

下列两篇文章中对树形dp进行了一定的讲解：

1. [Dynamic Programming on Trees \| Set-1](https://www.geeksforgeeks.org/dynamic-programming-trees-set-1/)
2. [Dynamic Programming on Trees \| Set 2](https://www.geeksforgeeks.org/dynamic-programming-trees-set-2/)

![](/assets/img/posts/dp_on_trees/Snipaste_2018-10-27_15-21-24.png)

![](/assets/img/posts/dp_on_trees/Snipaste_2018-10-27_15-23-02.png)

接下来，我们通过几个例子来了解树形dp

1. [没有上司的舞会](http://acm.hdu.edu.cn/showproblem.php?pid=1520)

   ![](/assets/img/posts/dp_on_trees/Snipaste_2018-10-27_15-36-47.png)

   ![](/assets/img/posts/dp_on_trees/Snipaste_2018-10-27_15-46-34.png)

2. 树的重心

   ![](/assets/img/posts/dp_on_trees/Snipaste_2018-10-27_15-54-59.png)

   ![](/assets/img/posts/dp_on_trees/Snipaste_2018-10-27_18-37-23.png)

   **注意：这里`dfs`中的两个参数 `u`、`pre`，`pre`是`u`的父节点，但初始时传入的参数是个例外，`0`不是这棵树上的节点，所以`1`没有父节点。下一个例子中的情况也是这样。**

3. 树上最远的距离

   ![](/assets/img/posts/dp_on_trees/Snipaste_2018-10-27_18-45-03.png)

   ![](/assets/img/posts/dp_on_trees/Snipaste_2018-10-27_18-54-56.png)

   ![](/assets/img/posts/dp_on_trees/Snipaste_2018-10-27_19-10-44.png)
