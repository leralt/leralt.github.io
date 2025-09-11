## 6.4进展
安装TI给的demo格式组织sum demo，编译通过但运行存在错误
![[Pasted image 20240605195127.png]]
## 6.5进展 

将demo拆成多个目录，解决编译问题与target ID报错问题（示例为C66，需改为C71），跑通测试程序
![[Pasted image 20240605194557.png]]
*warning为几个scalar未release*

## 6.12进展
使用 host_emulation 在 linux 上完成反量化demo


## 6.18
![[Pasted image 20240618165334.png]]
kernal参数类型对齐，注意host代码是静态库，编到了可执行文件中，需替换.out文件

- vxVerifyGraph卡住不报错（重启板子）
- vxWaitGraph卡住

用remote log看具体卡在哪个地方，难道是close？
解决：
__vstore_pred(vpStore, __SA0ADV(float16, dst_desc_target_ptr), vOut);
这里的dst_desc_target_ptr之前前面有个&，去掉就好了

- 输入tensor不对，原因是vxMapTensorPatch是把指定位置的指针的值给出来，是vxCreateTensor时分配的内存，需要对这块内存做赋值，这样的话就只能拷贝了
改用tivxReferenceExportHandle和tivxReferenceImportHandle来修改vx_tensor的内部指针值



遗留问题
- 4个DSP核适配



SE转置选项会导致卡住