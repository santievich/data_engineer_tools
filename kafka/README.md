Для запуска в фоновом режиме использовать 
```docker-compose up -d```

Необходимо создать топик.
Для этого нужно войти внутрь контейнера kafka:

```docker exec -it kafka /bin/bash```

Создаем топик с именем test_topic с фактором репликации 1 и одной партицией:

```kafka-topics --create --topic test_topic --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1```

Создаем сеть внутри контейнера кафки и связываем консоль с кафкой с помощью kafka-console-producer. Т.е. в продюссеры добавляем консоль:

```docker run -it --network kafka-docker_default confluentinc/cp-kafka:latest kafka-console-producer --bootstrap-server kafka:9092 --topic test_topic```

Заставляем kafka отправлять сообщения в консоль с помощью kafka-console-consumer. В консюмеры добавляем консоль:

```docker run -it --network kafka-docker_default confluentinc/cp-kafka:latest kafka-console-consumer --bootstrap-server kafka:9092 --topic test_topic --from-beginning```

Теперь в одной консоли можно писать сообщения, которые будут попадать в другую консоль через kafka.

Интерфейс kafka-ui доступен по адресу 

```http://localhost:8081```
