> Author: JunJianSyu <br />
> Slogan: Stay focused and keep shipping.

##  Shadowsocks

> If you want to keep a secret, you must also hide it from yourself.<br/>
> 如果你想保持一个秘密，你就必须隐藏自己。

## Download

* [clients](https://shadowsocks.org/en/download/clients.html) Download the latest application
* [Servers](https://shadowsocks.org/en/download/servers.html) VPS install source code


## 快速开始

Python Example:

```bash
// Debian/Ubuntu
apt-get install python-pip
pip install shadowsocks
// ContOS:
yum install python-setuptools && easy_install pip
pip install shadowsocks
```

```bash
// ContOS 遇到端口防火墙问题
yum install firewalld
// gui
yum install firewall-config
// 允许 tpc/udp:8080 开启 dmz级别
firewall-cmd --zone=dmz --add-port=8080/tcp
firewall-cmd --zone=dmz --add-port=8080/udp
// 启动服务 -> restart vps
systemctl enable firewalld        # 开机启动
```

用法:
创建`config`文件 `/etc/shadowsocks.json` Example:

*shadowsocks.json*
```bash
{
    "server":"my_server_ip",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```

前台运行:

```bash
ssserver -c /etc/shadowsocks.json
```

运行失败报错，原因有2种：端口被占用、源码依赖版本不对

后台运行:

```bash
ssserver -c /etc/shadowsocks.json -d start
ssserver -c /etc/shadowsocks.json -d stop
```

## 高级

> Optimize the shadowsocks server on Linux<br/>
> 优化linux下的 shadowsocks 服务

#### 第一步, 增加打开的文件描述符的最大数量
要处理数千个并发TCP连接，我们应该增加打开的文件描述符的限制。

修改 `limits.conf`

```bash
vi /etc/security/limits.conf
```

添加这2行到配置文件

```bash
* soft nofile 51200
* hard nofile 51200
```

在启动shadowsocks服务器之前，先设置ulimit

```bash
ulimit -n 51200
```

#### 第二步, 调整内核参数

shadowsocks调整参数的原因是:

1. 尽快重新使用端口和连接
2. 尽可能扩大队列和缓冲区
3. 为大延迟和高吞吐量选择TCP拥塞算法

下面是我们的生产服务器的`/etc/sysctl.conf`示例：

```bash
fs.file-max = 51200

net.core.rmem_max = 67108864
net.core.wmem_max = 67108864
net.core.netdev_max_backlog = 250000
net.core.somaxconn = 4096

net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_tw_recycle = 0
net.ipv4.tcp_fin_timeout = 30
net.ipv4.tcp_keepalive_time = 1200
net.ipv4.ip_local_port_range = 10000 65000
net.ipv4.tcp_max_syn_backlog = 8192
net.ipv4.tcp_max_tw_buckets = 5000
net.ipv4.tcp_fastopen = 3
net.ipv4.tcp_mem = 25600 51200 102400
net.ipv4.tcp_rmem = 4096 87380 67108864
net.ipv4.tcp_wmem = 4096 65536 67108864
net.ipv4.tcp_mtu_probing = 1
net.ipv4.tcp_congestion_control = hybla
```

记得执行`sysctl -p`重新加载配置

#### 附言 PPTP VPN搭建(linux + Debian)

1. 更新软件包 安装 pptpd
```bash
apt-get update
apt-get install -y pptpd
```

2. 编辑`/etc/pptpd.conf`,在文件最后添加
```bash
localip 192.168.92.1
remoteip 192.168.92.11-16
```
localip 是vpn服务器的地址
remoteip 是vpn客户端的地址, 可以配置成一个地址范围

3. 编辑`/etc/ppp/pptpd-options`
```bash
name pptpd
refuse-app
refuse-chap
refuse-mschap
require-mschap-v2
require-mppe-128
ms-dns 8.8.8.8
ms-dns 8.8.4.4
#nodefaultroute
#debug
#dump
proxyarp
lock
nobsdcomp
#nologfd
logfile /var/log/pptpd.log
```
文件其他的配置可通通注释掉

4. 编辑`/etc/ppp/chap-secrets`, 添加账户
```bash
username pptpd password *
```
用户名 vpn服务程序 密码 分配ip地址

5. 编辑`/etc/sysctl.conf`
```bash
net.ipv4.ip_forward = 1
```
去掉'#'取消注释, 执行命令使配置生效
```bash
sysctl -p
```

6. 编辑`/etc/rc.local`, 系统启动时添加服务
```bash
iptables -t nat -A POSTROUTING -s 192.168.92.0/24 -j SNAT --to-source "你的服务器的公开IP"
```
添加可执行权限
```bash
chmod +x /etc/rc.local
```

7. 重启服务
```bash
/etc/init.d/pptpd restart
```
