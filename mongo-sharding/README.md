# mongo-sharding

## Как запустить

* Запускаем mongodb и приложение

```shell
docker compose up -d
```


### Автоматическая настройка

```shell
./mongo-init.sh
```


### Ручная настройка

* **Шаг 1.** Настройка сервера конфигурации
    * Настройка сервера конфигурации: поключение
    ```shell
    docker exec -it configSrv mongosh --port 27017
    ```

    * Настройка сервера конфигурации: инициализация
    ```js
    rs.initiate(
    {
        _id : "config_server",
        configsvr: true,
        members: [
        { _id : 0, host : "configSrv:27017" }
        ]
    }
    );
    ```

* **Шаг 2.** Настройка шардов
    * Настройка шарда 1: поключение
    ```shell
    docker exec -it shard1 mongosh --port 27018
    ```
    * Настройка шарда 1: инициализация
    ```js
    rs.initiate(
    {
      _id : "shard1",
      members: [
        { _id : 0, host : "shard1:27018" },
      ]
    }
    );
    ```

    * Настройка шарда 2: поключение
    ```shell
    docker exec -it shard2 mongosh --port 27019
    ```
    * Настройка шарда 2: инициализация
    ```js
    rs.initiate(
    {
      _id : "shard2",
      members: [
        { _id : 0, host : "shard2:27019" },
      ]
    }
    );
    ```

* **Шаг 3.** Настройка роутера
    * Настройка роутера: поключение
    ```shell
    docker exec -it mongos_router mongosh --port 27020
    ```

    * Настройка роутера: инициализация
    ```js
    sh.addShard( "shard1/shard1:27018");
    sh.addShard( "shard2/shard2:27019");
    sh.enableSharding("somedb");
    sh.shardCollection("somedb.helloDoc", { "name" : "hashed" } )
    ```

* **Шаг 4.** Загрузка данных
    * Поключение
    ```shell
    docker exec -it mongos_router mongosh --port 27020
    ```
    * Загрузка
    ```js
    use somedb
    for(var i = 0; i < 1000; i++) db.helloDoc.insertOne({age:i, name:"ly"+i})
    ```
    