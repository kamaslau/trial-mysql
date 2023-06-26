# trial-mysql

A [MySQL](https://dev.mysql.com/doc/)/[MariaDB](https://mariadb.org/documentation/) bootstraper, using [Adminer](https://www.adminer.org/) as Web UI。

## Service URL

- Database (MySQL/MariaDB) [http://localhost:3306](http://localhost:3306)
- Web UI (Adminer) [http://localhost:8080](http://localhost:8080)

## Default user for dashboard

Default logins should only be used in local/dev environments.

| Username | Password |
| -------- | -------- |
| root     | 123456   |

## Usage

### Start a service group

Manage service stack with [Docker Compose](https://docs.docker.com/compose/).

```bash
# Start services
docker compose up -d

# Stop services and prune Docker volumes
docker compose down -v
```

To update containers with latest images:

```bash
docker compose pull
docker compose up -d
```

### Start services individually

Launch database first, then the UI.

#### MariaDB

```bash
docker run -p 3306:3306 -d --restart always --env MYSQL_ROOT_PASSWORD=123456 --name=mysql mariadb:latest
```

#### MySQL

```bash
docker run -p 3306:3306 -d --restart always --env MYSQL_ROOT_PASSWORD=123456 --name=mysql mysql:latest
```

#### Adminer

Pass in to link the database container with param `--link database-container-name:db` .

```bash
docker run -p 8080:8080 -d --restart always --link mysql:db --name adminer adminer:latest
```

### Further operations

```bash
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
- [MariaDB](https://hub.docker.com/_/mariadb)
- [MySQL](https://hub.docker.com/_/mysql)
