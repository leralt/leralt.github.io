涉及多个topic，对比文件diff，之后修改
https://gerrit.senseauto.com/q/topic:%22rs_fusion_op%22+(status:open%20OR%20status:merged)
tlsr_tracking_op.cpp
bbox_fusion_resize_crop_op.hpp
bbox_fusion_resize_crop_op.cpp
[perception_camera_object.proto](https://gerrit.senseauto.com/c/sap_camera/+/198376/2/src/common/proto/perception_camera_object.proto)
[front_view_pipeline_center_camera_fov120_tlsr.prototxt](https://gerrit.senseauto.com/c/sap_camera_node/+/198375/2/resource/config/vdc/front_view_pipeline_center_camera_fov120_tlsr.prototxt) 主要是加参数

https://gerrit.senseauto.com/q/topic:%22rs_trk_turn_opt%22+(status:open%20OR%20status:merged)
[similarity_util.hpp](https://gerrit.senseauto.com/c/senseauto_modules_perception_common/+/197063/2/include/perception_common/data_association/similarity_util.hpp)
[bbox_motion.hpp](https://gerrit.senseauto.com/c/sap_camera/+/197064/2/src/common/plugin/common/similarities/bbox_motion.hpp)

https://gerrit.senseauto.com/c/sap_camera/+/193058
[bbox_fusion_resize_crop_op.cpp](https://gerrit.senseauto.com/c/sap_camera/+/193058/3/src/common/operator/bbox_fusion_resize_crop_op.cpp)
[tlsr_tracking_op.cpp](https://gerrit.senseauto.com/c/sap_camera/+/193058/3/src/road_semantics/tlsr_tracking_op.cpp)

https://gerrit.senseauto.com/q/topic:%22tlsr_tracking_opt%22+(status:open%20OR%20status:merged)
[perception_camera_object.proto](https://gerrit.senseauto.com/c/sap_camera/+/191823/1/src/common/proto/perception_camera_object.proto)
[tlsr_tracking_op.hpp](https://gerrit.senseauto.com/c/sap_camera/+/191823/1/src/road_semantics/tlsr_tracking_op.hpp)
tlsr_tracking_op.cpp
[front_view_pipeline_center_camera_fov120_tlsr.prototxt](https://gerrit.senseauto.com/c/sap_camera_node/+/191824/1/resource/config/vdc/front_view_pipeline_center_camera_fov120_tlsr.prototxt)

https://gerrit.senseauto.com/q/topic:%22unidet_v1.5.b%22+(status:open%20OR%20status:merged) -- 对齐tlsr_tracking即可，上面的依赖这个

https://gerrit.senseauto.com/q/topic:%22tlsr_miss_category%22+(status:open%20OR%20status:merged)
[adas_lkf_bbox_tracker.hpp](https://gerrit.senseauto.com/c/sap_camera/+/185661/1/src/common/plugin/object_tracking/adas_lkf_bbox_tracker.hpp)
[front_view_pipeline_center_camera_fov120_tlsr.prototxt](https://gerrit.senseauto.com/c/sap_camera_node/+/185765/1/resource/config/vdc/front_view_pipeline_center_camera_fov120_tlsr.prototxt)参数看下对齐

https://gerrit.senseauto.com/c/sap_camera/+/176427
[object_common.hpp](https://gerrit.senseauto.com/c/sap_camera/+/176427/11/src/common/base/data_type/object/object_common.hpp)
[obstacle_tracking_op.cpp](https://gerrit.senseauto.com/c/sap_camera/+/176427/11/src/gop/obstacle_tracking_op.cpp)

总结下：
直接同步的：
tlsr_tracking_op.hpp
tlsr_tracking_op.cpp
[similarity_util.hpp](https://gerrit.senseauto.com/c/senseauto_modules_perception_common/+/197063/2/include/perception_common/data_association/similarity_util.hpp)  
[bbox_motion.hpp](https://gerrit.senseauto.com/c/sap_camera/+/197064/2/src/common/plugin/common/similarities/bbox_motion.hpp)
[obstacle_tracking_op.cpp](https://gerrit.senseauto.com/c/sap_camera/+/176427/11/src/gop/obstacle_tracking_op.cpp)
[adas_lkf_bbox_tracker.hpp](https://gerrit.senseauto.com/c/sap_camera/+/185661/1/src/common/plugin/object_tracking/adas_lkf_bbox_tracker.hpp)

添加参数：
[front_view_pipeline_center_camera_fov120_tlsr.prototxt](https://gerrit.senseauto.com/c/sap_camera_node/+/198375/2/resource/config/vdc/front_view_pipeline_center_camera_fov120_tlsr.prototxt) 主要是加参数
[front_view_pipeline_center_camera_fov120_tlsr.prototxt](https://gerrit.senseauto.com/c/sap_camera_node/+/191824/1/resource/config/vdc/front_view_pipeline_center_camera_fov120_tlsr.prototxt)
[front_view_pipeline_center_camera_fov120_tlsr.prototxt](https://gerrit.senseauto.com/c/sap_camera_node/+/185765/1/resource/config/vdc/front_view_pipeline_center_camera_fov120_tlsr.prototxt)参数看下对齐
[perception_camera_object.proto](https://gerrit.senseauto.com/c/sap_camera/+/191823/1/src/common/proto/perception_camera_object.proto)
[object_common.hpp](https://gerrit.senseauto.com/c/sap_camera/+/176427/11/src/common/base/data_type/object/object_common.hpp)
[perception_camera_object.proto](https://gerrit.senseauto.com/c/sap_camera/+/198376/2/src/common/proto/perception_camera_object.proto)

[[Pasted image 20240321140941.png]]
[[yuv]]




|         模型         | 单次推理耗时 | 实车平均推理耗时 | 单次推理DDR读写量 |
| :----------------: | :----: | :------: | :--------: |
| backbone(pvb lane) | 7.8ms  |  19.3ms  |   55.7M    |
|      pvb_head      | 15.9ms |  19.5ms  |   72.1M    |
|     lane_head      | 3.6ms  |  7.9ms   |   20.2M    |
|      semantic      | 19.3ms |  25.3ms  |  _186.7M_  |
|      blockage      | 5.7ms  |  12.6ms  |   21.4M    |




|         模型         | 单次推理耗时 | 实车平均推理耗时 | 单次推理DDR读写量 |
| :----------------: | :----: | :------: | :--------: |
| backbone(pvb lane) | 7.8ms  |  19.3ms  |   55.7M    |
|      pvb_head      | 15.9ms |  19.5ms  |   72.1M    |
|     lane_head      | 3.6ms  |  7.9ms   |   20.2M    |
|      semantic      | 19.3ms |  25.3ms  |   186.7M   |
|      blockage      | 5.7ms  |  12.6ms  |   21.4M    |
