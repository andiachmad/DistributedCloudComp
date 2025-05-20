FROM composer:2.7 AS builder
WORKDIR /app
COPY . /app
RUN composer install --no-dev --optimize-autoloader

FROM php:8.2-fpm-alpine

RUN apk add --no-cache nginx && \
    docker-php-ext-install pdo pdo_mysql

COPY nginx.conf /etc/nginx/nginx.conf

COPY . /var/www/html
COPY --from=builder /app/vendor /var/www/html/vendor

RUN chown -R www-data:www-data /var/www/html && \
    chmod -R 775 /var/www/html/storage /var/www/html/bootstrap/cache

COPY start.sh /start.sh
RUN chmod +x /start.sh

EXPOSE 80

CMD ["/start.sh"]
