# 使用
```
docker-compose up -d .

docker-compose -f docker-compose.yml up -d

docker exec -it mongo /bin/bash

mongo
use admin
db.createUser({user:"root",pwd:"密码：12345678",roles:[{role:'root',db:'admin'}]})
exit
exit

```

账号：root
密码：12345678


连接URI `mongodb://root@127.0.0.1:27017/?authSource=admin`