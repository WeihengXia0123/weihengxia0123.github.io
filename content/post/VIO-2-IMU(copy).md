---
title: "VIO 2: IMU"
date: 2020-12-12T11:18:10+01:00
categories:
- VIO
tags:
- VIO
- IMU
keywords:
- VIO
- IMU
markup: mmark
#thumbnailImage: //example.com/image.jpg
---
$$
\dot{R}_{ib} = R_{ib}[w^b]^{\wedge} = R_{ib}[w^b]\times
$$
<!--more-->

{{<table_of_contents>}}

## Important Properties of Rotation Matrix and Vector

(1) The derivative of Rotation matrix, with regard to Rotation vector $$ hello $$   ():

$$
\begin{equation}
\begin{aligned}
\dot{R}_{ib} &= \frac{dR_{ib}}{dt}\\
&= \lim_{\triangle{t}\to 0}\frac{R_{ib}exp([w^b\triangle{t}]^{\wedge})-R_{ib}}{\triangle{t}}\\
&= \lim_{\triangle{t}\to 0}\frac{R_{ib}(I+[w^b\triangle{t}]^{\wedge})-R_{ib}}{\triangle{t}}\\
&= \lim_{\triangle{t}\to 0}\frac{R_{ib}[w^b\triangle{t}]^{\wedge}}{\triangle{t}}\\
&= R_{ib}[w^b]^{\wedge}\\
\end{aligned}
\end{equation}
$$


(2) The Skew-symmetric matrix ^ , is equal to a cross-product X:

$$
\begin{aligned}
\mathbf c=&a\times b\\
=&
\begin{vmatrix}
i & j & k\\
a_1 & a_2 & a_3 \\
b_1 & b_2 & b_3
\end{vmatrix}\\
=&
\begin{pmatrix}
a_2b_3-a_3b_2\\
-(a_1b_3-a_3b_1)\\
a_1b_2-a_2b_1
\end{pmatrix}\\
=&
\begin{pmatrix}
0 & -a_3 & a_2\\
a_3 & 0 & -a_1 \\
-a_2 & a_1 & 0
\end{pmatrix}
\begin{pmatrix}
b_1 \\ b_2 \\b_3
\end{pmatrix}
\end{aligned}
$$

$$
\begin{aligned}
(\mathbf a\times)&=
\begin{pmatrix}
0 & -a_3 & a_2\\
a_3 & 0 & -a_1 \\
-a_2 & a_1 & 0
\end{pmatrix}\\
&=(\mathbf a^{\wedge})
\end{aligned}
$$



(3) From the above properties (1) and (2), we can get 

$$
\dot{R}_{ib} = R_{ib}[w^b]^{\wedge} = R_{ib}[w^b]\times
$$
