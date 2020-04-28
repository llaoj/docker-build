FROM php:7.4-fpm

RUN sed -ri 's/(deb|security).debian.org/mirrors.163.com/g' /etc/apt/sources.list && \
    apt-get update && apt-get install -y \
        git \
        libzip-dev \
        zip \
        unzip \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        nginx \
        supervisor \
    && apt-get autoclean; rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install \
        pdo_mysql \
        bcmath \
        zip

RUN pecl install redis-5.2.1 \
    && pecl install grpc-1.28.0 \
    && pecl install protobuf-3.11.4 \
    && docker-php-ext-enable redis grpc protobuf

RUN curl -sS https://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/ \
    && composer global require hirak/prestissimo

COPY ./conf/supervisor/ /etc/supervisor/conf.d/

WORKDIR /var/www/html

CMD ["supervisord","-n"]