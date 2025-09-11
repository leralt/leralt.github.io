## 在EP40上的编译新SDK
在新SDK现有基础上, 完成在TDA4上的编译,能够生成common, camera, aggregator二进制库
完善编译工具链, 使编译的环境变量等规范化
## conan更新适配
pilot更新添加了conan，改动很大，需要先熟悉下项目结构，重新做下适配
## 使用conan的编译方式
创建文件夹，在文件夹中运行saphub-upgrade.sh脚本：`./saphub-upgrade.sh -v 1.0.0`拉取编译cmake文件
目录结构目前是这样的
```txt
.saphub
├── CHANGELOG
├── cmake
│   ├── conan.cmake
│   ├── cuda.cmake
│   ├── CxxStandards.cmake
│   ├── get_source_info.cmake
│   ├── Linux-aarch64-SD5223.cmake
│   ├── Neutrino-aarch64-TDA.cmake
│   ├── package.cmake
│   └── utils.cmake
├── conan
│   ├── conan.conf
│   ├── profiles
│   │   └── default
│   └── settings.yml
├── conanfile.py
├── readme.md
├── sap.cmake
├── saphub
├── saphub-upgrade.sh
├── upload.sh
└── version.c.in
```
编译时在项目目录中创建build文件夹，执行命令(hozon项目)：
```bash
cmake -DCMAKE_TOOLCHAIN_FILE=../.saphub/cmake/Neutrino-aarch64-TDA.cmake -DWITH_ADCU=ON ..
```

## 修改接口适配SWC
思路：把C接口封装下，添加适配合众的preprocess接口，封装方式尽量参考原C++接口，不知道好不好实现。
