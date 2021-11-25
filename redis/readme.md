[使用docker-compose创建Redis - 在线打工者 - 博客园 (cnblogs.com)](https://www.cnblogs.com/lucky9322/p/13648259.html)

# 安装

```bash
mkdir -p ./redis/data

cd ./redis
```

docker-compose.yml

```sql
version: '3'

services:
  redis:
    image: redis:latest
    container_name: redis
    restart: always
    ports:
      - 16379:6379
    volumes:
      - ./redis.conf:/usr/local/etc/redis/redis.conf:rw
      - ./data:/data:rw
    command:
      /bin/bash -c "redis-server /usr/local/etc/redis/redis.conf "
networks:
  mynetwork:
    external: true
```

redis.conf

```
bind 0.0.0.0
protected-mode no
port 6379
timeout 0
save 900 1 # 900s内至少一次写操作则执行bgsave进行RDB持久化
save 300 10
save 60 10000
rdbcompression yes
dbfilename dump.rdb
dir /data
appendonly yes
appendfsync everysec
requirepass nedsK6uagHZ1Ymzs
```

```bash
docker-compose up -d
```