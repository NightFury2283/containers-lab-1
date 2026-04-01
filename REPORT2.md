# Отчет по практической работе №1
## Студент: KAI
## Группа: 16
## Дата выполнения: 30.03.2026

### 1. Подготовка к Лабораторной

## 1. Установка kubectl

![alt text](image.png)

## 2. Установка Minikube

![alt text](image-1.png)

## Добавление в PATH

![alt text](image-2.png)

## Запуск Minikube с драйвером Docker

![alt text](image-3.png)

## Настройка доступа к GHCR

![alt text](image-4.png)

## Подготовка репозитория

![alt text](image-5.png)

### Часть 2. Знакомство с kubectl и базовыми концепциями

## 2.1 Первые команды kubectl

Задание 2.1.1: Исследуйте кластер

![alt text](image-6.png)

![alt text](image-7.png)

![alt text](image-8.png)

Самостоятельное задание

![alt text](image-9.png)

NAME       STATUS   ROLES           AGE   VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE                         KERNEL-VERSION                       CONTAINER-RUNTIME
minikube   Ready    control-plane   86m   v1.35.1   192.168.49.2   <none>        Debian GNU/Linux 12 (bookworm)   5.15.167.4-microsoft-standard-WSL2   docker://29.2.1

2.2 Работа с подами (Pods)

![alt text](image-10.png)

Logs
![alt text](image-11.png)

![alt text](image-12.png)

![alt text](image-13.png)

Самостоятельное задание
![alt text](image-14.png)

2026/03/31 20:14:24 Error creating table:dial tcp [::1]:5432: connect: connection refused

## 2.3 Работа с ReplicaSet

![alt text](image-15.png)

![alt text](image-16.png)

![alt text](image-17.png)

## 2.4 Работа с Deployment

![alt text](image-18.png)

![alt text](image-19.png)

![alt text](image-20.png)

### Самостоятельное задание:

![alt text](image-21.png)

![alt text](image-22.png)

### 2.5 Работа с Service

![alt text](image-23.png)

![alt text](image-24.png)

![alt text](image-25.png)

## 2.6 Полный стек приложения

![alt text](image-26.png)

### Часть 3. Валидация и отладка манифестов
## 3.1 Валидация YAML

![alt text](image-27.png)

![alt text](image-28.png)

![alt text](image-29.png)

## 3.2 Отладка приложений

![alt text](image-30.png)

![alt text](image-31.png)

![alt text](image-32.png)

## 3.3 Мониторинг через Dashboard

![alt text](image-33.png)

### Часть 4. GitHub Actions для проверки манифестов

![alt text](image-34.png)

### Часть 5. Финальный запуск и проверка

![alt text](image-35.png)

![alt text](image-36.png)

