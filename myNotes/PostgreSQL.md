##  Проверить Docker
Получить версию установленного у вас Docker
```bash
docker version
```

![alt text](image-34.png)

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
![alt text](image-35.png)

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
# Поиск готового образа PostgreSQL
```bash
docker run -d \
  --name my-postgres \
  -p 5432:5432 \
  -e POSTGRES_PASSWORD=mysecretpassword \
  postgres:alpine
```
##  Получение готового образа PostgreSQL

Получить информацию по загруженному образу:
```bash
docker inspect my-postgres
```
При необходимости остановить контейнер с таким именем:
```bash
docker stop my-postgres
```
Перезапустить контейнер по имени
```bash
docker restart my-postgres
```
Перезапустить контейнер по его id
```bash
docker restart 4da1a4828be1
```
Удалить выбранный контейнер по его имени
```bash
docker rm my-postgres
```
![alt text](image-36.png)

И можно удалить ещё и образ загруженного ранее PostgreSQL:

Получить id образа
```bash
docker images
```
Удалить по id нужный образ
```bash
docker rmi 4da1a4828be1
```
![alt text](image-37.png)

## Проверить работу контейнера

Можно снова установить и запустить PostgreSQL (если его удаляли ранее)
```bash
docker run -d \
  --name my-postgres \
  -p 5432:5432 \
  -e POSTGRES_PASSWORD=mysecretpassword \
  postgres:alpine
```
Показать наличие загруженного файла образа
```bash
docker images
```
![alt text](image-89.png)

Показать только запущенные контейнеры
```bash
docker ps
```
или показать все контейнеры (в т.ч. остановленные)
```bash
docker ps -a
```
![alt text](image-90.png)


## Управление контейнером
Показать состояние всех контейнеров
```bash
docker ps -a
```
Показать подробности о контейнере
```bash
docker inspect my-postgres
```
Запустить мониторинг контейнеров
```bash
docker stats
```
![alt text](image-91.png)

Получить лог контейнера
```bash
docker logs my-postgres
```
Показать логи в режиме ожидания
```bash
docker logs -f my-postgres
```
![alt text](image-92.png)

Остановить контейнер
```bash
docker stop my-postgres
```
Снова запустить контейнер
```bash
docker start my-postgres
```
Перезапустить контейнер
```bash
docker restart my-postgres
```
Зайти в контейнер
```bash
docker exec -it my-postgres /bin/bash
```
или
```bash
docker exec -it my-postgres bash
```
или
```bash
docker exec -it my-postgres /bin/sh
```
или
```bash
docker exec -it my-postgres sh
```
внутри контейнера можно повыполнять некоторые команды Linux Получить информацию об ОС контейнера
```bash
uname -a
```

![alt text](image-43.png)

Получить больше информации об ОС контейнера
```bash
cat /etc/os-release
```

![alt text](image-32.png)

Выйти из контейнера можно командой exit

Подключиться:
```bash
docker exec -it my-postgres psql -U postgres
```

![alt text](image-93.png)

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