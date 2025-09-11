### 文件说明
- dequant、bev_pool、transpose：代码文件夹
- vx_app_c7x_target_kernel_\*.lib： C7x lib库
- custom.json：AOS裁剪脚本用

### 裁剪
裁剪工程文件路径以 /home/faw/ti-processor-sdk-rtos-j784s4-evm-08_06_00_14/ 为例

1. 将DSP_Kernels文件夹放入裁剪工程文件夹内
2. dequant、bev_pool、transpose文件夹放置于/home/faw/ti-processor-sdk-rtos-j784s4-evm-08_06_00_14/vision_apps/kernels
3. ==transpose/c71/concerto.mak文件中的 IDIRS  += /home/faw/ti-processor-sdk-rtos-j784s4-evm-08_06/mmalib_02_06_02_00/ti/mmalib/src 修改为实际对应路径==
4. 修改/home/faw/ti-processor-sdk-rtos-j784s4-evm-08_06_00_14/vision_apps/Makefile，在DIRECTORIES := 声明下方添加 
	DIRECTORIES += kernels/dequant
	DIRECTORIES += kernels/bev_pool
	DIRECTORIES += kernels/transpose
5. 检查custom.json中 libName 和 includeName 是否与放置路径对应
6. 参考HUAWEI AOS 3.2 AI框架参考手册 3.3.2裁剪工具部分，使用--customop 添加自定义算子

运行裁剪脚本时添加如下选项
```bash
--customop ./DSP_dequant/custom.json
```


