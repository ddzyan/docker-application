[docker-compose.yml](https://www.notion.so/docker-compose-yml-9f2efc4fc2a344f58ecf3418836603b4)

## **ELK安装**

```bash
$ git clone https://github.com/deviantony/docker-elk.git

$ cd docker-elk

# 后台守护进程模式启动所有服务
$ docker-compose up -d

# 检查服务状态
$ docker-compose ps
CONTAINER ID        IMAGE                      COMMAND                  CREATED             STATUS              PORTS                                                                              NAMES
e306f6fae7bf        docker-elk_kibana          "/usr/local/bin/dumb…"   16 seconds ago      Up 14 seconds       0.0.0.0:5601->5601/tcp                                                             docker-elk_kibana_1
e906b1d1b8db        docker-elk_logstash        "/usr/local/bin/dock…"   16 seconds ago      Up 14 seconds       0.0.0.0:5000->5000/tcp, 0.0.0.0:9600->9600/tcp, 0.0.0.0:5000->5000/udp, 5044/tcp   docker-elk_logstash_1
f32071efd7f3        docker-elk_elasticsearch   "/tini -- /usr/local…"   16 seconds ago      Up 15 seconds       0.0.0.0:9200->9200/tcp, 0.0.0.0:9300->9300/tcp                                     docker-elk_elasticsearch_1
```

docker swarm 启动

```
docker stack deploy -c docker-stack.yml elk
```

打开浏览器地址：[http://127.0.0.1:5601/](http://127.0.0.1:5601/)

账号：elastic

密码：changeme

### **配置文件修改**

以下操作都修改 docker-compose.yml 文件

1. 设置最大堆的值不能超过你物理内存的50%，要确保有足够多的物理内存来保证内核文件缓存。

```
elasticsearch:
  environment:
    ES_JAVA_OPTS: "-Xmx256m -Xms256m"
```

1. 设置 kibana 默认语言为中文

```
kibana
  environment:
    I18N_LOCALE: "zh-CN"
```

---

## **APM安装**

```
$ cd /opt

$ curl -L -O https://artifacts.elastic.co/downloads/apm-server/apm-server-7.8.0-x86_64.rpm

$ sudo rpm -vi apm-server-7.8.0-x86_64.rpm
```

修改 apm-server.yml 配置文件

```
$ vim /etc/apm-server/apm-server.yml
[apm-server.yml]
apm-server:
  host: "0.0.0.0:8200"
kibana:
  enabled: true
  host: "localhost:5601"

  protocol: "http"
  username: "elastic"
  password: "changeme"

output.elasticsearch:
  hosts: ["localhost:9200"]
  username: "elastic"
  password: "changeme"
service apm-server start
```

进入后台 [http://127.0.0.1:5601/](http://127.0.0.1:5601/), 检查apm服务是否正常，点击 check apm server status

## **备注**

请在 kibana 启动完毕后，再启动 apm-server 服务，可以通过访问: [http://127.0.0.1:5601/](http://127.0.0.1:5601/), 确认kibana状态

### **修改密码**

操作步骤，注意密码必须为string:

1. 修改 docker-compose.yml 中 elasticsearch 的环境变量 ELASTIC_PASSWORD
2. 修改 logstash/config/logstash.yml 中密码
3. 修改 kibana/config/kibana.yml 中密码
4. 修改 /etc/apm-server/apm-server.yml

```
service apm-server restart

docker-compose restart
```

### **服务端口**

1. 5000: Logstash TCP input.
2. 9200: Elasticsearch HTTP
3. 9300: Elasticsearch TCP transport
4. 5601: Kibana
5. 8200: APM

### **修改 kibana日志时间格式**

[http://127.0.0.1:5601/app/kibana#/management/kibana/settings](http://127.0.0.1:5601/app/kibana#/management/kibana/settings)

找到 Advanced Settings-->Date format 修改这里的值，默认是MMMM Do YYYY, HH:mm:ss.SSS,可以改成YYYY-MM-DD HH:mm:ss.SSS，这样页面的所有日期就会显示成2019-04-23 16:30:39.139这种格式