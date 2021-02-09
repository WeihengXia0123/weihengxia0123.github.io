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

### Trajectory Result

![](https://res.cloudinary.com/dr5l034cf/image/upload/c_scale,w_407/v1612891598/SensorFusion/ICP_SVD_2_qnjtal.png)

​																		Figure: PCL::ICP result

![](https://res.cloudinary.com/dr5l034cf/image/upload//c_scale,w_407/v1612891602/SensorFusion/myICP_SVD_2_iw3ylv.png)





### Trick of ICP (Very important!)

From what we saw in the previous result, the ICP-based method produces a very bad trajectory.

When running the program, I saw one warning:
```bash
"Warning: Unnormalized quaternion. blah blah"
```

After getting hints from my friend @崔峰,   I realize this could be the source of the bad result in the previous  implementaton. Thus, I used a "naive" approach to normalize q:

```c++
// normalize Quaternion
Eigen::Matrix3f Rot = result_pose.block<3,3>(0,0);
Eigen::Quaternionf q(Rot);
result_pose.block<3,3>(0,0) = q.normalized().toRotationMatrix();
```

After this change, the ICP-approach trajectory improved dramatically:

![](https://res.cloudinary.com/dr5l034cf/image/upload/v1612891605/SensorFusion/myICP_SVD_norm_1_zindks.png)



![](https://res.cloudinary.com/dr5l034cf/image/upload/c_scale,w_407/v1612891605/SensorFusion/myICP_SVD_norm_evo_rpe_2_vw1gvt.png)