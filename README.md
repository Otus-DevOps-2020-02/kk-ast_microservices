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
