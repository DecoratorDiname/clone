# Работа с Docker и Apache из готового образа

Цель: описать все шаги по запуску Apache внутри Docker-контейнера на основе готового образа, используя пример с Debian и веб-сервером Apache.
##  Проверить Docker
Получить версию установленного у вас Docker
```bash
docker version
```

![alt text](image-9.png)

## Подготовка Docker (чтобы начать работать с “чистого листа”)
Остановить все запущенные контейнеры
Удалить все остановленные контейнеры
Удалить все неиспользуемые образы

- Следует убедиться, нет ли у вас уже установленных и запущенных контейнеров:
```bash
docker ps -a
```
- Если есть, то лучше их остановить:
```bash
docker stop $(docker ps -q)
```
- Если остановленные контейнеры не нужно, то удалить их:
```bash
docker container prune
```
или
```bash
docker container prune $(docker ps -q)
```
- Ещё раз убедиться, что нет лишних контейнеров:
```bash
docker ps -a
```

![alt text](image-10.png)

- Опционально можно удалить ненужные образы. Показать текущие образы:
```bash
docker images
```
- Удалить все ненужные образы
```bash
docker image prune -a
```
или
```bash
docker rmi $(docker images -q)
```

##  Поиск готового образа Apache с Debian (самый первый по списку)

В Docker Hub есть несколько образов с Apache. Обычно самый простой путь — официальный образ httpd, который базируется на Debian/Alpine. В примере ниже использован образ httpd. При необходимости уточните конкретный тег.

- Поиск образа (не обязательно, на практике используйте напрямую команду ниже):
```bash
docker search httpd
```
![alt text](image.png)

- Пример выбора образа и запуска контейнера (самый простой вариант — официальный образ httpd):
```bash
docker run -d --name my-apache -p 8081:80 httpd
```

Примечание: данная команда запускает контейнер в фоновом режиме, пробрасывает порт 80 внутри контейнера на порт 8081 на хосте, и дает имя контейнеру my-apache.



##  Получение готового образа Apache

Получить информацию по загруженному образу:
```bash
docker inspect httpd
```
При необходимости остановить контейнер с таким именем:
```bash
docker stop my-apache
```
Перезапустить контейнер по имени
```bash
docker restart my-apache
```
Перезапустить контейнер по его id
```bash
docker restart b89c19a39051
```
Удалить выбранный контейнер по его имени
```bash
docker rm my-apache
```
![alt text](image-11.png)

И можно удалить ещё и образ загруженного ранее Apache:

Получить id образа
```bash
docker images
```
Удалить по id нужный образ
```bash
docker rmi b89c19a39051
```
![alt text](image-12.png)

## Проверить работу контейнера

Можно снова установить и запустить Apache (если его удаляли ранее)
```bash
docker run -d --name my-apache -p 8081:80 httpd
```
Показать наличие загруженного файла образа
```bash
docker images
```
![alt text](image-13.png)

Показать только запущенные контейнеры
```bash
docker ps
```
или показать все контейнеры (в т.ч. остановленные)
```bash
docker ps -a
```
![alt text](image-14.png)

Показать работающий Apache

Способ 1
```bash
curl http://localhost:8081
```
![alt text](image-15.png)

Способ 2 - открыть http://localhost:8081 адрес в браузере

![alt text](image-16.png)

## Управление контейнером
Мониторинг контейнеров
Показать состояние всех контейнеров
```bash
docker ps -a
```
Показать подробности о контейнере
```bash
docker inspect my-apache
```
Запустить мониторинг контейнеров
```bash
docker stats
```
![alt text](image-17.png)

Получить лог контейнера
```bash
docker logs my-apache
```
Показать логи в режиме ожидания
```bash
docker logs -f my-apache
```
![alt text](image-18.png)

Остановить контейнер
```bash
docker stop my-apache
```
Снова запустить контейнер
```bash
docker start my-apache
```
Перезапустить контейнер
```bash
docker restart my-apache
```
Зайти в контейнер
```bash
docker exec -it my-apache /bin/bash
```
или
```bash
docker exec -it my-apache bash
```
или
```bash
docker exec -it my-apache /bin/sh
```
или
```bash
docker exec -it my-apache sh
```
внутри контейнера можно повыполнять некоторые команды Linux Получить информацию об ОС контейнера
```bash
uname -a
```

![alt text](image-19.png)

Получить больше информации об ОС контейнера
```bash
cat /etc/os-release
```

![alt text](image-20.png)

Установить Fastfetch
```bash
apt update && apt install -y fastfetch
```
```bash
fastfetch
```

![alt text](image-21.png)

Можно установить ещё несколько приложений внутри Docker-контейнера:
```bash
apt update && apt install -y fastfetch htop cmatrix hollywood mc micro
```
и позапускать их отдельно друг от друга:
```bash
htop
```

![alt text](image-23.png)

```bash
cmatrix
```

![alt text](image-22.png)

```bash
hollywood
```

![alt text](image-24.png)

Выйти из контейнера можно командой exit

Остановить все запущенные контейнеры
```bash
docker stop $(docker ps -q)
```
Удалить все остановленные контейнеры
```bash
docker container prune $(docker ps -q)
```
Удалить все образы
```bash
docker rmi $(docker images -q)
```