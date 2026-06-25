# Docker 搭建 Heimdall 专属导航页

## 一、介绍

Heimdall 是一种以简单的方式组织所有指向您最常用的网站和 Web 应用程序链接的方法。它甚至可以使用 Google、Bing 或 DuckDuckGo 包含一个搜索栏。

![Heimdall 界面](https://img-blog.csdnimg.cn/c5d8ccd97b384279976b73fac042a361.png)

## 二、安装环境

- 系统：CentOS Linux release 7.9.2009 (Core)
- Docker：Docker version 20.10.18, build b40c2f6

## 三、支持的架构

| 架构 | 可用 |
|------|------|
| x86-64 | ✅ |
| arm64 | ✅ |
| armhf | ✅ |

## 四、使用 Docker 部署 Heimdall

创建挂载目录：

```bash
mkdir -p /opt/docker/heimdall/config
```

### 方式一：docker-compose（推荐）

创建 `docker-compose.yml`：

```bash
vi /opt/docker/heimdall/docker-compose.yml
```

添加以下内容：

```yaml
version: "2.1"
services:
  heimdall:
    image: lscr.io/linuxserver/heimdall:latest
    container_name: heimdall
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Shanghai
    volumes:
      - /opt/docker/heimdall/config:/config
    ports:
      - 80:80
      - 443:443
    restart: unless-stopped
```

进入挂载目录并启动容器：

```bash
cd /opt/docker/heimdall
docker-compose up -d
```

### 方式二：docker cli

```bash
docker run -d \
  --name=heimdall \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Asia/Shanghai \
  -p 80:80 \
  -p 443:443 \
  -v /opt/docker/heimdall/config:/config \
  --restart unless-stopped \
  lscr.io/linuxserver/heimdall:latest
```

## 五、使用 Heimdall

### 1. 访问服务

```bash
http://192.168.50.101:80
```

![Heimdall 登录界面](https://img-blog.csdnimg.cn/f0706e99cb5d40f181c0db9f680384b2.png)

### 2. 设置语言

![设置语言](https://img-blog.csdnimg.cn/857b68b7b4bb4c7e9584855cec980f5c.png)

将语言设置为简体中文。

### 3. 添加应用

![添加应用](https://img-blog.csdnimg.cn/7d7d721b0ec64fd390eea8071698a99c.png)

可通过选择应用类型自动添加网站信息，也可通过输入网址自动获取网站信息。设置完成后，点击右下角保存即可。

![应用配置](https://img-blog.csdnimg.cn/d738307b63fd46bd9c9e0cd70b0603e0.png)

### 4. 最终效果

![Heimdall 仪表盘](https://img-blog.csdnimg.cn/0dd1153ec75641898625a91efa64f6d5.png)
