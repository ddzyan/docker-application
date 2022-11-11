# 简介
搭建 Prometheus + Grafana 监控 node 程序

# 使用
修改采集的 targets.json：文件里面 ${ip} 替换为 Node.js 应用所在服务器的 ip 地址

```shell
$  docker-compose up -d
```

至此，Prometheus 已经会去拉取我们 Node 应用程序的指标数据了。

如果想要更新 target 怎么做： 修改了这个 targets.json 文件后，通过 prometheus 的 reload 方法进行热加载。 方法如下：
```shell
$ curl -X POST http://${prometheus的ip}:9090/-/reload
```
然后我们可以查看 prometheus 的页面也可以确认是否生效，界面地址：
```shell
http://${prometheus的ip}:9090/classic/targets
```
