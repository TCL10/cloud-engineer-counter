FROM php:8.0.3-fpm-alpine3.13 as php-fpm
WORKDIR /var/www/html
RUN rm ${PHP_INI_DIR}/php.ini-development \
    && mv ${PHP_INI_DIR}/php.ini-production ${PHP_INI_DIR}/php.ini \
    && apk add --no-cache --virtual buildDeps autoconf g++ make \
    && pecl install redis && docker-php-ext-enable redis \
    && docker-php-ext-install bcmath pdo_mysql opcache && docker-php-ext-enable bcmath pdo_mysql opcache \ 
    && apk del buildDeps


FROM php:8.0.3-cli-alpine3.13 as worker
WORKDIR /var/www/html
RUN rm ${PHP_INI_DIR}/php.ini-development \
    && mv ${PHP_INI_DIR}/php.ini-production ${PHP_INI_DIR}/php.ini \
    && apk add --no-cache --virtual buildDeps autoconf g++ make \
    && pecl install redis && docker-php-ext-enable redis \
    && docker-php-ext-install bcmath pdo_mysql && docker-php-ext-enable bcmath pdo_mysql \
    && apk del buildDeps