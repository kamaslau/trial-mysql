# trial-mysql

A [MySQL](https://dev.mysql.com/doc/)/[MariaDB](https://mariadb.org/documentation/) bootstraper, using Adminer as dashboard UI。

## Service URL

- Database (MySQL/MariaDB) [http://localhost:3306](http://localhost:3306)
- Dashboard UI (Adminer) [http://localhost:8080](http://localhost:8080)

## Default user for dashboard

| Username | Password |
| -------- | -------- |
| root     | 123456   |

## Usage

### Start a service group

Manage service stack with Docker Compose, through NPM (optional).

```shell
# Start services
npm run docker:up

# Stop services and prune Docker volumes
npm run docker:down
```

### Start services individually

Launch database first, then the UI.

#### MySQL

```shell
docker run -p 3306:3306 -d --restart always --env MYSQL_ROOT_PASSWORD=123456 --name=mysql mysql:latest
```

#### MariaDB

```shell
docker run -p 3306:3306 -d --restart always --env MYSQL_ROOT_PASSWORD=123456 --name=mysql mariadb:latest
```

#### Adminer

通过使用 `--link database-container-name:db` 参数，传入要在 Adminer 中操作的数据库。

```shell
docker run -p 8080:8080 -d --restart always --link mysql:db --name adminer adminer:latest
```

### Further operations

```shell
# Enter container and initiate shell
docker exec -it mysql sh

# List installed plugins
ls -l /usr/lib/mysql/plugin
```

### Error handling

## Host '172.18.0.1' is not allowed to connect to this MySQL server

[用户无权通过当前网络访问 MySQL](https://github.com/docker-library/mysql/issues/275)。

```
docker exec -it trial-mysql_db_1 bash # 进入容器内部命令行
mysql -u root -p # 使用root用户名登录，会提示输入密码
CREATE USER 'root'@'%' IDENTIFIED BY '123456'; # 创建通过所有网络访问MySQL的root用户
grant all on *.* to 'root'@'%'; # 授予全部权限给上述用户
select host ,user from mysql.user; # 确认用户创建成功
```

## Relevent Docker images

- [Adminer](https://hub.docker.com/_/adminer)
- [MySQL](https://hub.docker.com/_/mysql)
- [MariaDB](https://hub.docker.com/_/mariadb)
