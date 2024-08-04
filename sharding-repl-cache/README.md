# mongo-sharding

## Как проверить

```shell
curl localhost:8080
curl localhost:8080/helloDoc/count

time curl localhost:8080/helloDoc/users
time curl localhost:8080/helloDoc/users
time curl localhost:8080/helloDoc/users

docker exec -it shard1-1 mongosh 
use somedb;
db.helloDoc.countDocuments();
exit();

docker exec -it shard2-1 mongosh
use somedb;
db.helloDoc.countDocuments();
exit(); 

docker exec -it shard2-2 mongosh
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
