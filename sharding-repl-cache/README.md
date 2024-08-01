# mongo-sharding

## Как првоерить

```shell
curl localhost:8080
curl localhost:8080/helloDoc/count

docker exec -it shard1-1 mongosh --port 27011
use somedb;
db.helloDoc.countDocuments();
exit();

docker exec -it shard2 -1mongosh --port 27021
use somedb;
db.helloDoc.countDocuments();
exit(); 

docker exec -it shard2-2 mongosh --port 27022
use somedb;
db.helloDoc.countDocuments();
exit(); 
```

## Как запустить

* Запускаем mongodb и приложение

```shell
docker compose up -d
```


### Автоматическая настройка

```shell
./mongo-init.sh
```
