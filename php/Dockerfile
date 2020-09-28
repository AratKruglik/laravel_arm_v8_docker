FROM arm64v8/php:7.4.10-fpm

WORKDIR /var/www

RUN apt-get update && apt-get install -y \
    build-essential \
    libzip-dev \
    libpng-dev \
    libjpeg62-turbo-dev \
    unzip \
    curl

RUN docker-php-ext-install pdo_mysql mbstring zip sodium
RUN docker-php-ext-configure pd --with-freetype-dir=/usr/include/ --with-webp-dir=/usr/include/ --with-jpeg-dir=/usr/include/

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

COPY ./src /var/www

USER www-data

EXPOSE 9000
CMD ["php-fpm"]
