ARG phpversion

FROM php:$phpversion-apache

ENV APACHE_DOCUMENT_ROOT /var/www/html/public

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

RUN apt-get update && apt-get install -y git zip
RUN a2enmod rewrite

RUN docker-php-ext-install pdo pdo_mysql

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

COPY ./docker/prod/000-default.conf /etc/apache2/sites-enabled/000-default.conf
COPY ./bin /var/www/html/bin
COPY ./config /var/www/html/config
COPY ./public /var/www/html/public
COPY ./src /var/www/html/src
COPY ./templates /var/www/html/templates
COPY ./composer.json /var/www/html/composer.json
COPY ./composer.lock /var/www/html/composer.lock

RUN mkdir -p /var/www/html/var/cache /var/www/html/var/log
RUN chown -R www-data:www-data /var/www/html

WORKDIR /var/www/html

RUN composer install --no-dev
