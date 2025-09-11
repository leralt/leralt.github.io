[vh交接文档](https://ones.ainewera.com/wiki/#/team/Ttz6FJha/space/PCcjHZpX/page/13Gr6Syw)
```bash
LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/ota/liuchenlong/lib ./unittest_perception_camera_faw_ti_mono3d
```

```c++
void CreateDeviceInfo() override {
    // std::cout << "create " << vhTargetID << std::endl;
    if(device_info_contexts_.find(vhTargetID) == device_info_contexts_.end()){
        std::cout << "create " << vhTargetID << std::endl;
        auto tda4_context = std::make_shared<MindSporeA::Tda4DeviceInfo>();
        tda4_context->SetSocType(MindSporeA::SocType::J784S4);
        tda4_context->SetTargetId(MindSporeA::TargetID(vhTargetID));
        device_info_contexts_[vhTargetID] = tda4_context;
}
```

2个line在0 1个mono在1   第9次 出现shared_mem问题
3个0 1个1  tivxEventWait
10个0 在第六个tivxEventWait

一个line 跑29次出现内存问题 
一个mono 第20次出现shared_mem

daemon_cli stop&&daemon_cli start --mode had/offline --data_path /data/2023_04_10_15_48_03_AutoCollect_1024x576 --rsclbag perception_result.rsclbag.00000.rsclbag --topic /localization/odomstate_info --env_arguments rec_pause:=false LOG_level:=debug --board_ip vh:=10.151.187.197