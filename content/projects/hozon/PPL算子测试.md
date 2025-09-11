## Patch 8

没有dsp failed -1

## 实车测试CPU优化情况
- latest包抓取CPU使用率、trace
- 换上adcu、pdcu固件，kestrel_nart.kep继续抓取

## 测试结果

## 7月6日，帮忙配置调试环境
考虑到单测较为简单且同样能复现问题，决定配置单测环境
- [x] 写一个mono3d的fov30的单测
- [x] 配置libs、prototxt、models、images
- [ ] 测试能否复现问题

## 7月11日 回放测试一下
算了，上车测吧

nart版本：dev-ti-int8-output-dequant-20230707-v1
两个版本：log版和无log版
- log版 ：CI 460