---
title: "VIO笔记一：李群 李代数"
date: 2020-12-05T23:28:25+01:00
categories:
- VIO
tags:
- VIO
- Lie Group
- Lie Algebra
keywords:
- VIO
- Lie Group
- Lie Algebra

#thumbnailImage: //example.com/image.jpg
---

<!--more-->

引入李群和李代数，主要是因为Rotation matrix的性质 只能想乘，不能相加，相加了就不是旋转矩阵了。而要把它作为普通矩阵来处理优化，就必须对它加以约束。

所以，咱们引入李群 (R, ·)，中间这个 · 是一个运算符，咱们rotation matrix的李群就是刚才说的，乘号。 然后咱们可以把李群(矩阵)用指数映射到李代数(向量)上，而这个李代数由于是向量 支持+号 所以可以咱们终于可以求导啦！

然后求导的方法有两种，用BCH近似的直接求导和扰动求导。扰动求导用得最多，就是本科学求极限用泰勒展开的方式，很容易计算就得到了位置坐标关于R的导数，这样方便优化一开始这章提到的那个大J，loss function。

而这个大J越小，说明误差越小 观察越精确。用啥方法呢，高中数学学过的，求导找曲线的极点，这个点一般就是局部最小的最优点。

Reference:\
[1] [半闲居士 - 视觉SLAM中的数学基础 第三篇 李群与李代数](https://www.cnblogs.com/gaoxiang12/p/5137454.html)\
[2] [大师兄！SLAM 为什么需要李群与李代数？](https://mp.weixin.qq.com/s/sVjy9kr-8qc9W9VN78JoDQ)\
[3] [7天搞定视觉SLAM 第三天——李群和李代数](https://www.icxbk.com/article/detail/1312.html)\
