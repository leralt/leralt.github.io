## Could not load library libsap_common.so
把libsap_common.so以及他的依赖库都拷到板子上
```
ldd:FATAL: Could not load library libsap_common.so
```
是因为编译的时候，默认加了个选项
## 依赖库
```
libsap_camera.so
libsap_common.so
libpcl_common.so
libSophs
SENSEAD.liccd 
```
## 修改项
prototxt中的预处理OP -> TiImageMultiPatchOP