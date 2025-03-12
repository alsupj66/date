# 修复 Racknerd KVM VPS 80端口无法访问问题的解决方案

最近黑五促销，我在 [Racknerd](https://bit.ly/Rack_Nerd) 上购买了一台年付仅 55.93 美元的 KVM VPS，但在部署完 Nginx 后，发现外网无法正常访问。经过排查，发现问题出在防火墙设置，下面记录下具体的解决步骤，希望对大家有所帮助。

## Racknerd VPS 配置详情
Racknerd 这台 VPS 配置如下：

- **4 个 vCPU 核心**
- **130 GB 纯 SSD 存储**
- **5 GB 内存**
- **每月 12,000 GB 传输量**
- **1 Gbps 网络端口**
- **完全根管理员访问权限**
- **1 个专用 IPv4 地址**
- **提供 KVM / SolusVM 控制面板**
- **免费 Clientexec 许可证**
- **可选多个位置**
- **年付费用：55.93 美元**

👉 [【建议收藏】2025年Racknerd最新优惠套餐整理汇总 - 每日更新可用活动优惠](https://bit.ly/Rack_Nerd)

## 解决方案步骤
问题的主要原因是默认防火墙策略没有打开需要的端口，以下是解决方案：

### 1. 检查 80 和 443 端口是否开启
运行以下命令检查 80 端口：

bash
$ firewall-cmd --query-port=80/tcp


输出结果：`no`，说明端口未开启。

### 2. 开启 80 和 443 端口
使用以下命令将 80 和 443 端口添加到防火墙规则：

bash
$ firewall-cmd --permanent --add-port=80/tcp
output: success

$ firewall-cmd --permanent --add-port=443/tcp
output: success


### 3. 重载防火墙配置
修改防火墙设置后，需要重载它以生效：

bash
$ firewall-cmd --reload
output: success


至此，外网访问问题解决。

## 常用防火墙命令清单

以下是一些常见的防火墙操作命令，方便大家日后进行其他端口管理或问题排查：

### 查看防火墙服务状态
bash
systemctl status firewalld


### 查看防火墙状态
bash
$ firewall-cmd --state


### 列出防火墙规则
bash
$ firewall-cmd --list-all


### 检查指定端口是否开放
bash
$ firewall-cmd --query-port=19999/tcp


### 开放指定端口
bash
$ firewall-cmd --permanent --add-port=80/tcp


### 关闭指定端口
bash
$ firewall-cmd --permanent --remove-port=80/tcp


### 重载防火墙（必要操作）
bash
$ firewall-cmd --reload


通过以上命令，可以轻松管理服务器防火墙，确保服务正常运行。