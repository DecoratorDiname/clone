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
# Поиск готового образа cAdvisor
```bash
docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:rw \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --publish=8080:8080 \
  --detach=true \
  --restart=always \
  --name=cadvisor \
  gcr.io/cadvisor/cadvisor:latest
```
##  Получение готового образа cAdvisor

Получить информацию по загруженному образу:
```bash
docker inspect cadvisor
```
При необходимости остановить контейнер с таким именем:
```bash
docker stop cadvisor
```
Перезапустить контейнер по имени
```bash
docker restart cadvisor
```
Перезапустить контейнер по его id
```bash
docker restart aa9694377d3b
```
Удалить выбранный контейнер по его имени
```bash
docker rm cadvisor
```
![alt text](image-36.png)

И можно удалить ещё и образ загруженного ранее cadvisor:

Получить id образа
```bash
docker images
```
Удалить по id нужный образ
```bash
docker rmi aa9694377d3b
```
![alt text](image-37.png)

## Проверить работу контейнера

Можно снова установить и запустить cAdvisor (если его удаляли ранее)
```bash
docker run \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:rw \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --publish=8080:8080 \
  --detach=true \
  --restart=always \
  --name=cadvisor \
  gcr.io/cadvisor/cadvisor:latest
```
Показать наличие загруженного файла образа
```bash
docker images
```
![alt text](image-38.png)

Показать только запущенные контейнеры
```bash
docker ps
```
или показать все контейнеры (в т.ч. остановленные)
```bash
docker ps -a
```
![alt text](image-39.png)

Проверить порт 8080 для Linux/Mac/WSL:
```bash
# Проверьте, занят ли порт
netstat -tuln | grep :8080
```
Проверить порт 8080 для Windows:
```bash
netstat -aon | findstr :8080
```
Откройте: http://localhost:8080

![alt text](image-40.png)

## Управление контейнером
Показать состояние всех контейнеров
```bash
docker ps -a
```
Показать подробности о контейнере
```bash
docker inspect cadvisor
```
Запустить мониторинг контейнеров
```bash
docker stats
```
![alt text](image-41.png)

Получить лог контейнера
```bash
docker logs cadvisor
```
Показать логи в режиме ожидания
```bash
docker logs -f cadvisor
```
![alt text](image-42.png)

Остановить контейнер
```bash
docker stop cadvisor
```
Снова запустить контейнер
```bash
docker start cadvisor
```
Перезапустить контейнер
```bash
docker restart cadvisor
```
Зайти в контейнер
```bash
docker exec -it cadvisor /bin/bash
```
или
```bash
docker exec -it cadvisor bash
```
или
```bash
docker exec -it cadvisor /bin/sh
```
или
```bash
docker exec -it cadvisor sh
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