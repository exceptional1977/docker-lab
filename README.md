# Docker Lab: Веб-приложение с Flask, Nginx и PostgreSQL

## Описание
Проект состоит из трёх сервисов:
- Frontend: Nginx с статическим HTML и проксированием API
- Backend: Flask API для управления задачами (CRUD)
- Database: PostgreSQL для хранения данных

## Запуск проекта
1. Скопируйте `.env.example` в `.env` и отредактируйте
2. Выполните: `docker compose up -d --build`
3. Откройте http://localhost в браузере
