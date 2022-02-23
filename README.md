# trial-mysql

MySQL 选型/快速试用。UI 为 Adminer。

## 路径

- [DB@3306](http://localhost:3306)
- [UI@8080](http://localhost:8080)

## 初始账号

| 用户名 | 密码   |
| ------ | ------ |
| root   | 123456 |

## 命令行工具

通过 docker-compose 管理服务栈（含存储容器、网络等配置）：

```shell
# 启动服务栈
npm run docker:up

# 停用并移除服务栈（及其数据卷）
npm run docker:down
```

## 分别启动容器

先启动数据库（MySQL/MariaDB）容器，再启动 UI 容器。

### MySQL

```shell
docker run -p 3306:3306 -d --restart always --env MYSQL_ROOT_PASSWORD=123456 --name=mysql mysql:latest
```

### MariaDB

```shell
docker run -p 3306:3306 -d --restart always --env MYSQL_ROOT_PASSWORD=123456 --name=mariadb mariadb:latest
```

### Adminer

通过使用 `--link 数据库容器名:db` 参数，传入要在 Adminer 中操作的数据库。

```shell
docker run -p 8080:8080 -d --restart always --link mysql:db --name adminer adminer:latest
```

## 操作容器

```shell
# 启动并挂载命令行工具到容器内
docker exec -it [容器名称] sh

# 列出已安装插件
ls -l /usr/lib/mysql/plugin
```

### 错误处理

## Host '172.18.0.1' is not allowed to connect to this MySQL server

[用户无权通过当前网络访问MySQL](https://github.com/docker-library/mysql/issues/275)。

```
docker exec -it trial-mysql_db_1 bash # 进入容器内部命令行
mysql -u root -p # 使用root用户名登录，会提示输入密码
CREATE USER 'root'@'%' IDENTIFIED BY '123456'; # 创建通过所有网络访问MySQL的root用户
grant all on *.* to 'root'@'%'; # 授予全部权限给上述用户
select host ,user from mysql.user; # 确认用户创建成功
```

## 相关 Docker 镜像

- [Adminer](https://hub.docker.com/_/adminer)
- [MySQL](https://hub.docker.com/_/mysql)
- [MariaDB](https://hub.docker.com/_/mariadb)
