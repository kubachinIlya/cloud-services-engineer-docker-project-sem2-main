# Docker Project  

Контейнеризация приложения: Go-бэкенд + Vue.js фронтенд + PostgreSQL 

## Стек

- **Backend**: Go 1.21
- **Frontend**: Vue.js (сборка через Node 20, раздача через Nginx)
- **Database**: PostgreSQL 16
- **Orchestration**: Docker Compose

## Структура
.
├── backend/ # Go-приложение 

│ ├── Dockerfile 

│ └── .dockerignore 

├── frontend/ # Vue.js приложение 

│ ├── Dockerfile 

│ ├── nginx.conf 

│ └── .dockerignore 

├── .github/workflows/deploy.yaml 

├── docker-compose.yml 

├── .env.example 

└── README.md 

 
## Запуск

1. Скопируйте `.env.example` в `.env` и заполните значения:

```bash
cp .env.example .env
```
Соберите и запустите:

```bash
docker compose up --build -d
```
Приложение доступно:

Frontend: http://localhost:80

Backend API: http://localhost:8081

Остановить:

```bash
docker compose down
```
Переменные окружения

Переменная	Описание
DOCKER_USER :	Логин Docker Hub
DB_USER	: Пользователь PostgreSQL
DB_PASSWORD :	Пароль PostgreSQL
DB_NAME	: Имя базы данных

Реальные значения хранятся в .env (не коммитится). В репозитории только .env.example.

## Безопасность
Непривилегированные пользователи: контейнеры работают от appuser (backend) и nginx-unprivileged (frontend).

Multi-stage builds: инструменты сборки не попадают в финальный образ.

Минимальные базовые образы: alpine, nginx-unprivileged.

Read-only файловая система: контейнеры запускаются с read_only: true, запись — только в tmpfs.

Изоляция сетей: backend-network объявлена internal: true, БД недоступна извне.

Ограничения ресурсов: CPU и память ограничены для каждого сервиса.

Секреты: передаются через .env / env_file, не попадают в образы.

Сканирование уязвимостей: Trivy запускается в CI после сборки образов.

 
## CI/CD
Pipeline .github/workflows/deploy.yaml:

Собирает и пушит образы на Docker Hub.

Проверяет, что docker compose поднимает проект.

Сканирует образы Trivy на уязвимости (severity: CRITICAL, HIGH).
