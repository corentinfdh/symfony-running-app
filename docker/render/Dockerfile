FROM php:8.4-cli

RUN apt-get update && apt-get install -y \
    git unzip zip libpq-dev libicu-dev libzip-dev \
    && docker-php-ext-install pdo pdo_pgsql intl zip opcache

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN curl -sS https://get.symfony.com/cli/installer | bash \
    && mv /root/.symfony*/bin/symfony /usr/local/bin/symfony \
    && chmod +x /usr/local/bin/symfony

WORKDIR /var/www/html
COPY . .

RUN mv _composer.json composer.json && mv _composer.lock composer.lock

RUN export PATH="$PATH:/usr/local/bin" && composer install --no-dev --optimize-autoloader
RUN chmod -R 777 var

EXPOSE 10000

CMD ["php", "-S", "0.0.0.0:10000", "-t", "public"]
