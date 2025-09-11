doImgPreprocessThread 图像预处理线程
doCameraPerceptionWithPreprocThread 感知处理线程
topic : `NV_frame_sync`

---
`doRecvImgFdThread`在不断的`fatchFd` 

> 推理OP后，哪里通知的前处理线程

`pre_hdl`中的成员变量`process_bset_` 用于判断**一阶段推理**是否完成
到BevUniconvViewTransformOP::ProcessInner时，代表前处理已经完成，会将该pipeline的`process_bset_`全部置为`false`，并通知doImgPreprocessThread进行下次图像预处理，预处理结束则重新置为`true`

所以目前是每次一阶段推理完成就开始进行图像预处理，图像预处理之后到下次一阶段推理中间还有一段时间，这段时间如果有新的图像到来，也不会被用来替换旧的数据，这是一个问题
- 用当前时间跟图像时间戳进行比较，如果差距比较大，则继续`wait`，但是如何定义**比较大**，图像fetch间隔并不固定

每次fetch是五路相机一块，时间戳是相同的

```
right_front_camera: 1678017411959311000                                                                                                                                               
left_front_camera: 1678017411959311000                                                                                                                                                
right_rear_camera: 1678017411959311000                                                                                                                                                
left_rear_camera: 1678017411959311000                                                                                                                                                 
center_camera_fov120_lane: 1678017411959311000                                                                                                                                        
center_camera_fov120_mono: 1678017411959311000                                                                                                                                        
right_front_camera: 1678017412025597000                                                                                                                                               
left_front_camera: 1678017412025597000                                                                                                                                                
right_rear_camera: 1678017412025597000                                                                                                                                                
left_rear_camera: 1678017412025597000                                                                                                                                                 
center_camera_fov120_lane: 1678017412025597000                                                                                                                                        
center_camera_fov120_mono: 1678017412025597000                                                                                                                                        
right_front_camera: 1678017412092317000                                                                                                                                               
left_front_camera: 1678017412092317000                                                                                                                                                
right_rear_camera: 1678017412092317000                                                                                                                                                
left_rear_camera: 1678017412092317000                                                                                                                                                 
center_camera_fov120_lane: 1678017412092317000                                                                                                                                        
center_camera_fov120_mono: 1678017412092317000                                                                                                                                        
right_front_camera: 1678017412125914000                                                                                                                                               
left_front_camera: 1678017412125914000                                                                                                                                                
right_rear_camera: 1678017412125914000                                                                                                                                                
left_rear_camera: 1678017412125914000                                                                                                                                                 
center_camera_fov120_lane: 1678017412125914000                                                                                                                                        
center_camera_fov120_mono: 1678017412125914000                                                                                                                                        
right_front_camera: 1678017412192693000                                                                                                                                               
left_front_camera: 1678017412192693000                                                                                                                                                
right_rear_camera: 1678017412192693000                                                                                                                                                
left_rear_camera: 1678017412192693000                                                                                                                                                 
center_camera_fov120_lane: 1678017412192693000                                                                                                                                        
center_camera_fov120_mono: 1678017412192693000                                                                                                                                        
right_front_camera: 1678017412259473000                                                                                                                                               
left_front_camera: 1678017412259473000                                                                                                                                                
right_rear_camera: 1678017412259473000
left_rear_camera: 1678017412259473000
center_camera_fov120_lane: 1678017412259473000
center_camera_fov120_mono: 1678017412259473000
```