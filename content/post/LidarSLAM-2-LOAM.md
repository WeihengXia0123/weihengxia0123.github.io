---
title: "LidarSLAM 1 ICP"
date: 2021-02-09T17:59:21+01:00
categories:
- Lidar
- SLAM
tags:
- Lidar
- SLAM
keywords:
- SLAM
thumbnailImage: //example.com/image.jpg
---

<!--more-->

### Ceres: Plane Analytical Cost Function

- Note 1: Eigen Library requires the data type to be consistent, eg. 

  ```c++
  // Since de is Vector3d, thus nu must be double
  Eigen::Vector3d de = (last_point_l-last_point_j).cross(last_point_m-last_point_j);
  double nu = (lp-last_point_j).dot(de);
  ```

- Note 2: Eigen Vector product must use explicitly `.dot()` pr `.cross()`