# 加速Linux Server下的网络

## 令人折腾的各种问题
1. 大部分的代理服务商屏蔽了22端口，导致不能git pull.<br>
[解决方法](https://docs.github.com/zh/authentication/troubleshooting-ssh/using-ssh-over-the-https-port#enabling-ssh-connections-over-https)

2. docker pull 不走 "export https_proxy=...";
* 改systemd的设置，改完重启docker服务会影响当前正在运行的服务；不是很好的解决方案。 <br>
* 原版clash的`tun`模式利用虚拟网卡接管全局流量而不需要显式设置代理，除了配置麻烦，见下述3。

3. 使用systemd-resolved提供dns服务的Linux发行版（例如：Debian，Ubuntu Server），原版clash的auto-route不能inject dns请求，需要配置iptables。

## 本仓库
使用[mihomo](https://github.com/MetaCubeX/mihomo) (原ClashMeta)完美解决上述问题，使开发人员不折腾。<br>
* 将各种配置（分流，web管理）都写好
* 不需要export http(s)_proxy
* **只需添加代理服务的订阅地址**

### 使用步骤
1. `git clone --depth 1 https://mirror.ghproxy.com/https://github.com/uuip/clashmeta_linux`
2. 在config.yaml中 **proxy-providers**:url 添加clash代理的订阅地址
3. 以root权限运行 ./mihomo-linux-amd64 -d .
4. [可选] 访问 http://ip:9090/ui 查看代理状态和切换代理，密码配置config.yaml: line 42-44。

### 已知问题
- 使用tun模式启动时，外网无法访问docker提供的服务；停止后回复正常。
