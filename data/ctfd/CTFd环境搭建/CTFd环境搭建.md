# 系统换源

#### /etc/apt/sources.list

```sh
#清华大学

deb http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free

deb-src https://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free

#阿里云

deb http://mirrors.aliyun.com/kali kali-rolling main non-free contrib

deb-src http://mirrors.aliyun.com/kali kali-rolling main non-free contrib

#中科大

deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib

deb-src http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib

#浙大

deb http://mirrors.zju.edu.cn/kali kali-rolling main contrib non-free

deb-src http://mirrors.zju.edu.cn/kali kali-rolling main contrib non-free

#东软大学

deb http://mirrors.neusoft.edu.cn/kali kali-rolling/main non-free contrib

deb-src http://mirrors.neusoft.edu.cn/kali kali-rolling/main non-free contrib
```

#### chmod 666 /etc/apt/sources.list

#### 运行 apt-get update && apt-get upgrade更新索引以生效

# Docker环境安装

#### apt install docker.io

![截图](img/29fcea0a24a516d4435f1a3959adfd51.png)

如果启动时发现如下错误请重新跟新源，重新下载

![截图](img/71bb4ee59d177ba6813a5bf67164473d.png)

## Docker-compose安装

```sh
curl -L https://get.daocloud.io/docker/compose/releases/download/1.29.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

![截图](img/f4630d587d7ecca902206c538bbd39a6.png)

## Docker开机启动

```sh
systemctl start docker
systemctl enable docker
```

## Docker加速

```sh
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://si9m86nl.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

# Docker集群设置

```sh
docker swarm init		# 初始化
docker node ls			# 查看节点ID
docker node update --label-add='name=linux-1' $(docker node ls -q) # 添加别名
```

![截图](img/4c1f432a76df419738908a2ea9e73132.png)

![截图](img/fbcce4183ea2258015f566823a2d32f5.png)

# 下载CTFd修改版

**博主 @Vicosna 已经对CTFd v3.3.1官方源码进行了更换国内镜像源、添加CTFd-Whale子模块、配置frp网络、设置静态文件CDN加速等工作。**

```sh
git clone -b frp https://github.com/vicosna/CTFd.git		# 修改版（根目录不建议修改名字）
cd CTFd														# 进入CTFd目录
git submodule update --init 								# 更新CTFd-Whale子模块

# ——————————————————————————————————————————————————————
# 如果你访问Github的速度不佳，也可以使用博主提供的CSDN和Gitee版（可选）
git clone -b https://codechina.csdn.net/vicosna/CTFd.git	# CSDN
cd CTFd														# 进入CTFd目录
sed -i 's/github.com/codechina.csdn.net/g' .gitmodules		# 修改子模块Url
git submodule update --init 								# 更新CTFd-Whale子模块
# ——————————————————————————————————————————————————————
git clone -b frp https://gitee.com/vicosna/CTFd.git			# Gitee
cd CTFd														# 进入CTFd目录
sed -i 's/github.com/gitee.com/g' .gitmodules				# 修改子模块Url
git submodule update --init 								# 更新CTFd-Whale子模块

```

![截图](img/1819893082e28fc47f3574af1ac2b89b.png)

![截图](img/c10564ac1b30d3617b03345fbb5a02fd.png)

# 构建镜像

```sh
docker-compose build
```

![截图](img/69768875f09f550fb41cf8c43f3c46b2.png)

# 部署容器

```sh
docker-compose up -d
```

![截图](img/494ee06aa840a98f572a783dfc48aaaa.png)

```sh
开docker ps -a
```

![截图](img/f2d49d36d3a2df44c48c3db2ca602745.png)

# 访问CTFd

![截图](img/202f47d9901d39955e268787fd6d850f.png)

<img src="img/3a93786cd7c9747a43efd1a7b7155964.png" alt="截图" style="zoom:50%;" />

# 汉化

**汉化包：https://github.com/Gu-f/CTFd_chinese_CN/tree/master/V3.4.1/CTFd-3.4.1/CTFd**

**将/root/Desktop/CTFd/CTFd/themes/下的admin，core 替换成汉化包里的 admin，core**

<img src="img/9a72711153676748300318d23f35d557.png" alt="截图" style="zoom:50%;" />

# 防火墙开放端口

## 安装ufw

```sh
apt-get install ufw
```

![截图](img/4c9e26ff8da199df1cf990a74a35ee6a.png)

## 开启27000到37000个端口

![截图](img/2d83b6e2eb5adace5d302d2de2a54ded.png)
