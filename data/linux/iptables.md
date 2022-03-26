# Iptables防火墙的配置
## NAT综合案例
![NAT综合案例拓扑图](./img/NAT%E6%8B%93%E6%89%91%E5%9B%BE.png)
#### 准备三台虚拟机，分别配置IP（如上图）
``` text
注意：Intranet网络连接使用Vmware1(仅主机)

      Firewall网卡1使用Vmware1(仅主机)，网卡2使用Vmware8(NAT)

      Extranet网络连接使用Vmware8(NAT)

不懂请自行百度，谢谢！！
```
#### 测试三台机器的连通性
##### Intranet：
```sh
[root@intranet ~]# ping 192.168.10.20
PING 192.168.10.20 (192.168.10.20) 56(84) bytes of data.
64 bytes from 192.168.10.20: icmp_seq=1 ttl=64 time=16.0 ms
64 bytes from 192.168.10.20: icmp_seq=2 ttl=64 time=0.402 ms
^C
--- 192.168.10.20 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 0.402/8.204/16.007/7.803 ms
```
```sh
[root@intranet ~]# ping 202.112.113.112
PING 202.112.113.112 (202.112.113.112) 56(84) bytes of data.
64 bytes from 202.112.113.112: icmp_seq=1 ttl=64 time=1.97 ms
64 bytes from 202.112.113.112: icmp_seq=2 ttl=64 time=0.337 ms
^C
--- 202.112.113.112 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 0.337/1.156/1.976/0.820 ms
```
```sh
[root@intranet ~]# ping 202.112.113.113
PING 202.112.113.113 (202.112.113.113) 56(84) bytes of data.
^C
--- 202.112.113.113 ping statistics ---
8 packets transmitted, 0 received, 100% packet loss, time 7001ms
```
##### Firewall:
```sh
[root@firewall ~]# ping 192.168.10.1
PING 192.168.10.1 (192.168.10.1) 56(84) bytes of data.
64 bytes from 192.168.10.1: icmp_seq=1 ttl=64 time=0.337 ms
64 bytes from 192.168.10.1: icmp_seq=2 ttl=64 time=0.550 ms
^C
--- 192.168.10.1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1000ms
rtt min/avg/max/mdev = 0.337/0.443/0.550/0.108 ms
```
```sh
[root@firewall ~]# ping 202.112.113.113
PING 202.112.113.113 (202.112.113.113) 56(84) bytes of data.
64 bytes from 202.112.113.113: icmp_seq=1 ttl=64 time=0.511 ms
64 bytes from 202.112.113.113: icmp_seq=2 ttl=64 time=0.393 ms
^C
--- 202.112.113.113 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1000ms
rtt min/avg/max/mdev = 0.393/0.452/0.511/0.059 ms
```
##### Extranet:
```sh
[root@extranet ~]# ping 202.112.113.112
PING 202.112.113.112 (202.112.113.112) 56(84) bytes of data.
64 bytes from 202.112.113.112: icmp_seq=1 ttl=64 time=0.213 ms
64 bytes from 202.112.113.112: icmp_seq=2 ttl=64 time=0.397 ms
^C
--- 202.112.113.112 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 0.213/0.305/0.397/0.092 ms
```
```sh
[root@extranet ~]# ping 192.168.10.20
PING 192.168.10.20 (192.168.10.20) 56(84) bytes of data.
^C
--- 192.168.10.20 ping statistics ---
3 packets transmitted, 0 received, 100% packet loss, time 2000ms
```
```sh
[root@extranet ~]# ping 192.168.10.1
PING 192.168.10.1 (192.168.10.1) 56(84) bytes of data.
^C
--- 192.168.10.1 ping statistics ---
3 packets transmitted, 0 received, 100% packet loss, time 2000ms
```
#### 配置yum源
```sh
[root@firewall ipv4]# mount /dev/cdrom /media       #挂载镜像
mount: /dev/sr0 写保护，将以只读方式挂载

vim /etc/yum.repos.d/rhel.repo

[dvd]
name=redhat
baseurl=file:///media
enabled=1
gpgcheck=1

保存退出之后（:wq）
输入：yum clean all
```
##### 可能出现的问题
1. 您已启用软件包 GPG 签名检查，这样很好。不过您尚未安装任何 GPG 公钥......
> rpm --import /media/RPM-GPG-KEY-redhat-release
2. failure: repodata/repomd.xml from dvd: [Errno 256] No more mirrors to try.
file:///medi/repodata/repomd.xml: [Errno 14] curl#37 - "Couldn't open file /medi/repodata/repomd.xml"
> 检查一下yum源的配置文件，baseurl路径是否有写错误
3. 显示依赖不全
> 从头挂载，重新配置yum源

#### 安装iptables服务
在 **Firewall** 机器上安装
```sh
yum install iptables iptables-services -y
```

#### 开启路由存储转发，其值为1
**工作原理**
> **内网向公网发送数据包时**，由于目的主机跟源主机 **不在同一网段** ，所以数据包暂时发往 **内网默认网关** 处理，而本网段的主机对此数据包不做任何回应。
由于**源主机IP是私有的**，禁止在公网使用，所以必须将数据包的**源发送地址**改成**公网上的可用IP**，网关收到数据包后就要**转换IP**。然后网关再把数据包发往目的主机。

> **目的主机收到数据包之后**，只认为这是网关发送的请求，并不知道内网主机的存在，也没必要知道，**目的主机处理完请求**，把回应信息发还给网关。
**网关收到后**，将目的主机发还的数据包的 **目的ip地址** 改为发出请求的 **内网主机的ip地址**，并将其发给内网主机。这就是数据包的**路由转发**。内网的主机只要查看数据包的目的ip与发送请求的源主机ip地址相同，就会回应，这就完成了一次请求。

> 出于安全考虑，**Linux系统默认是禁止数据包转发的。** 所谓转发即当主机拥有**一块以上的网卡时**，其中一块收到数据包，根据数据包的目的ip地址将包发往本机另一网卡，该网卡根据路由表继续发送数据包。这通常就是路由器所要实现的功能。

**设置**
开启IP转发功能：
临时开启：
```sh
echo 1 > /proc/sys/net/ipv4/ip_forward
```
永久开启：
```sh
1. vim /etc/sysctl.conf
2. net.ipv4.ip_forward = 1     (在这个/etc/sysctl.conf文件里插入；1表示开启，0表示关闭)
3. sysctl -p
```
### SNAT
改变源地址，实现局域网使用同一个公网IP接入Internel，好处如下：
1. 保护内网用户安全，因为公网地址总有一些人会恶意扫描，而内网地址在公网没有路由，所以无法被扫描，能被扫描的只有防火墙这一台，这样就减少了被攻击的可能。
2. IPV4地址匮乏，很多公司只有一个IPV4地址，但是却有几百个用户需要上网，这个时候就需要使用SNAT。
3. 省钱，公网地址付费，使用SNAT只需要一个公网IP就可以满足几百人同时上网
[想要了解更多，请点击>>](http://t.zoukankan.com/yuhaohao-p-14096431.html)

#### 在Firewall上配置SNAT
```sh
# 1. iptables 和 firewalld 不能同时开启
systemctl stop firewalld
systemctl start iptables

# 2. 查看旧iptables的规则，并清空规则
iptables -L
iptables -F

# 3. 添加SNAT，修改源目的地址的内网IP (注意大小写)
iptables -t nat -A POSTROUTING -s 192.168.10.0/24 -j SNAT --to-source 202.112.113.112

# 4. 查看是否添加成功
iptables -t nat -L

## 如果添加成功会有这一条记录（仔细找找）
POSTROUTING_ZONES  all  --  anywhere             anywhere            
SNAT       all  --  192.168.10.0/24      anywhere             to:202.112.113.112

## 5. 如果上面的操作都没有问题，则现在内网的机器（Intranet）可以ping通外网（Extranet）。如果不行，请重复上述操作
```

#### 在Extranet上提供WEB服务
**安装httpd**
```sh
yum install httpd -y
```
> [安装不成功请点击查看yum源配置](#配置yum源)

```sh
# 1. 防火墙允许开启http服务
firewall-cmd --permanent --add-service=http

# 2. 重启防火墙
firewall-cmd --reload

# 3. 重启http服务
systemctl restart httpd

# 4. 查看80端口是否开放
netstat -an | grep :80

显示如下，表示80端口开放，（回车发现什么也没有，说明没有开放80端口）：
[root@extranet ~]# netstat -an | grep :80
tcp6       0      0 :::80                   :::*                    LISTEN
## 通常重启httpd服务之后就会开启80端口

# 5. 测试，在本机查看WEB服务是否正常运行，能打开页面并看到页面内容说明成功，
#   如果出现 无法连接、页面载入出错，重试 等字样，说明失败。
#   请重新安装httpd服务，并重新操作上述步骤

可以自己在 /var/www/html 里面编写一些网页

firefox 127.0.0.1
```

**在Extranet上配置好WEB服务之后**
可以在Intranet的浏览器上输入Extranet的IP地址就可以访问到页面，效果如下
![](./img/%E5%A4%96%E7%BD%91web.png)


### DNAT
改变目标地址，外网可以访问到内网服务器
1. 配置双网卡，一网卡对内，一网卡对外；一般是高访问量的web服务器，为了避免占用网关的流量才这样做，使用不是很广泛。
2. 内网web服务器，或是ftp服务器，为了用户在公网也可以访问，有不想买公网ip地址，采用DNAT方案。
[想要了解更多，请点击>>](http://t.zoukankan.com/yuhaohao-p-14096431.html)

#### 在Intranet上配置WEB、防火墙
```sh
# 1. 安装httpd服务
yum install httpd -y

# 2. 启动httpd服务
systemctl start httpd

# 3. 每一次开机都自动开启httpd服务
systemctl enable httpd

# 4. 安装iptables
yum install iptables iptables-services -y

systemctl stop firewalld
systemctl start iptables
systemctl enable iptables

# 5. 查看是否启动成功
systemctl status iptables

# 6. 清空旧规则
iptables -F

# 7. 开放80端口
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

**在Firewall上配置DNAT**
将外网请求的80端口数据包，修改目的地址为内网的Intranet
```sh
iptables -t nat -A PREROUTING -d 202.112.113.112 -p tcp --dport 80 -
j DNAT --to-destination 192.168.10.1:80
```

**此时，在外网的机器就可以访问内网的网页了，如下图：**
![](./img/%E5%86%85%E7%BD%91web.png)

##### 如果内网想拒绝其他的服务（FTP，SSH，samba等等），就输入：
```sh
iptables -A INPUT-j REJECT
```
iptables还有很多的命令，到时候再弄一个专题来研究

本期的Iptables综合案例就到此结束，上面的问题如果有啥问题请联系作者[^wechar]，谢谢观看！
[^wechar]:JChan_L