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

docker-machine пример запуска:

```
$ export GOOGLE_PROJECT=_ваш-проект_

# Создать докер-хост
docker-machine create --driver google \
    --google-machine-image https://www.googleapis.com/compute/v1/projects/ubuntu-os-cloud/global/images/family/ubuntu-1604-lts \
    --google-machine-type n1-standard-1 \
    --google-zone europe-west1-b \
    docker-host

# Настроить докер на удаленный хост
eval $(docker-machine env docker-host)

# Переключение на локальный докер
eval $(docker-machine env --unset)

# Узнать IP-адрес
$ docker-machine ip docker-host

# Удалить instance
$ docker-machine rm docker-host

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
## docker-4

Цели:
- Научиться работе с сетями в Docker
- Научиться работать с docker-compose

Утилиты:
- bridge-utils

Для просмотра net namespaces
```
sudo ln -s /var/run/docker/netns /var/run/netns
sudo ip netns
```

Создание сетей:
```
docker network create back_net --subnet=10.0.2.0/24
docker network create front_net --subnet=10.0.1.0/24
```

Подключение контейнера к сети:
```
docker network connect <network> <container>
```

## docker-compose

Конфигурация в .yml (https://docs.docker.com/compose/compose-file/), параметры передаются через переменные окружения или .env файл, для изменения имени проекта используется переменная COMPOSE_PROJECT_NAME

```
docker-compose up -d #запуск
docker-compose ps #список
docker-compose down #выключение
```

## gitlab-ci-1

Цели:
- Подготовить инсталляцию Gitlab CI
- Подготовить репозиторий с кодом приложения
- Описать для приложения этапы пайплайна
- Определить окружения

Подготовлен, запущен и настроен свой инстанс с Gitlab, созданы пользователь для работы и проект. Настроена отправка кода.

GitLab CI/CD, рассмотрены:
- Pipelines https://docs.gitlab.com/ee/ci/pipelines/index.html
- Environment variables https://docs.gitlab.com/ee/ci/variables/README.html
- Environments https://docs.gitlab.com/ee/ci/environments/index.html
- Job artifacts https://docs.gitlab.com/ee/ci/pipelines/job_artifacts.html
- Cache dependencies https://docs.gitlab.com/ee/ci/caching/index.html
- Runners https://docs.gitlab.com/runner/
- Примеры выкладки https://docs.gitlab.com/ee/ci/examples/

## monitoring-1

Цели:

- Знакомство с Prometheus: запуск и конфигурация
- Настройка мониторинга состояния сервисов
- Сбор метрик

Сделано:
- Подготовлено окружение
- Запущен Prometheus, проанализирован веб-интерфейс
- Создан docker образ Prometheus с предустановленной конфигурацией по мониторингу приложения
- Подготовлен сценарий docker-compose для запуска приложения вместе с мониторингом, включая мониторинг состояния микросервисов
- Проведены эксперименты по включению/отключению контейнеров и изменений в мониторинге

Ссылки на репозитории:

https://hub.docker.com/repository/docker/kkast2020/prometheus
https://hub.docker.com/repository/docker/kkast2020/post
https://hub.docker.com/repository/docker/kkast2020/comment
https://hub.docker.com/repository/docker/kkast2020/ui
https://hub.docker.com/repository/docker/kkast2020/otus-reddit

## monitoring-2

Сделано:
- Настроен мониторинга контейнеров с помощью prometheus
- Настроен сбор метрик уровня приложений и бизнес-логики
- Настроена визуализации в grafana, дашборды мониторинга инфраструктуры и приложений

Ссылки на репозитории:

https://hub.docker.com/repository/docker/kkast2020/prometheus
https://hub.docker.com/repository/docker/kkast2020/alertmanager
https://hub.docker.com/repository/docker/kkast2020/post
https://hub.docker.com/repository/docker/kkast2020/comment
https://hub.docker.com/repository/docker/kkast2020/ui


##logging-1

Сделано:
Настроен сбор неструктурированных/структурированных логов с помощью EFK (Elasticsearch, Fluentd, Kibana)
Настроен индекс в Kibana
Настроена трассировка с помощью zipkin

копирование данных из git
```
svn export https://github.com/express42/reddit.git/branches/logging/
```

Перезапуск отдельного контейнера с удалением:
```
docker-compose stop ui
docker-compose rm ui
docker-compose up -d
```
