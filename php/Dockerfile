FROM arm64v8/php:7.4.10-fpm

WORKDIR /var/www

USER root

COPY ./php/fpm /etc/php/7.4/fpm/pool.d/

RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && chmod -R 755 /usr/local/sbin \
    && chown -R www-data:www-data /var/www

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

COPY ./src /var/www

EXPOSE 9000

CMD ["php-fpm"]
