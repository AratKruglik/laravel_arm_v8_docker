FROM arm64v8/php:7.4.10-fpm

WORKDIR /var/www

USER root

COPY ./php/fpm /etc/php/7.4/fpm/pool.d/

RUN apt-get update && \
    docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/  &&  \
    docker-php-ext-install gd

RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && sed -i 's/max_execution_time.*/max_execution_time=600/g' /etc/php/7.4/cli/php.ini \
    && sed -i 's/max_execution_time.*/max_execution_time=600/g' /etc/php/7.4/fpm/php.ini \
    && sed -i 's/memory_limit.*/memory_limit=2048M/g' /etc/php/7.4/fpm/php.ini \
    && for file in /etc/supervisor/conf.d/*.conf; do sed -i "www-data/www-data/g" ${file}; done \
    && sed -i "s/user.*/user wwww-data;/g" /etc/nginx/nginx.conf \
    && sed -i -e "s/post_max_size = 8M/post_max_size = 15M/g" /etc/php/7.4/fpm/php.ini \
    && sed -i -e "s/upload_max_filesize = 2M/upload_max_filesize = 10M/g" /etc/php/7.4/fpm/php.ini \
    && chmod -R 755 /usr/local/sbin \
    && chown -R www-data:www-data /var/www

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

COPY ./src /var/www

EXPOSE 9000

CMD ["php-fpm"]
