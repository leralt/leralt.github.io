```bash
LD_LIBRARY_PATH=/usr/local/lib/:./lib:./ ./unittest_perception_camera_faw_ti_mono3d
```

### 替换固件

固件下载链接：http://10.198.15.254/senseauto_packages/senseauto-hozon-bsp/2023-05-25/ADCU_feature_ppl-test-c66_418/upg-r5f-tda4adcu-hozon-20230525155818.zip

在板端执行df -h |grep appact 查看 /hirain/appact/路径对应的分区

1.  分区是/dev/emmc0t178.7 时 mount -t ext2 /dev/emmc0t131.1 /r5
2.  分区是/dev/emmc0t178.6 时 mount -t ext2 /dev/emmc0t131 /r5

之后将upg-r5f-tda4adcu-hozon-20230525155818.zip中的j7-c66开头的固件拷贝至板端/r5/lib/firmware目录下

### 替换kestrel_nart.kep

将kestrel_nart.kep拷贝至板端/asw/act/lib下

之后按照标准流程测试CPU占用