FROM php:7-fpm-alpine

RUN apk add --update \
        git \
        autoconf \
        g++ \
        libtool \
        make \
        wget \
        zlib-dev \
        libpng-dev \
        libzip-dev \
        libmemcached-dev \
        icu-dev \
        libxml2-dev \
    && git clone https://github.com/php-memcached-dev/php-memcached.git /usr/src/php/ext/memcached/ \
    && docker-php-ext-configure memcached \
    && docker-php-ext-install -j$(nproc) memcached \
    && pecl install memcache \
    && docker-php-ext-enable memcache \
    && docker-php-ext-install -j$(nproc) pcntl \
    && pecl install redis \
    && docker-php-ext-enable redis \
    && docker-php-ext-install -j$(nproc) bcmath \
    && docker-php-ext-install -j$(nproc) pdo_mysql \
    && docker-php-ext-install -j$(nproc) opcache \
    && docker-php-ext-install -j$(nproc) intl \
    && apk add --update postgresql-dev \
    && docker-php-ext-install -j$(nproc) pgsql \
    && apk del \
        postgresql-libs \
        libsasl \
        db \
    && docker-php-ext-install -j$(nproc) sockets \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install -j$(nproc) zip \
    && docker-php-ext-install -j$(nproc) soap \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug

ADD ./php.ini /usr/local/etc/php/php.ini

RUN wget https://getcomposer.org/installer -O - -q \
    | php -- --install-dir=/bin --filename=composer

RUN composer global require hirak/prestissimo
RUN composer global require phpunit/phpunit

WORKDIR /var/www/src
