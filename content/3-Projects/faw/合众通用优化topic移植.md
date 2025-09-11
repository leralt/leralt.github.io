[合众项目优化项](https://ones.ainewera.com/wiki/#/team/Ttz6FJha/space/7wDsQ8pi/page/EeiPeXFP/)
[合众通用优化topic移植](https://ones.ainewera.com/wiki/#/team/Ttz6FJha/space/CWSU4Yry/page/VtK9HNb4)
CI: 1325
CI有代码质量问题，正在修复
已合入
## perception common

1. perception_common仓库: 优化定位信息缓存队列, 原本使用map缓存定位数据, 改为循环队列.

1. 优化依据: 定位数据更新频率100hz, 如果使用map, 将会有100hz的插入和删除操作, 又因为键值是时间戳, 因此插入会带来频繁的红黑树调整, 并且伴随定位结构体的申请释放.
2. topic: [http://gerrit.senseauto.com/c/senseauto_modules_perception_common/+/118626](http://gerrit.senseauto.com/c/senseauto_modules_perception_common/+/118626)
3. bug修复(循环队列可能下标越界): [http://gerrit.senseauto.com/c/senseauto_modules_perception_common/+/144276](http://gerrit.senseauto.com/c/senseauto_modules_perception_common/+/144276)

3. perception_common仓库: 优化查询车辆状态相关接口, 缓存最新查询结果, 减少查询结果维护队列size.

1. 优化依据: 融合或者相机感知里对于车辆状态的查询最常见的情况就是同一帧的多个目标可能查询多次, 因此时间戳是相同的, 同时刻的车辆状态是确定的, 因为查询接口可以缓存最新一帧的查询结果, 在下次查询如果时间匹配直接返回. 这个优化对于融合峰值有非常明显的降低.
2. topic: [http://gerrit.senseauto.com/c/senseauto_modules_perception_common/+/158134](http://gerrit.senseauto.com/c/senseauto_modules_perception_common/+/158134)

5. perception_camera仓库:

1. 优化项1: 后处理/birdview op中vector变量使用static(或者改成类成员变量)修饰, 减少每次计算时变量结构体的申请/内存扩展.
2. 优化项2: 优化后处理op的topk实现, 原本实现是先将网络数据的scores全部解析到vector中然后排序取topk, 优化后使用小顶堆原地做topk, 即边读取输出tensor边做topk, 减少往vector中存的耗时和排序的耗时.
3. 优化项3: general_predictor op删除模型输出tensor的拷贝.
4. 优化项4: 前处理结果使用0拷贝, 结果直接给到模型推理(但需要确保前处理和推理的串行关系, 避免前处理数据被覆盖)
5. 优化依据: 对于每个调用周期都要使用的vector结构, 应该避免使用临时变量, 尽量在使用前先申请容量, 避免扩展内存损耗; 对于有内存拷贝的地方, 尽量避免.
6. topic: [http://gerrit.senseauto.com/c/senseauto_modules_perception_camera/+/115218](http://gerrit.senseauto.com/c/senseauto_modules_perception_camera/+/115218)

7. perception_camera仓库(仅限tda4): 前处理通过直接给openvx tensor结构的结果给推理, 从而避免前处理到推理的拷贝; 图片到前处理使用ti提供的swap接口, 避免图片到前处理的拷贝.

1. 优化依据: tda4在做模型推理时需要提前将输入进行pad, 相当于做了一次拷贝. ti提供了从vx_tensor直接创建网络推理context的接口, 有dsp做pad从而避免有cpu做而带来的拷贝.
2. topic: [http://gerrit.senseauto.com/c/senseauto_modules_perception_camera/+/116260](http://gerrit.senseauto.com/c/senseauto_modules_perception_camera/+/116260)

9. perception_camera仓库: 优化后nms计算, 框相似度不使用多边形iou, 使用框中心点距离

1. 优化依据: 多边形iou计算比较复杂, 简化成中心点距离, 计算量小
2. topic: [http://gerrit.senseauto.com/c/senseauto_modules_perception_camera/+/120540](http://gerrit.senseauto.com/c/senseauto_modules_perception_camera/+/120540)

11. perception_cmaera仓库: 优化遮挡op, 直接使用nv12原图替代bgr图; 使用循环队列代替vector缓存历史结果

1. 优化依据: 遮挡需要使用灰度图, 如果使用前处理后的bgr图, 虽然size较小, 但是多了一个bgr转灰度的过程,相当于一次拷贝, 使用nv12原图,虽然较大, 但可以避免拷贝; vector不适用于先入先出的数据缓存.
2. topic: [http://gerrit.senseauto.com/c/senseauto_modules_perception_camera/+/152325](http://gerrit.senseauto.com/c/senseauto_modules_perception_camera/+/152325)
3. topic: [http://gerrit.senseauto.com/c/senseauto_modules_perception_camera/+/152393](http://gerrit.senseauto.com/c/senseauto_modules_perception_camera/+/152393)

13. perception_cmaera仓库:

1. 优化项1: birdview op里一些临时变量的创建, 代码写法优化
2. 优化项2: bbox_tracker.cpp中, 每次创建tracker都需要创建一个卡尔曼滤波器, 创建滤波器时需要创建和计算一些初始化矩阵, 缓存这些矩阵, 避免每次创建; tracker中的一些临时矩阵使用类成员变量, 避免反复创建; 删除无用的缓存队列.
3. 优化依据: 每个相机的tracker参数在一次运行过程中都是固定的, 因此初始化参数也是确定的, 因此可以缓存下来复用;
4. topic: [http://gerrit.senseauto.com/c/senseauto_modules_perception_camera/+/155907](http://gerrit.senseauto.com/c/senseauto_modules_perception_camera/+/155907)

15. perception_camera仓库: 删除PerceptionObjectFrame结构, 由通信结构体直接转SDK内部结构体, 避免多次转换

1. topic: [http://gerrit.senseauto.com/c/senseauto_modules_perception_camera/+/158655](http://gerrit.senseauto.com/c/senseauto_modules_perception_camera/+/158655)

17. perception_camera仓库: taskflow init阶段创建, 而不是每次init时创建

1. topic: [http://gerrit.senseauto.com/c/senseauto_modules_perception_camera/+/118157](http://gerrit.senseauto.com/c/senseauto_modules_perception_camera/+/118157) 其中存在一个bug, failed_tasks每次调用没用清除

19. perception_aggregator仓库: 删除PerceptionObjectFrame结构, 由通信结构体直接转SDK内部结构体, 避免多次转换

1. topic: [http://gerrit.senseauto.com/c/senseauto_modules_perception_aggregator/+/158348](http://gerrit.senseauto.com/c/senseauto_modules_perception_aggregator/+/158348)

21. 推理库反量化优化, 量化模型的直接输出都是int8, 推理库一般会反量化到fp32, 可以优化到加速硬件上做, 减少cpu使用
22. swc层的优化:

1. 进程合并, 在同一个进程中跑多个相机的目标检测和车道线. 合并进程可以减少大量的cpu占用. [http://gerrit.senseauto.com/c/senseauto_assd_hozon_perception/+/121879](http://gerrit.senseauto.com/c/senseauto_assd_hozon_perception/+/121879)
2. 目标合并, 合并多个相机的目标后发送, 减少通信频次 [http://gerrit.senseauto.com/c/senseauto_assd_hozon_perception/+/132899](http://gerrit.senseauto.com/c/senseauto_assd_hozon_perception/+/132899)
3. 目标转换优化, 各种临时变量的创建等 [http://gerrit.senseauto.com/c/senseauto_assd_hozon_perception/+/158610](http://gerrit.senseauto.com/c/senseauto_assd_hozon_perception/+/158610)
4. 融合swc的process策略, 避免数据拥挤处理而产生峰值, 比如连续两帧radar触发process(radar 20hz, process 10hz, 最好是跳帧用) [http://gerrit.senseauto.com/c/senseauto_assd_hozon_perception/+/158610](http://gerrit.senseauto.com/c/senseauto_assd_hozon_perception/+/158610)
5. 设置线程优先级, 针对系统调度出现异常的情况下, 某些线程会有异常占用, 比如出现大量的sched_yield/timer_timeout调用等.[http://gerrit.senseauto.com/c/senseauto_assd_hozon_perception/+/128672](http://gerrit.senseauto.com/c/senseauto_assd_hozon_perception/+/128672)