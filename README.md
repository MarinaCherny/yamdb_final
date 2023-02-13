# CI и CD проекта api_yamdb

![example workflow](https://github.com/MarinaCherny/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

## Описание
Для приложения настроено Continuous Integration и Continuous Deployment: автоматический запуск тестов, обновление образов на Docker Hub, автоматический деплой на боевой сервер при пуше в главную ветку GitHub.

Для Continuous Integration в проекте используется облачный сервис GitHub Actions.
Для этого описана последовательность команд (workflow), которая будет выполняться после события push в репозиторий.

Само приложение взято из проекта api_yamdb, которое представляет собой API сервиса отзывов о фильмах, книгах и музыке. Зарегистрированные пользователи могут оставлять отзывы (Review) на произведения (Title).
