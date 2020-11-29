---
title: "Robotics 1 Kinematics and Dynamics"
date: 2020-11-29T15:10:18+01:00
categories:
- robotics
tags:
- robotics
keywords:
- robotics
thumbnailImage: https://res.cloudinary.com/dr5l034cf/image/upload/c_scale,w_300/v1606676704/Robotics/PUMA_bd0nms.png
---

<!--more-->
{{<table_of_contents>}}
## What is Kinematics vs Dynamics vs Machanics?
This is the first question when I heard of these 3 terms. They all frequently appear in Robotics books and sometimes appear together.\
\
 Let's first understand their definitions from Webster:
- Kinematics: branch of dynamics that deals with aspects of
motion **apart from considerations of mass and force**
- Dynamics: branch of mechanics that **deals with forces** and
their **relation primarily to the motion** but sometimes also
to the equilibrium of bodies
- Mechanics: branch of physical science that deals with
**energy and forces and their effect on bodies**

## Forward vs Inverse Kinematics
- Forward: Where is the end-effector based on a given configuration?
- Inverse: Where do the joints have to be to place the end-effector?

## End Effector Pose (operational space)
![EE](https://res.cloudinary.com/dr5l034cf/image/upload/c_scale,w_104/v1606677467/Robotics/EE_pose_y0hjns.png)
X is composed of position and direction

## Rotation Matrix
Here's a derivation of the rotation matrix:
![Rotation Matrix](https://res.cloudinary.com/dr5l034cf/image/upload/c_scale,w_500/v1606672132/Robotics/rotation_Matrix_oure92.png)

## Translation Matrix
t = (t1,t2,t3).transpose()

## Homogenerous Transformations (HTs)
![Homogeneous Transformation](https://res.cloudinary.com/dr5l034cf/image/upload/c_scale,w_358/v1606673838/Robotics/homogeneous_transformation_junzs4.png)

## DH parameters
What is DH parameters?
![](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d8/DHParameter.png/519px-DHParameter.png)
DH param -> Transformation
![](https://res.cloudinary.com/dr5l034cf/image/upload/v1606688863/Robotics/DH_jo2a8a.jpg)

## 3 Uses of HTs
1. Frame Description
![HT_des_frame](https://res.cloudinary.com/dr5l034cf/image/upload/c_scale,w_500/v1606675298/Robotics/HT_des_frame_io6gse.png)
2. Mapping between frames
![HT_map](https://res.cloudinary.com/dr5l034cf/image/upload/c_scale,w_500/v1606675298/Robotics/HT_map_between_frames_m8g736.png)
3. Transform a point
![HT_point](https://res.cloudinary.com/dr5l034cf/image/upload/c_scale,w_500/v1606675394/Robotics/HT_transform_point_vsp2y2.png)
## HT Computation
![HT_compute](https://res.cloudinary.com/dr5l034cf/image/upload/c_scale,w_358/v1606675522/Robotics/HT_compute_rwamn0.png)

## Conclusion
I guess some of you might be wondering: **so, why do we need Kinematics?**

Actualy, the reasons are covered above already:
- Forward: Where is the end-effector based on a given configuration?
- Inverse: Where do the joints have to be to place the end-effector?


