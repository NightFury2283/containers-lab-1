# Отчет по практической работе №1
## Студент: KAI
## Группа: 16
## Дата выполнения: 04.03.2026
### 1. Выполненные команды Docker
#### 1.1 Работа с образами
\`\`\`
# Поиск образов в Docker Hub
docker search nginx

![alt text](image.png)

# Скачивание образа
docker pull nginx:alpine

![alt text](report/image-1.png)

# Просмотр локальных образов
docker images

![alt text](report/image-2.png)

# Просмотр истории слоев образа
docker history nginx:alpine

![alt text](report/image-3.png)

# Удаление образа
docker rmi nginx:alpine

![alt text](report/image-4.png)

### Практическое задание: скачивание PostgreSQL 15 и Go 1.21
![alt text](report/image-5.png)
![alt text](report/image-6.png)

#### 1.2 Работа с контейнерами

# Запуск контейнера alpine в интерактивном режиме (при отсутствии образа на
# компьютере при выполнении этой команды должен загрузить автоматически!)
docker run -it --name test-alpine alpine:latest sh

![alt text](report/image-7.png)

# Внутри контейнера выполните: ls -la, exit. Должно отобразиться содержимое
# корневой папки контейнера alpine, затем exit выводит нас из интерактивного режима
# shell контейнера.

![alt text](report/image-8.png)


# Запуск контейнера в фоновом режиме
docker run -d --name web-server -p 8080:80 nginx:alpine

![alt text](report/image-9.png)

# Просмотр запущенных контейнеров
docker ps

![alt text](report/image-10.png)

# Просмотр всех контейнеров (включая остановленные)
docker ps -a

![alt text](report/image-11.png)

# Просмотр логов контейнера
docker logs web-server

![alt text](report/image-12.png)

# Подключение к работающему контейнеру
docker exec -it web-server sh

# Внутри контейнера выполните: cat /usr/share/nginx/html/index.html, exit

![alt text](report/image-13.png)

# Остановка контейнера
docker stop web-server

![alt text](report/image-14.png)

# Запуск остановленного контейнера
docker start web-server

![alt text](report/image-15.png)

# Удаление контейнера
docker rm web-server # предварительно остановите

![alt text](report/image-16.png)

### Практическое задание

![alt text](report/image-17.png)

#### 1.3 Работа с томами

##### Задание 2.3.1: Сохраните данные вне контейнера

# Создание именованного тома
docker volume create my-app-data

![alt text](report/image-18.png)

# Просмотр томов
docker volume ls

![alt text](report/image-19.png)

# Информация о томе
docker volume inspect my-app-data

![alt text](report/image-20.png)

# Запуск контейнера с томом
docker run -d \
--name postgres-db \
-e POSTGRES_PASSWORD=secret \
-e POSTGRES_DB=testdb \
-v my-app-data:/var/lib/postgresql/data \
-p 5432:5432 \
postgres:15-alpine

![alt text](report/image-21.png)

# Создание тестовой таблицы
docker exec -it postgres-db psql -U postgres -d testdb -c "CREATE TABLE users (id
SERIAL, name TEXT);"

![alt text](report/image-22.png)

docker exec -it postgres-db psql -U postgres -d testdb -c "INSERT INTO users
(name) VALUES ('KAI-16');"

![alt text](report/image-23.png)

# Остановка и удаление контейнера (данные сохранятся в томе)
docker stop postgres-db
docker rm postgres-db

![alt text](report/image-24.png)

# Запуск нового контейнера с тем же томом
docker run -d \
--name new-postgres \
-e POSTGRES_PASSWORD=secret \
-e POSTGRES_DB=testdb \
-v my-app-data:/var/lib/postgresql/data \
-p 5432:5432 \
postgres:15-alpine

![alt text](report/image-25.png)

# Проверка, что данные сохранились
docker exec -it new-postgres psql -U postgres -d testdb -c "SELECT * FROM users;"

![alt text](report/image-26.png)

### Практическое задание:
# Практическое задание (выполнить самостоятельно!): Создайте том для статических файлов,
# запустите Nginx с примонтированным томом, скопируйте в него файл index.html с помощью docker
# cp по пути внутри контейнера /usr/share/nginx/html и проверьте доступность страницы в браузере.
# Также нужно подставить своё ФИО и группу в месте, гже это указано.

![alt text](report/image-27.png)
![alt text](report/image-28.png)
![alt text](report/image-29.png)

##### Проверка в браузере
![alt text](report/image-30.png)

#### 1.4 Сеть в Docker

##### Задание 2.4.1: Создайте изолированную сеть для взаимодействия контейнеров

# Создание сети
docker network create my-app-network

![alt text](report/image-31.png)

# Просмотр сетей
docker network ls

![alt text](report/image-32.png)

# Запуск контейнеров в одной сети
docker run -d \
--name postgres \
--network my-app-network \
-e POSTGRES_PASSWORD=secret \
-e POSTGRES_DB=myapp \
postgres:15-alpine
docker run -d \
--name app \
--network my-app-network \
-p 8080:80 \
nginx:alpine

![alt text](report/image-33.png)

# Проверка связи между контейнерами
docker exec app ping postgres # должен быть ping

![alt text](report/image-34.png)

### Практическое задание
# Практическое задание (выполнить самостоятельно!): Создайте сеть типа bridge, запустите в ней два
# контейнера (Nginx и PostgreSQL), проверьте их взаимодействие по именам контейнеров (можно сделать
# ping из контейнера к другому контейнеру через docker exec). Отразить вывод в отчет.

![alt text](report/image-35.png)
# Создание контейнеров
![alt text](report/image-36.png)

![alt text](report/image-37.png)

###### Часть 2. Разработка многокомпонентного приложения
##### 2.1 Backend на Go
![alt text](report/image-38.png)
##### 2.2 Frontend и статика
![alt text](report/image-39.png)
##### 2.3 Скрипты инициализации БД
![alt text](report/image-40.png)

###### Часть 3. Multi-stage сборка Docker-образов
##### 3.1 Dockerfile для Go приложения
![alt text](report/image-41.png)
##### 3.2 Docker-compose для локальной разработки

###### Часть 4. GitHub Actions для автоматизации
![alt text](report/image-42.png)

#### 4.2 Настройка Secrets в репозитории
![alt text](report/image-43.png)

#### 4.3 Создание GitHub Actions workflow
![alt text](report/image-44.png)

# * ВАЖНО! Для корректной сборки в Github Actions также необходимо создать файл
# ./nginx/Dockerfile, и спомощью команд COPY поместить во внутрь образа файлы index.html и
# nginx.conf

![alt text](report/image-45.png)

#### 4.4 Тестовый docker-compose для CI
![alt text](report/image-46.png)

###### Часть 5. Скриншоты работающего приложения
### Главная страница 

### Добавление пользователя 

### Список пользователей в БД 

#### 4.5 GitHub Actions 

#### 4.6 Успешный запуск workflow 

#### 4.7 Опубликованные образы в GHCR 

###### Выводы 
[Опишите, что нового узнали, с какими трудностями столкнулись]