# Docker

## Создать образ из Dockerfile

```
docker build -t my_image .
```

## Создать и запустить контейнер

```
docker run -d --restart=unless-stopped --name=my_container -p 8000:80 my_image

// -d - запуск в фоновом режиме
// -p <порт внутри контейнера>:<порт снаружи контейнера>
```

## Создать временный контейнер и запустить в нем Bash

```
docker run --rm -it eclipse-mosquitto sh
```

## Остановить контейнер

```
docker stop my_container
```

## Удалить контейнер

```
docker rm my_container

// остановить и удалить контейнер
docker rm -f my_container
```

## Удалить образ

```
docker rmi my_image
```

### Мониторинг

```
// список запущенных контейнеров
docker ps

// посмотреть логи контейнера
docker logs my_container

// следить за логами в реальном времени
docker logs -f my_container
```

## Запустить bash в контейнере

```
docker exec -it my_container sh
```

## Перезапустить контейнер

Чтобы контейнер автоматически перезапускался после перезагрузки, лучше использовать `--restart=unless-stopped` или `--restart=always`.

```
docker run -d --restart=always --name my_container ubuntu

// Или изменить политику запуска существующего контейнера

docker update --restart=always my_container
```

- `no` - (по умолчанию) Контейнер не перезапускается после остановки или перезагрузки системы.
- `always` - Контейнер всегда перезапускается, даже если был остановлен вручную.
- `unless-stopped` - Контейнер перезапускается после перезагрузки, но не перезапустится, если был остановлен вручную.
- `on-failure` - Перезапуск контейнера только при ошибке (код возврата ≠ 0).
