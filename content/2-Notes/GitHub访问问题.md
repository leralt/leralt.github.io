---
tags:
  - github
  - solution
---

GitHub 无法访问通常是由于 **DNS 污染、网络限制或本地配置问题** 导致的。以下是几种**有效且当前仍可行的解决方法**，你可以按顺序尝试：

---
### ✅ 方法一：修改本地 `hosts` 文件（推荐）
GitHub 的域名解析被污染时，手动指定 IP 地址是最直接有效的方式。
#### 步骤如下：
1. 打开 [https://raw.hellogithub.com/hosts](https://raw.hellogithub.com/hosts)  
    这个页面会自动提供最新的 GitHub 相关域名和 IP 映射。
2. 复制页面内容，粘贴到你的 `hosts` 文件中：
    - **Windows**：`C:\Windows\System32\drivers\etc\hosts`
    - **macOS / Linux**：`/etc/hosts`
3. 保存后，在终端或命令提示符中运行：
```bash
ipconfig /flushdns  # Windows
sudo dscacheutil -flushcache  # macOS
 ```
4. 重新访问 GitHub，通常即可正常打开。

> 推荐使用 [SwitchHosts](https://switchhosts.vercel.app/zh) 工具自动同步更新 GitHub 的 hosts 配置。

---

### ✅ 方法二：使用 GitHub 镜像站（适合只读需求）

如果你只是需要下载项目或查看代码，可以使用以下镜像站：
- [https://hub.fastgit.org](https://hub.fastgit.org/)
- [https://github.com.cnpmjs.org](https://github.com.cnpmjs.org/)
- [https://gitee.com](https://gitee.com/)（支持导入 GitHub 仓库）
> 注意：镜像站可能无法登录账户或提交代码
---

### ✅ 方法三：使用 VPN 或代理（适合长期开发者）

由于 GitHub 访问受限的根本原因是网络封锁，使用**稳定可靠的 VPN 或代理工具**是最彻底的解决方案。
- 推荐选择支持**全局代理**或**规则代理**的工具（如 Clash、V2Ray、Shadowrocket）。
- 配置后，确保 `github.com` 和 `raw.githubusercontent.com` 走代理。
---

### ✅ 方法四：修改 DNS（辅助手段）
将 DNS 改为以下公共 DNS，有助于绕过部分解析污染：
- Google DNS：`8.8.8.8` 和 `8.8.4.4`
- Cloudflare DNS：`1.1.1.1` 和 `1.0.0.1`
> 修改方法：网络设置 → 更改适配器选项 → IPv4 → 手动填写 DNS


**最终通过修改DNS方案成功解决！**
