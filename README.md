# trial-mysql

Trial or micro-service unit of [MySQL](https://dev.mysql.com/doc/)/[MariaDB](https://mariadb.org/documentation/) with [Adminer](https://www.adminer.org/) as Web UI.

For PostgreSQL bootstrapper, checkout the [trial-postgresql](https://github.com/kamaslau/trial-postgresql).

## Service URL

- Database (MySQL/MariaDB) [http://localhost:3306](http://localhost:3306)
- Web UI (Adminer) [http://localhost:8080](http://localhost:8080)

## Default user

Default logins should only be used in local/dev environments.

| Username | Password |
| -------- | -------- |
| root     | 123456   |

## Usage

### Start with [Docker Compose](https://docs.docker.com/compose/)

```bash
# Initiate .env file
cp .env_template .env
# Start services
docker compose up -d
```

Update existing composed containers with latest images:

```bash
docker compose pull && \
docker compose down && \
docker compose up -d
```

### Start services individually

Launch database first, then the UI.

#### MariaDB

```bash
docker run -p 3306:3306 -d --restart always --env MYSQL_ROOT_PASSWORD=123456 --name mysql mariadb:latest
```

#### MySQL

```bash
docker run -p 3306:3306 -d --restart always --env MYSQL_ROOT_PASSWORD=123456 --name mysql mysql:latest
```

#### Adminer

Pass in to link the database container with param `--link database-container-name:db` .

```bash
docker run -p 8080:8080 -d --restart always --env ADMINER_DEFAULT_SERVER=mysql --name adminer adminer:latest
```

### Further operations

```bash
# Enter container and initiate shell
docker exec -it mysql bash

# List installed plugins
ls -l /usr/lib/mysql/plugin
```

### Error handling

## Host '172.18.0.x' is not allowed to connect to this MySQL server

[用户无权通过当前网络访问 MySQL](https://github.com/docker-library/mysql/issues/275)。

```bash
# Enter container and initiate shell
docker exec -it mysql bash

# Login as root user
mysql -u root -p
```

```sql
-- Create root user visiting from any network with password 123456 (change this for safety)
CREATE USER 'root'@'%' IDENTIFIED BY '123456';
grant all privileges on *.* to 'root'@'%';
-- grant all privileges on *.* to 'root'@'%' identified by '123456';

-- Verify the user creation
select host ,user from mysql.user;
```

## Relevent Docker images

- [Adminer](https://hub.docker.com/_/adminer)
- [MariaDB](https://hub.docker.com/_/mariadb)
- [MySQL](https://hub.docker.com/_/mysql)

## References

- https://dev.mysql.com/doc/refman/8.0/en/docker-mysql-getting-started.html
