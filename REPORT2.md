# Отчет по практической работе №2
## Студент: KAI
## Группа: 16
## Дата выполнения: 30.03.2026

### 1. Подготовка к Лабораторной

#### 1.1 Установка kubectl
![Установка kubectl](screenshots2/image.png)

#### 1.2 Установка Minikube
![Установка Minikube](screenshots2/image-1.png)

#### 1.3 Добавление в PATH
![Добавление в PATH](screenshots2/image-2.png)

#### 1.4 Запуск Minikube с драйвером Docker
![Запуск Minikube](screenshots2/image-3.png)
```bash
minikube status
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

#### 1.5 Настройка доступа к GHCR
![Настройка доступа к GHCR](screenshots2/image-4.png)

#### 1.6 Подготовка репозитория
![Подготовка репозитория](screenshots2/image-5.png)

---

### Часть 2. Знакомство с kubectl и базовыми концепциями

#### 2.1 Первые команды kubectl
Задание 2.1.1: Исследуйте кластер
![Первые команды kubectl 1](screenshots2/image-6.png)
![Первые команды kubectl 2](screenshots2/image-7.png)
![Первые команды kubectl 3](screenshots2/image-8.png)

Самостоятельное задание (nodes.txt):
![nodes.txt](screenshots2/image-9.png)
```text
NAME       STATUS   ROLES           AGE   VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE                         KERNEL-VERSION                       CONTAINER-RUNTIME
minikube   Ready    control-plane   22h   v1.35.1   192.168.49.2   <none>        Debian GNU/Linux 12 (bookworm)   5.15.167.4-microsoft-standard-WSL2   docker://29.2.1
```

#### 2.2 Работа с подами (Pods)
![Работа с подами](screenshots2/image-10.png)
![Логи пода](screenshots2/image-11.png)
![Описание пода](screenshots2/image-12.png)
![Выполнение команд в поде](screenshots2/image-13.png)

Самостоятельное задание (Go-приложение из ЛР1):
![crash-logs.txt](screenshots2/image-14.png)
```text
2026/03/31 20:14:24 Error creating table:dial tcp [::1]:5432: connect: connection refused
```

#### 2.3 Работа с ReplicaSet
![Создание ReplicaSet](screenshots2/image-15.png)
![Масштабирование ReplicaSet](screenshots2/image-16.png)
![Самовосстановление подов](screenshots2/image-17.png)

#### 2.4 Работа с Deployment
![Применение Deployment](screenshots2/image-18.png)
![Просмотр ReplicaSet](screenshots2/image-19.png)
![Просмотр Подов](screenshots2/image-20.png)

Самостоятельное задание (PostgreSQL Deployment):
![PostgreSQL Deployment 1](screenshots2/image-21.png)
![PostgreSQL Deployment 2](screenshots2/image-22.png)

#### 2.5 Работа с Service
![Создание Сервисов](screenshots2/image-23.png)
![Просмотр Сервисов](screenshots2/image-24.png)
![Описание Сервиса](screenshots2/image-25.png)

#### 2.6 Полный стек приложения
![Применение всего стека](screenshots2/image-26.png)

---

### Часть 3. Валидация и отладка манифестов

#### 3.1 Валидация YAML
![Сухая проверка dry-run](screenshots2/image-27.png)
![kubectl explain](screenshots2/image-28.png)
![kubeval валидация](screenshots2/image-29.png)

#### 3.2 Отладка приложений
![Просмотр событий](screenshots2/image-30.png)
![Логи подов](screenshots2/image-31.png)
![Интерактивная отладка netshoot](screenshots2/image-32.png)

#### 3.3 Мониторинг через Dashboard
![Kubernetes Dashboard](screenshots2/image-33.png)

---

### Часть 4. GitHub Actions для проверки манифестов
![GitHub Actions Workflow](screenshots2/image-34.png)

---

### Часть 5. Финальный запуск и проверка

#### 5.1 Главная страница приложения
![Главная страница](screenshots2/image-35.png)

#### 5.2 Результат GET /api/users
![API Response](screenshots2/image-36.png)

#### 5.3 GitHub Actions CI (Зеленая галочка)
![GitHub Actions Success](screenshots2/image-37.png)

---

### Часть 6. Ответы на контрольные вопросы

1. **В чем разница между Pod и Deployment?**
   Pod — это минимальная единица развертывания в K8s (один или несколько контейнеров). Deployment — это контроллер более высокого уровня, который управляет ReplicaSet и обеспечивает стратегии обновления (RollingUpdate), отката и автоматического масштабирования.

2. **Для чего нужен Service типа ClusterIP?**
   Service типа ClusterIP предоставляет стабильный внутренний IP-адрес и DNS-имя группе подов внутри кластера. Он позволяет компонентам системы (например, бэкенду обращаться к БД) находить друг друга по имени сервиса, даже если поды пересоздаются и меняют свои внутренние IP.

3. **Как ReplicaSet обеспечивает самовосстановление?**
   ReplicaSet постоянно отслеживает текущее количество работающих подов и сравнивает его с заданным значением `replicas`. Если под падает или удаляется, ReplicaSet мгновенно создает новый на основе шаблона `PodTemplate`, чтобы поддерживать желаемое состояние.

4. **Что произойдет с приложением, если удалить под PostgreSQL?**
   Deployment автоматически перезапустит под PostgreSQL. Однако, так как в данной работе мы использовали `emptyDir` (временное хранилище), все данные в базе данных будут потеряны. Для сохранения данных в реальных проектах используются `PersistentVolumes`.

---

### Часть 7. Выводы
В ходе практической работы был успешно развернут локальный кластер Kubernetes с использованием Minikube. Освоены основные концепции оркестрации: управление жизненным циклом приложений через Deployment, балансировка нагрузки через Service и хранение конфигураций в ConfigMap. Была настроена автоматизированная валидация манифестов в GitHub Actions, что гарантирует корректность YAML-файлов перед деплоем. Самой интересной частью было решение проблем со статус-пробами (Readiness/Liveness probes) и настройка сетевого взаимодействия между Go-приложением и базой данных PostgreSQL.
