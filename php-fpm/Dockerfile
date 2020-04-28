FROM php:7.3-fpm

RUN apt-get update && apt-get install -y \
        libzip-dev \
        zip \
        unzip \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
    && apt-get autoclean; rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) \
    	gd \
        pdo_mysql \
        bcmath \
        zip

WORKDIR /var/www/html