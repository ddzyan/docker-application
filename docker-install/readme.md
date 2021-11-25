# 记录各种系统 docker 安装
修改镜像源和默认存储位置

```bash
vim /etc/docker/daemon.json
{
  "registry-mirrors": ["https://7a1tnjfc.mirror.aliyuncs.com","http://hub-mirror.c.163.com"],
  "data-root": "/data1/docker-root" 
}

# 重启
service docker restart
```

## ubuntu
```bash
curl -sSL https://get.daocloud.io/docker | sh
```