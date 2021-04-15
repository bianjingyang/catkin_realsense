
## 查看相机的模型及坐标系关系
* 启动如下launch文件即可
`roslaunch realsense2_camera rs_d435_camera_with_model.launch`
* 查看两个坐标系的关系
` rosrun tf tf_echo camera_color_optical_frame base_link `
* 输出 Translation: [0.032, 0.012, -0.011]

` rosrun tf tf_echo camera_color_optical_frame camera_depth_optical_frame `
* 输出 - Translation: [0.015, 0.000, -0.000]

## 按照相机的几何尺寸计算RGB光学坐标系与两个螺纹安装孔的坐标系关系
X轴32.5mm

Y轴偏移0

z'=4.2mm
Z轴偏移25.05-4.2 = 20.84mm


## 订阅三维点云文件
* 获得三维点云rgbd信息，启动如下launch文件
` roslaunch realsense2_camera rs_rgbd.launch `
* 然后订阅 /camera/depth_registered/points话题的信息，拿来后处理就行。


## rgbd深度图的坐标系是那个，与相机坐标系的关系。
` rostopic echo /camera/depth_registered/points | grep frame_id `
* 对齐之后的rgbd深度图的坐标系是以rgb的坐标系"camera_color_optical_frame"为基准的
* 



## rviz中红色为X轴，绿色为Y轴，蓝色为Z轴。


##使用plc_tool查看pcd文件
`   sudo apt install pcl-tools   `
`   pcl_viewer xxx.pcd    `

### 录制数据
`rosbag record -a `

## 播放录制的数据
` rosbag play -r 1 realsense.bag ` # (这里在你录制的包的路径下运行)  1倍速回放


## 滤波的总体方案
* 直通滤波==>统计学滤波(去处离群点)==>体素滤波（下采样）
* 了解使得点云边界光滑有哪些滤波方法
* 可以考虑使用空间裁剪滤波器(CropHull滤波器)，获得点云在3D封闭曲面上内部的点;主要需要解决如何识别边界坐标的问题。
*__如果crophull算法可用，则可以用此方法替代简单的直通滤波__ 
* 滤波后的结果，提供给三维重建模块
