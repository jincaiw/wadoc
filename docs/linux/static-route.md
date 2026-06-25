# CentOS 添加静态路由

在日常使用中，服务器有两个 IP 地址、两块网卡的配置，访问不同网段，这种情况很常见。需要创建额外的路由条目，以确定通过正确的网关转发数据包。

以下在 CentOS 7、8 测试通过。

## 一、使用 route 命令加入临时路由（重启后失效）

route 命令参数：

```
add     增加路由
del     删除路由
-net    设置到某个网段的路由
-host   设置到某台主机的路由
gw      出口网关 IP 地址
dev     出口网关物理设备名
```

```bash
# 加入到主机的路由
route add -host 192.168.1.123 dev eth0
route add -host 192.168.1.123 gw 192.168.1.1

# 加入到网络的路由
route add -net 192.168.1.123 netmask 255.255.255.0 eth0
route add -net 192.168.1.123 netmask 255.255.255.0 gw 192.168.1.1
route add -net 192.168.1.123 netmask 255.255.255.0 gw 192.168.1.1 eth1
route add -net 192.168.1.0/24 eth1

# 加入默认网关
route add default gw 192.168.1.1

# 删除路由
route del -host 192.168.1.11 dev eth0
route del -net 192.168.1.123 netmask 255.255.255.0

# 查看路由信息
ip route
route -n
```

## 二、添加永久路由的方法

### 1. 默认网关写入 ifcfg 文件（推荐）

```bash
vi /etc/sysconfig/network-scripts/ifcfg-eth0
```

在配置 IP 地址时直接将 GATEWAY 写入 ifcfg 文件：

```
GATEWAY=gw-ip
```

或者写入 `/etc/sysconfig/network`：

```
GATEWAY=gw-ip
```

### 2. 写入 `/etc/rc.local`（不推荐）

注意：CentOS 7 必须执行 `chmod +x /etc/rc.d/rc.local` 来确保脚本在引导时执行。

```bash
vi /etc/rc.local
```

添加：

```bash
route add -net 192.168.3.0/24 dev eth0
route add -net 192.168.2.0/24 gw 192.168.3.254
route add -net 172.16.0.0 netmask 255.255.0.0 gw 192.168.1.100 dev eth0
```

> 不推荐此方法：若重启网络服务，路由会失效；某些网络服务在 rc.local 执行前已启动，可能导致链路不通。

### 3. 写入 `/etc/sysconfig/static-routes` 文件

需手动创建此文件：

```bash
vi /etc/sysconfig/static-routes
```

添加：

```
any net 192.168.1.0/24 gw 192.168.1.1
any net 192.168.2.0 netmask 255.255.255.0 gw 192.168.2.1
any host 10.19.190.11/32 gw 10.19.177.10
any host 10.19.190.12 gw 10.19.177.10
```

> 该方式在 CentOS 8 默认安装时无效。CentOS 8 默认使用 nmcli 管理网络，可通过 `yum install network-scripts` 安装传统 network.service 来恢复。

### 4. 创建 `route-eth0` 文件（推荐）

```bash
vi /etc/sysconfig/network-scripts/route-eth0
```

添加如下格式的内容：

```
192.168.1.0/24 via 192.168.0.1
```

重启网络验证：

```bash
systemctl restart network
```
