---
title: "VIO 6 Front-End"
date: 2021-01-17T00:49:33+01:00
categories:
- VIO
tags:
- VIO
- IMU
keywords:
- VIO
- IMU
# markup: mmark
thumbnailImage: https://res.cloudinary.com/dr5l034cf/image/upload/c_scale,w_504/v1610983482/VIO/VIO-6-front-end_gakgfa.png
---

<!--more-->
![](https://res.cloudinary.com/dr5l034cf/image/upload/v1610983986/VIO/VIO-6-Homework_tzwi80.png )

一. 基础题
1. 证明题 (Proof of y=u4)
![证明](https://res.cloudinary.com/dr5l034cf/image/upload/v1610985699/VIO/VIO%E4%BD%9C%E4%B8%9A6_1_dg0ob0.jpg)

2. Triangulation (Code)
```c++
   /// TODO::homework; 请完成三角化估计深度的代码
   // 遍历所有的观测数据，并三角化
   Eigen::Vector3d P_est;           // 结果保存到这个变量
   P_est.setZero();
   /* your code begin */
   int num_row = 2*(end_frame_id - start_frame_id);
 
   Eigen::MatrixXd D(Eigen::MatrixXd::Zero(num_row, 4));
   Eigen::Matrix<double, 3, 4> Pk;
   for (int k = start_frame_id; k < end_frame_id; k++){
       // Pk.setZero();
       Pk.block<3,3>(0,0) = camera_pose[k].Rwc.transpose();
       Pk.block<3,1>(0,3) = - camera_pose[k].Rwc.transpose() * camera_pose[k].twc;
 
       Eigen::Matrix<double, 1, 4> Pk1 = Pk.block<1,4>(0,0);
       Eigen::Matrix<double, 1, 4> Pk2 = Pk.block<1,4>(1,0);
       Eigen::Matrix<double, 1, 4> Pk3 = Pk.block<1,4>(2,0);
 
       D.block<1,4>((k-start_frame_id)*2, 0) = camera_pose[k].uv.x()*Pk3 - Pk1;
       D.block<1,4>((k-start_frame_id)*2+1, 0) = camera_pose[k].uv.y()*Pk3 - Pk2;
   }
 
   Eigen::JacobiSVD<Eigen::MatrixXd> svd(D.transpose() * D, Eigen::ComputeThinU | Eigen::ComputeThinV);
  
   Eigen::MatrixXd U = svd.matrixU();
   Eigen::MatrixXd V = svd.matrixV();
   Eigen::MatrixXd A = svd.singularValues();
 
   std::cout <<"U: \n"<< U <<std::endl;
   std::cout <<"V: \n"<< V <<std::endl;
   std::cout <<"A: \n"<< A <<std::endl;
 
   // 归一化为 [x,y,z,1]
   P_est = V.block<3,1>(0,3) / V(3,3);
   /* your code end */

```

二. 提升题\
3. Add different normal noises to camera measurements, and observe the change of the ratio between sigma4 and sigma3.\
关键代码如下:
- Generate norm_noise variances
```c++
  std::vector<float> noise_variances;
   std::vector<float> sigma_3;
   std::vector<float> sigma_4;
   float variance = 0.0;
   for (int k = 0; k < 20; k++){
       noise_variances.push_back(variance);
       variance += 0.001;
   }
```
- Add noise to observations
```c++
   for(int k=0; k<20; k++){
       std::normal_distribution<double> noise_gaussian(0., noise_variances[k]); // Create normal distribution as noises
       for (int i = start_frame_id; i < end_frame_id; ++i) {
           Eigen::Matrix3d Rcw = camera_pose[i].Rwc.transpose();
           Eigen::Vector3d Pc = Rcw * (Pw - camera_pose[i].twc);
 
           double x = Pc.x();
           double y = Pc.y();
           double z = Pc.z();
 
           x += noise_gaussian(generator);
           y += noise_gaussian(generator);
           z += noise_gaussian(generator);
 
           camera_pose[i].uv = Eigen::Vector2d(x/z,y/z);
       }

```
- Save sigma 3/4
```c++
    sigma_3.push_back(A(2,0));
    sigma_4.push_back(A(3,0));
```
- Plot the ratio
![](https://res.cloudinary.com/dr5l034cf/image/upload/v1610987831/VIO/noise_figure_gvalv4.png)

4. Plot the ratio of sigma4/sigma3, but this time fix the noise, while increasing the number of frames observed.
{{< image
src="https://res.cloudinary.com/dr5l034cf/image/upload/v1610987830/VIO/frame_figure_yostcr.png"
alt="frame figure plot" >}}

坑点总结：\
[1] u4是一个1x4的向量\
[2] y分别是[x,y,z,1],而u4没有归一化,所以y还有在u4做一个归一化才能得到。\
[3] D*y = 0\

Reference: \
[1] [SLAM中部分小知识点的记录](https://blog.csdn.net/remanented/article/details/103452070)\
[2] [MIT线性代数Ax=b总结](https://zhuanlan.zhihu.com/p/44114447)
