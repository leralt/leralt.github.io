先跑下adcu和单跑车道线的CPU
试下张波的那个测试工具，在linux下用单图跑一下车道线，抓个火焰图分析下
## 查看是否有输出
```bash
./run_board_dds_monitor.sh echo -t L2Plus_perception_msgs::RoadGeometryFrame -n /perception/center_camer_fov120/RoadGeometryFrame -d 47
```
## 查看是否收到图
```bash
./run_board_dds_monitor.sh echo -t CameraEncodedFrame -n /perception/camera/encoded_center_camera_fov120 -d 47
```
## 查看定位信息
```bash
./run_board_dds_monitor.sh echo -t ad_localization_msgs::OdomStateInfo -n /localization/odomstate_info -d 47
```