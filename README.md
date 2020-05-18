# kk-ast_microservices
kk-ast microservices repository

## docker-2

Настройка и установка ПО для работы с docker

Создание своего образа

```
docker version
docker info
docker ps -a # список всех контейнеров
docker images # список локальных образов
docker run -it ubuntu:16.04 /bin/bash # запуск контейнера --rm после остановки будет сохранено содержимое контейнера
docker start <u_container_id> # запускает остановленный контейнер
docker attach <u_container_id> # консоль
docker exec -it <u_container_id> bash # запускает новый процесс внутри контейнера bash
docker kill $(docker ps -q) # убить контейнер
docker rm/rmi # удалить контейнер/образ
```

docker-machine
```
#пример запуска
docker-machine create --driver google --google-machine-image https://www.googleapis.com/compute/v1/projects/ubuntu-os-cloud/global/images/family/ubuntu-1604-lts —google-machine-type n1-standard-1 --google-zone europe-west1-b docker-host
eval $(docker-machine env docker-host)
```

Работа с Docker Hub

## docker-3

Цели:

1. Научиться описывать и собирать образы для приложения
2. Оптимизация образов
3. Запуск и работа приложения на основе Docker-образов, docker run

linter
https://github.com/hadolint/hadolint/releases/tag/v1.17.6

При запуске хоста из-за смены IP получаем ошибку: Unable to query docker version: Get https://34.76.28.170:2376/v1.15/version: x509: certificate is valid for 35.187.52.239, not 34.76.28.170

```
docker-machine regenerate-certs name
eval $(docker-machine env docker-host)
```

## Оптимизация

До:
```
kkast2020/comment       1.0                 defd21d83cfb        51 minutes ago       784MB
kkast2020/ui            1.0                 7241dbc130f5        52 minutes ago       786MB
```

После перевода на alpine, также пришлось добавить в список необходимых gem json:
```
kkast2020/comment       2.1                 534b2903f588        About a minute ago   218MB
kkast2020/ui            3.1                 ae8dbdfc9d69        8 seconds ago        221MB
```

## Запуск приложения
```
docker kill $(docker ps -q)
docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db -v reddit_db:/data/db mongo:latest
docker run -d --network=reddit --network-alias=post kkast2020/post:1.0
docker run -d --network=reddit --network-alias=comment kkast2020/comment:2.2
docker run -d --network=reddit -p 9292:9292 kkast2020/ui:3.2
```
