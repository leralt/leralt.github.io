## 环境搭建
```
docker run -it --name replay_faw_had  --net=host -v /data:/data  -d registry.sensetime.com/senseauto-ci-image/senseauto/developer:v1.1.2 /bin/bash
```
上面的命令不能有`/bin/bash`，奇怪
### 配置aws
### 安装环境
```bash
sudo senseauto-pkm install --module ...
```
`...`代表jenkins上FawHad分支的had-x86的所有包

## 跨机通信配置
需配置PC端和板端的rscl.pb.txt
参考https://ones.ainewera.com/wiki/#/team/Ttz6FJha/space/PCcjHZpX/page/Cz6RVc8x
目前端口配置
```
data_bind_address: "tcp://10.151.187.186:8979"
control_bind_address: "tcp://10.151.187.186:8978"
```

## PC端docker内
### 可视化server：
防止冲突，可在`/opt/senseauto_active/senseauto-visualizer/etc`路径下，修改`visualizer_config.json`中的`server_port`
启动server
```bash
visualizer_server   
```
### zmq桥接server
```bash
cd /opt/senseauto_active/senseauto-framework-sdk/bin
```

```bash
./zmq_forward_bridge_server tcp://0.0.0.0:8766 tcp://0.0.0.0:8768
```
### 播放回放数据
目前可用数据包仅有`/data/2023_04_10_15_48_03_AutoCollect_1024x576`
```bash
daemon_cli stop&&daemon_cli start --mode had/offline --data_path /data/2023_04_10_15_48_03_AutoCollect_1024x576 --rsclbag perception_result.rsclbag.00000.rsclbag --topic /localization/odomstate_info --env_arguments rec_pause:=false
```

## 板端配置

