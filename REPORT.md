# Отчет по практической работе №1
## Студент: KAI
## Группа: 16
## Дата выполнения: 04.03.2026

### 1. Выполненные команды Docker

#### 1.1 Работа с образами
Поиск образов в Docker Hub
docker search nginx


![alt text](report/image.png)
Скачивание образа
docker pull nginx:alpine


![alt text](report/image-1.png)
Просмотр локальных образов
docker images


![alt text](report/image-2.png)
Просмотр истории слоев образа
docker history nginx:alpine


![alt text](report/image-3.png)
Удаление образа
docker rmi nginx:alpine


![alt text](report/image-4.png)

##### Практическое задание: скачивание PostgreSQL 15 и Go 1.21

![alt text](report/image-5.png)
![alt text](report/image-6.png)


#### 1.2 Работа с контейнерами
Запуск контейнера alpine в интерактивном режиме
docker run -it --name test-alpine alpine:latest sh


![alt text](report/image-7.png)
Внутри контейнера: ls -la, exit

![alt text](report/image-8.png)
Запуск контейнера в фоновом режиме
docker run -d --name web-server -p 8080:80 nginx:alpine


![alt text](report/image-9.png)
Просмотр запущенных контейнеров
docker ps


![alt text](report/image-10.png)
Просмотр всех контейнеров (включая остановленные)
docker ps -a


![alt text](report/image-11.png)
Просмотр логов контейнера
docker logs web-server


![alt text](report/image-12.png)
Подключение к работающему контейнеру
docker exec -it web-server sh


![alt text](report/image-13.png)
Остановка контейнера
docker stop web-server


![alt text](report/image-14.png)
Запуск остановленного контейнера
docker start web-server


![alt text](report/image-15.png)
Удаление контейнера
docker rm web-server


![alt text](report/image-16.png)

##### Практическое задание: запуск PostgreSQL и выполнение SQL-запроса

![alt text](report/image-17.png)


#### 1.3 Работа с томами

##### Задание: Сохраните данные вне контейнера
Создание именованного тома
docker volume create my-app-data


![alt text](report/image-18.png)
Просмотр томов
docker volume ls


![alt text](report/image-19.png)
Информация о томе
docker volume inspect my-app-data


![alt text](report/image-20.png)
Запуск контейнера с томом
docker run -d
--name postgres-db
-e POSTGRES_PASSWORD=secret
-e POSTGRES_DB=testdb
-v my-app-data:/var/lib/postgresql/data
-p 5432:5432
postgres:15-alpine


![alt text](report/image-21.png)
Создание тестовой таблицы
docker exec -it postgres-db psql -U postgres -d testdb -c "CREATE TABLE users (id SERIAL, name TEXT);"


![alt text](report/image-22.png)
Вставка данных
docker exec -it postgres-db psql -U postgres -d testdb -c "INSERT INTO users (name) VALUES ('KAI-16');"


![alt text](report/image-23.png)
Остановка и удаление контейнера
docker stop postgres-db
docker rm postgres-db


![alt text](report/image-24.png)
Запуск нового контейнера с тем же томом
docker run -d
--name new-postgres
-e POSTGRES_PASSWORD=secret
-e POSTGRES_DB=testdb
-v my-app-data:/var/lib/postgresql/data
-p 5432:5432
postgres:15-alpine


![alt text](report/image-25.png)
Проверка сохранности данных
docker exec -it new-postgres psql -U postgres -d testdb -c "SELECT * FROM users;"


![alt text](report/image-26.png)

##### Практическое задание: том для статических файлов Nginx

![alt text](report/image-27.png)
![alt text](report/image-28.png)
![alt text](report/image-29.png)

###### Проверка в браузере

![alt text](report/image-30.png)


#### 1.4 Сеть в Docker

##### Задание: Создайте изолированную сеть для взаимодействия контейнеров
Создание сети
docker network create my-app-network


![alt text](report/image-31.png)
Просмотр сетей
docker network ls


![alt text](report/image-32.png)
Запуск контейнеров в одной сети
docker run -d --name postgres --network my-app-network -e POSTGRES_PASSWORD=secret -e POSTGRES_DB=myapp postgres:15-alpine
docker run -d --name app --network my-app-network -p 8080:80 nginx:alpine


![alt text](report/image-33.png)
Проверка связи между контейнерами
docker exec app ping postgres


![alt text](report/image-34.png)

##### Практическое задание: создание bridge сети и проверка взаимодействия

![alt text](report/image-35.png)
![alt text](report/image-36.png)
![alt text](report/image-37.png)


### 2. Разработка многокомпонентного приложения

#### 2.1 Backend на Go

![alt text](report/image-38.png)

#### 2.2 Frontend и статика

![alt text](report/image-39.png)

#### 2.3 Скрипты инициализации БД

![alt text](report/image-40.png)


### 3. Multi-stage сборка Docker-образов

#### 3.1 Dockerfile для Go приложения

![alt text](report/image-41.png)

#### 3.2 Docker-compose для локальной разработки

*Файл docker-compose.yml создан и настроен для работы трех сервисов: PostgreSQL, Go приложения и Nginx.*


### 4. GitHub Actions для автоматизации

#### 4.1 Настройка Personal Access Token

![alt text](report/image-42.png)

#### 4.2 Настройка Secrets в репозитории

![alt text](report/image-43.png)

#### 4.3 Создание GitHub Actions workflow

![alt text](report/image-44.png)

> **Важно!** Для корректной сборки в Github Actions создан файл `./nginx/Dockerfile` с командами COPY для включения файлов index.html и nginx.conf внутрь образа.

![alt text](report/image-45.png)

#### 4.4 Тестовый docker-compose для CI

![alt text](report/image-46.png)


### 5. Скриншоты работающего приложения

#### 5.1 Главная страница

![Главная страница](screenshots/main-page.png)

#### 5.2 Добавление пользователя

![Добавление пользователя](screenshots/add-user.png)

#### 5.3 Список пользователей в БД

![Данные в PostgreSQL](screenshots/postgres-data.png)


### 6. Результаты GitHub Actions

#### 6.1 Успешный запуск workflow

![GitHub Actions Success](screenshots/github-actions.png)

**Репозиторий:** [NightFury2283/containers-lab-1](https://github.com/NightFury2283/containers-lab-1)

#### 6.2 Опубликованные образы в GHCR

![GHCR Packages](screenshots/ghcr-packages.png)


### 7. Выводы

В ходе выполнения практической работы были освоены ключевые навыки работы с Docker:

**1. Базовые операции с Docker:**

**2. Работа с томами (Volumes):**

**3. Сетевые взаимодействия:**

Было разработано Go-приложение с API-эндпоинтами
Настроили Nginx как reverse proxy для статики и API
Подключили PostgreSQL с инициализацией через init-скрипты
Запустили полный стек через docker-compose

**4. CI/CD с GitHub Actions:**

**Трудности, с которыми столкнулся:**
- Изначально были проблемы с YAML-синтаксисом в workflow файлах (отступы и форматирование)
- Возникали ошибки с копированием статических файлов из-за несоответствия путей, пришлось продублировать index.html в папку static и папку nginx
- Пришлось разобраться с порядком выполнения шагов в CI/CD, а также с синтаксисом, который не понимает Dockerfile (из Github Actions)