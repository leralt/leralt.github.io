回放板子卡死，固件替换后仍然卡死

### 查看mono结果
```
./run_board_dds_monitor.sh echo -t ObjectFrame -n /perception/pdcu_ObjectFrame -d 47
```
### tlsr
```
./run_board_dds_monitor.sh echo -t RoadSemanticsFrame -n /perception/center_camera_fov30/RoadSemanticsFrame -d 47
```
### facula
```
./run_board_dds_monitor.sh echo -t BlobFrame -n /perception/center_camera_fov30/BlobFrame -d 47
```
## 测试结果
使用路灯充足的回放数据测试，整体CPU在12%~15%
![xm](http://i.imgur.com/h7MyAV1.png)