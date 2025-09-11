![[Pasted image 20250905190017.png]]
今日进展：42ms -> 32ms
https://gerrit.senseauto.com/c/sap_camera/+/300176
优化项：
- 改用float计算
- 不使用exp_val数组存储，改为原地计算
- 除法改为乘法


常规优化 单测：22ms 回灌：32ms
+qexp 单测：18ms 回灌 20ms