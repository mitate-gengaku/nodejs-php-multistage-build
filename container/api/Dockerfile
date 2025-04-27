FROM php:8.4-fpm

RUN apt-get update \
    && apt-get install -y \
    build-essential \
    git \
    zip \
    unzip \
    vim \
    curl \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng-dev && \
    pecl install redis && \
    docker-php-ext-enable redis
RUN docker-php-ext-install bcmath
RUN docker-php-ext-install pdo_mysql mysqli exif
RUN cd /usr/bin && curl -s http://getcomposer.org/installer | php && ln -s /usr/bin/composer.phar /usr/bin/composer

WORKDIR /home/php/api