FROM composer:2.0.11 AS composer
COPY ./composer.json ./composer.lock /app/
WORKDIR /app
RUN composer install -vvv --no-dev --ignore-platform-reqs --no-scripts
COPY ./ /app
RUN composer dump-autoload && php artisan route:cache \
    && php artisan view:cache \
    && chgrp -R www-data bootstrap/cache storage/  \
    && chmod -R 770 bootstrap/cache storage/ 

FROM jakmall/recruitment/cloud-engineer/counter/php-fpm AS php-fpm
WORKDIR /var/www/html
COPY --from=composer /app /var/www/html
USER www-data:www-data
ENTRYPOINT [ "/usr/local/bin/php", "/var/www/html/artisan", "config:cache" ]

FROM jakmall/recruitment/cloud-engineer/counter/php-cli AS worker
WORKDIR /var/www/html
COPY --from=composer /app /var/www/html
USER www-data:www-data
ENTRYPOINT [ "/usr/local/bin/php", "/var/www/html/artisan", "config:cache" ]