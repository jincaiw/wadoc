# Docker 搭建 phpIPAM 的 IP 地址管理系统

### 1. 新建 Docker 打包文件

```bash
vim /opt/docker/phpIPAM/docker-compose.yml
```

### 2. 编写 docker-compose.yml

```yaml
# WARNING: Replace the example passwords with secure secrets.
# WARNING: 'my_secret_phpipam_pass' and 'my_secret_mysql_root_pass'

version: '3'

services:
  phpipam-web:
    image: phpipam/phpipam-www:latest
    ports:
      - "80:80"
    environment:
      - TZ=Europe/London
      - IPAM_DATABASE_HOST=phpipam-mariadb
      - IPAM_DATABASE_PASS=my_secret_phpipam_pass
      - IPAM_DATABASE_WEBHOST=%
    restart: unless-stopped
    volumes:
      - phpipam-logo:/phpipam/css/images/logo
      - phpipam-ca:/usr/local/share/ca-certificates:ro
    depends_on:
      - phpipam-mariadb

  phpipam-cron:
    image: phpipam/phpipam-cron:latest
    environment:
      - TZ=Europe/London
      - IPAM_DATABASE_HOST=phpipam-mariadb
      - IPAM_DATABASE_PASS=my_secret_phpipam_pass
      - SCAN_INTERVAL=1h
    restart: unless-stopped
    volumes:
      - phpipam-ca:/usr/local/share/ca-certificates:ro
    depends_on:
      - phpipam-mariadb

  phpipam-mariadb:
    image: mariadb:latest
    environment:
      - MYSQL_ROOT_PASSWORD=my_secret_mysql_root_pass
    restart: unless-stopped
    volumes:
      - phpipam-db-data:/var/lib/mysql

volumes:
  phpipam-db-data:
  phpipam-logo:
  phpipam-ca:
```

### 3. 启动容器

进入目标目录：

```bash
cd /opt/docker/phpIPAM/
```

启动容器：

```bash
docker-compose up -d
```
