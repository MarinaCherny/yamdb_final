# CI и CD проекта api_yamdb

![yamdb_final workflow status](https://github.com/MarinaCherny/yamdb_final/actions/workflows/main.yml/badge.svg)

## Описание
Для приложения настроено Continuous Integration и Continuous Deployment: автоматический запуск тестов, обновление образов на Docker Hub, автоматический деплой на боевой сервер при пуше в главную ветку GitHub.

Для Continuous Integration в проекте используется облачный сервис GitHub Actions.
Для этого описана последовательность команд (workflow), которая будет выполняться после события push в репозиторий.
