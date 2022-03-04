# 简介

**Portainer** 是一款轻量级的图形化管理工具，通过它我们可以轻松管理不同的 **docker** 环境。**Portainer**部署和使用都非常的简单，它由一个可以运行在任何**docker**引擎上的容器组成。

```yaml
services:
  portainer:
    command: -H unix:///var/run/docker.sock
    image: portainer/portainer
    ports:
    - 9000:9000/tcp
    volumes:
    - /var/run/docker.sock:/var/run/docker.sock:rw
    - data:/data:rw
version: '3.0'
volumes:
  data: {}
```

```bash
docker-compose up -d
```

open url: **[http://localhost:9000/#/init/admin](http://localhost:9000/#/init/admin)**