# RackNerd便宜美国VPS服务器安装Google BBR详细教程

RackNerd是一家成立于2019年的美国服务器供应商，以提供高性价比的VPS服务著称，其产品覆盖了虚拟服务器、独立服务器、大硬盘服务器及站群服务器等。其VPS方案价格极具竞争力，部分套餐价格低至几美元一年，支持支付宝、微信、PayPal等多种支付方式。如果正在寻找一款性价比高的VPS，RackNerd无疑是一个值得考虑的选择。为了最大化利用这些廉价VPS，我们可以通过安装Google BBR来提升其网络性能，下面是详细的安装教程。

👉 [【建议收藏】2025年RackNerd最新优惠套餐整理汇总 - 每日更新可用活动优惠](https://bit.ly/Rack_Nerd)

---

## RackNerd VPS推荐配置

RackNerd采用KVM虚拟化架构，配备AMD Ryzen处理器和NVMe高速硬盘，支持高性能运作。其数据中心位于纽约、芝加哥以及最新上线的西雅图，可以根据需求选择合适的机房。

---

## 如何安装Google BBR加速

以下是安装Google BBR的具体步骤，这将帮助您优化VPS的网络性能。

### 步骤一：升级Linux内核
运行以下命令以升级至4.9或更高版本Linux内核：

bash
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
yum --enablerepo=elrepo-kernel install kernel-ml -y


### 步骤二：设置默认内核
完成内核安装后，默认内核可能不是最新版本，因此需要手动修改。运行以下命令查看系统中已安装的所有内核：

bash
cat /boot/grub2/grub.cfg | grep menuentry


然后设置默认内核，例如：

bash
grub2-set-default 'CentOS Linux 7 Rescue f162c5663d6044ba8d784979acd61b44 (5.4.2-1.el7.elrepo.x86_64)'


注意：根据个人需求更换为自己安装的内核版本。

### 步骤三：重启系统
输入“reboot”命令重启VPS。

### 步骤四：确认新内核是否生效
通过以下命令检查当前系统内核版本：

bash
uname -r


显示类似于`5.4.2-1.el7.elrepo.x86_64`的版本信息，则表示升级成功。

### 步骤五：安装BBR
内核升级完成后，执行以下命令以启用Google BBR：

bash
echo 'net.core.default_qdisc=fq' | sudo tee -a /etc/sysctl.conf
echo 'net.ipv4.tcp_congestion_control=bbr' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p


### 步骤六：验证BBR是否成功开启
运行以下命令检查BBR状态：

bash
sudo sysctl net.ipv4.tcp_available_congestion_control
sudo sysctl -n net.ipv4.tcp_congestion_control
lsmod | grep bbr


如果结果显示包含“bbr”，就表示安装成功！

---

## 系统内核版本升级方法

如果当前内核版本低于4.9，则需要升级内核。以下为具体步骤：

1. 使用`root`账户登录远程VPS，运行以下命令以下载并执行内核升级脚本：

bash
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh


2. 执行脚本后，会显示当前系统内核信息并自动检测，内核版本可能显示为3.10.0系列。

3. 按回车键确认，待安装完成后系统内核会被升级为最新版本，如5.6.14。

4. 重启系统：

bash
reboot


5. 登录系统后，通过以下命令再次确认内核版本：

bash
uname -r


若内核版本成功变更为升级后的版本（如5.6.14），即表示操作成功。

---

## Google BBR加速效果测试

在BBR启用后，可以通过如下命令简单测试性能提升：

bash
sudo dd if=/dev/zero of=500mb.zip bs=1024k count=500


测试完成后，您会发现访问速度尤其是在观看高清视频等任务时有明显的提升。

---

## 总结

Google BBR是一种创新的拥塞控制算法，可以显著提升Linux服务器的吞吐量并降低延迟。特别是在国外VPS中，BBR可以有效改善网络性能。如果您希望轻松获得BBR加速，也可以直接选择支持自带BBR模板的优质VPS服务。

RackNerd等提供高配置低价格VPS的服务商，是优化网络体验的优质选项之一，赶快试试吧！