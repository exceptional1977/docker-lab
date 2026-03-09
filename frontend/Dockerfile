# Базовый образ
FROM nginx:1.27-alpine

# Копируем конфигурацию Nginx
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Копируем HTML файлы
COPY html /usr/share/nginx/html

# Открываем порт 80
EXPOSE 80
