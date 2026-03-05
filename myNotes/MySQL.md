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

## Поиск готового образа MySQL
```bash
docker run -d \
  --name my-mysql \
  -p 3306:3306 \
  -e MYSQL_ROOT_PASSWORD=rootpassword \
  -e MYSQL_DATABASE=mydb \
  -e MYSQL_USER=user \
  -e MYSQL_PASSWORD=password \
  mysql:8
```

##  Получение готового образа MySQL

Получить информацию по загруженному образу:
```bash
docker inspect my-mysql
```
При необходимости остановить контейнер с таким именем:
```bash
docker stop my-mysql
```
Перезапустить контейнер по имени
```bash
docker restart my-mysql
```
Перезапустить контейнер по его id
```bash
docker restart a2d126916bc2
```
Удалить выбранный контейнер по его имени
```bash
docker rm my-mysql
```
![alt text](image-53.png)

И можно удалить ещё и образ загруженного ранее MySQL:

Получить id образа
```bash
docker images
```
Удалить по id нужный образ
```bash
docker rmi a2d126916bc2
```
![alt text](image-54.png)

## Проверить работу контейнера

Можно снова установить и запустить MySQL (если его удаляли ранее)
```bash
docker run -d \
  --name my-mysql \
  -p 3306:3306 \
  -e MYSQL_ROOT_PASSWORD=rootpassword \
  -e MYSQL_DATABASE=mydb \
  -e MYSQL_USER=user \
  -e MYSQL_PASSWORD=password \
  mysql:8
```
Показать наличие загруженного файла образа
```bash
docker images
```
![alt text](image-55.png)

Показать только запущенные контейнеры
```bash
docker ps
```
или показать все контейнеры (в т.ч. остановленные)
```bash
docker ps -a
```
![alt text](image-56.png)


## Управление контейнером
Показать состояние всех контейнеров
```bash
docker ps -a
```
Показать подробности о контейнере
```bash
docker inspect my-mysql
```
Запустить мониторинг контейнеров
```bash
docker stats
```
![alt text](image-57.png)

Получить лог контейнера
```bash
docker logs my-mysql
```
Показать логи в режиме ожидания
```bash
docker logs -f my-mysql
```
![alt text](image-58.png)

Остановить контейнер
```bash
docker stop my-mysql
```
Снова запустить контейнер
```bash
docker start my-mysql
```
Перезапустить контейнер
```bash
docker restart my-mysql
```
Зайти в контейнер
```bash
docker exec -it my-mysql /bin/bash
```
или
```bash
docker exec -it my-mysql bash
```
или
```bash
docker exec -it my-mysql /bin/sh
```
или
```bash
docker exec -it my-mysql sh
```
внутри контейнера можно повыполнять некоторые команды Linux Получить информацию об ОС контейнера
```bash
uname -a
```

![alt text](image-59.png)

Получить больше информации об ОС контейнера
```bash
cat /etc/os-release
```

![alt text](image-60.png)

Выйти из контейнера можно командой exit

Подключиться к БД
```bash
docker exec -it my-mysql mysql -u root -p
```
Пароль:rootpassword

![alt text](image-61.png)



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