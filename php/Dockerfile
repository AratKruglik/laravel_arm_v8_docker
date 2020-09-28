FROM arm64v8/php:7.4.10-fpm

WORKDIR /var/www

USER root

RUN apt-get update && apt-get install -y \
    build-essential \
    libzip-dev \
    libpng-dev \
    libjpeg62-turbo-dev \
    unzip \
    curl

RUN apt-get update && \
    pecl channel-update pecl.php.net && \
    pecl install apcu igbinary mongodb && \
    # compile Redis with igbinary support
    pecl bundle redis && cd redis && phpize && ./configure --enable-redis-igbinary && make && make install && \
    docker-php-ext-install bcmath sockets && \
    docker-php-ext-enable apcu igbinary mongodb opcache redis && \
    docker-php-source delete && \
    rm -r /tmp/* /var/cache/* /var/www/*

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

COPY ../src /var/www

EXPOSE 9000
CMD ["php-fpm"]
