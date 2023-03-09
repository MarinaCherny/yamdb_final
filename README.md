# CI и CD проекта api_yamdb

![example workflow](https://github.com/MarinaCherny/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

Проект развернут по адресу http://84.201.154.76/redoc

## Описание
!!!Для приложения настроено Continuous Integration и Continuous Deployment: автоматический запуск тестов, обновление образов на Docker Hub, автоматический деплой на боевой сервер при пуше в главную ветку GitHub.

Для Continuous Integration в проекте используется облачный сервис GitHub Actions.
Для этого описана последовательность команд (workflow), которая будет выполняться после события push в репозиторий.

Само приложение взято из проекта api_yamdb, которое представляет собой API сервиса отзывов о фильмах, книгах и музыке. Зарегистрированные пользователи могут оставлять отзывы (Review) на произведения (Title).

## Развертывание проекта

### Развертывание на локальном сервере

1. Установите на сервере docker и docker-compose.
2. Создайте файл /infra/.env. Шаблон для заполнения файла нахоится в /infra/.env.sample
3. Выполните команду ```docker-compose up -d --buld```
4. Выполните миграции ```docker-compose exec web python manage.py migrate```
5. Создайте суперюзера ```docker-compose exec web python manage.py createsuperuser```
6. Соберите статику ```docker-compose exec web python manage.py collectstatic --no-input```
7. При необходимости заполните базу ```docker-compose exec web python manage.py loaddata fixtures.json```

### Подготовка удалённого сервера
1. Войти на удалённый сервер ```ssh <username>@<ip_address>```
2. Установить docker и docker-compose: ```sudo apt install docker.io```  ``` sudo apt install docker-compose -y```
3. Скопировать файлы docker-compose.yaml и nginx/templates/default.conf.template из проекта (локально) на сервер в home/<username>/docker-compose.yaml и home/<username>/nginx/templates/default.conf.template соответственно:
  + перейти в директорию с файлом docker-compose.yaml и выполните:
    ```scp docker-compose.yaml <username>@<ip_address>:/home/<username>/docker-compose.yaml```
  + перейти в директорию с файлом default.conf.template и выполните:
    ```scp default.conf.template <username>@<ip_address>:/home/<username>/nginx/templates/default.conf.template```
4. Добавьте в GitHub в secrets следующие переменные:
    ```
    DB_ENGINE=django.db.backends.postgresql
    DB_NAME=имя базы данных postgres
    DB_USER=пользователь бд
    DB_PASSWORD=пароль
    DB_HOST=db
    DB_PORT=5432

    DOCKER_PASSWORD=пароль от DockerHub
    DOCKER_USERNAME=имя пользователя

    SECRET_KEY=секретный ключ проекта django

    USER=username для подключения к серверу
    HOST=IP сервера
    PASSPHRASE=пароль для сервера, если он установлен
    SSH_KEY=ваш SSH ключ (для получения команда: cat ~/.ssh/id_rsa)

    TELEGRAM_TO=ID чата, в который придет сообщение
    TELEGRAM_TOKEN=токен вашего бота
    ```    
    При push в ветку main автоматически отрабатывают сценарии:

### Workflow
При push в ветку main автоматически отрабатывают сценарии:

- tests - проверка кода на соответствие стандарту PEP8 и запуск pytest
- build_and_push_to_docker_hub - сборка и доставка докер-образов на DockerHub
- deploy - автоматический деплой проекта на боевой сервер
- send_message - отправка уведомления в Telegram.

### После успешного деплоя
При удачном прохождении workflow для окончательной настройки, зайдите на уделенный сервер и выполните от  миграции, создайте суперюзера, соберите статику и заполните базу 
1. Выполните миграции ```docker-compose exec web python manage.py migrate```
2. Создайте суперюзера ```docker-compose exec web python manage.py createsuperuser```
3. Соберите статику ```docker-compose exec web python manage.py collectstatic --no-input```
4. При необходимости заполните базу ```docker-compose exec web python manage.py loaddata fixtures.json```

Проверьте работоспособность приложения.
Документация к API находится по адресу: http://localhost/redoc/