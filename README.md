# kk-ast_microservices
kk-ast microservices repository

## docker-1

Настройка и установка ПО для работы с docker
```
docker version
docker info
docker ps -a # список всех контейнеров
docker images # список локальных образов
docker run -it ubuntu:16.04 /bin/bash # запуск контейнера --rm после остановки будет сохранено содержимое контейнера
docker start <u_container_id> # запускает остановленный контейнер
docker attach <u_container_id> # консоль
docker exec -it <u_container_id> bash # запускает новый процесс внутри контейнера bash
```

Создание своего образа

Работа с Docker Hub
