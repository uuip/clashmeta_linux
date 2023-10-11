# 加速Linux下的网络

## 令人折腾的各种问题
1. 大部分的代理服务商屏蔽了22端口，导致不能git pull.<br>
[解决方法](https://docs.github.com/zh/authentication/troubleshooting-ssh/using-ssh-over-the-https-port#enabling-ssh-connections-over-https)

2. docker pull 不走 "export https_proxy=..."; 需要改systemd的设置，改完重启docker服务会影响当前正在运行的服务 <br>
-- clash的`tun`模式利用虚拟网卡接管全局流量而不需要显式设置代理。

3. 使用systemd-resolved提供dns服务的Linux发行版（例如：Debian，Ubuntu），原版clash的auto-route不能inject dns请求，需要配置iptables。

## 完美方案
使用的[Clash.Meta](https://github.com/MetaCubeX/Clash.Meta) 能比较完美的解决上述各问题。
本仓库将各种配置都写好，只需添加代理服务的地址；不需要export https_proxy，且国内IP走直连。

1. `git clone https://ghproxy.com/https://github.com/uuip/clashmeta_linux`
2. 在config.yaml中 **proxy-providers**:url 添加clash代理的订阅地址
3. 以root权限运行 ./clash.meta-linux-amd64 -d .
4. 可以访问 http://ip:9090/ui 查看代理状态和切换代理，密码配置config.yaml: line 42-44。
