# Docker Lab: Веб-приложение с Flask, Nginx и PostgreSQL

## Описание и архитектура:
Проект представляет собой полноценное веб-приложение для управления задачами (Task Manager) и состоит из трёх сервисов:
- Frontend: Nginx с статическим HTML и проксированием API
- Backend: Flask API для управления задачами (CRUD)
- Database: PostgreSQL для хранения данных

## Схема сервисов:
### Компоненты системы:
- Frontend (Nginx) — порт 80 (доступен извне)
- Backend (Flask) — порт 5000 (доступен только внутри сети Docker)
- Database (PostgreSQL) — порт 5432 (доступен только внутри сети Docker)

### Взаимодействие сервисов:
1. Пользователь открывает в браузере `http://localhost`
2. Nginx отдаёт статическую HTML-страницу
3. JavaScript на странице отправляет запросы к `/api/...`
4. Nginx проксирует эти запросы на `http://backend:5000/api/...`
5. Backend обрабатывает запросы и взаимодействует с PostgreSQL
6. PostgreSQL хранит данные в volume `postgres_data`

### Описание потоков данных:
- GET /api/tasks → Nginx → Backend → PostgreSQL → список задач → пользователь
- POST /api/tasks → Nginx → Backend → PostgreSQL → создание задачи → пользователь
- PATCH /api/tasks/:id → Nginx → Backend → PostgreSQL → обновление задачи
- DELETE /api/tasks/:id → Nginx → Backend → PostgreSQL → удаление задачи

### Сетевые правила:
- Только Frontend имеет открытый порт (80) для доступа извне
- Backend и PostgreSQL изолированы внутри сети Docker и не доступны снаружи
- Backend обращается к PostgreSQL по имени сервиса: `postgres:5432`
- Frontend обращается к Backend по имени сервиса: `backend:5000`

## Запуск проекта:
1. Скопируйте `.env.example` в `.env` и отредактируйте
2. Выполните: `docker compose up -d --build`
3. Откройте http://localhost в браузере

## Описание переменных окружения
Переменная		Описание		Пример
POSTGRES_DB		Название базы данных	taskdb
POSTGRES_USER		Пользователь PostgreSQL	appuser
POSTGRES_PASSWORD	Пароль PostgreSQL	SecurePass123!

## Полезный команды
docker compose up -d		Запустить все сервисы в фоне
docker compose down		Остановить и удалить контейнеры
docker compose logs -f [service]		Просмотр логов (backend/frontend/postgres)
docker compose ps		Статус контейнеров
docker compose exec [service] sh		Войти внутрь контейнера
docker compose exec postgres psql -U appuser -d taskdb		Подключиться к PostgreSQL
curl http://localhost/api/tasks			Получить список задач через API
curl -X POST http://localhost/api/tasks -H "Content-Type: application/json" -d '{"title":"Тест"}'		Создать задачу

## Ответ:
docker compose down — останавливает контейнеры, но volume сохраняется. При следующем запуске все задачи будут на месте.

docker compose down -v — останавливает контейнеры и удаляет volume. Все данные в базе будут безвозвратно потеряны, потому что удаляются файлы базы данных PostgreSQL. Volume — это специальное хранилище, которое живёт отдельно от контейнеров. Когда контейнер удаляется, volume остаётся. Флаг -v принудительно удаляет и volume тоже.
