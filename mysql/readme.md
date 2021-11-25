# 安装

```bash
mkdir -p mysql/db mysql/conf mysql/init

cd mysql
```

 docker-compose.yml

```yaml
services:
  mysql:
		container_name: mysql5.7
    environment:
      MYSQL_ROOT_PASSWORD: B9tiNZ7dXrk2qwr6
    image: mysql:5.7
    network_mode: bridge
    ports:
    - 13306:3306/tcp
    restart: always
    volumes:
    - /home/ubuntu/docker/mysql/db:/var/lib/mysql:rw
    - /home/ubuntu/docker/mysql/conf/my.cnf:/etc/my.cnf:rw
    - /home/ubuntu/docker/mysql/init:/docker-entrypoint-initdb.d:rw
version: '3.0'
```

my.cnf

```
[mysqld]
user=mysql
default-storage-engine=INNODB
character-set-server=utf8mb4
[client]
default-character-set=utf8mb4
[mysql]
default-character-set=utf8mb4
```

**init.sql（非必要）**

```sql
create database test;
use test;
create table user
(
	id int auto_increment primary key,
	username varchar(64) unique not null,
	email varchar(120) unique not null,
	password_hash varchar(128) not null,
	avatar varchar(128) not null
);
insert into user values(1, "zhangsan","test12345@qq.com","passwd","avaterpath");
insert into user values(2, "lisi","12345test@qq.com","passwd","avaterpath");
```

```bash
docker-compose up -d
```