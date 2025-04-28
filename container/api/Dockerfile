###################################
#
#   開発環境
#
###################################

FROM php:8.4-fpm AS dev

WORKDIR /home/php/api

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

COPY ./container/api/php.ini /usr/local/etc/php/php.ini

# 非ルートユーザーに切り替え
RUN useradd -ms /bin/bash php && \
    echo "php:php" | chpasswd

USER php

###################################
#
#   本番環境
#
###################################

FROM php:8.4-fpm-alpine AS production

WORKDIR /home/php/api

COPY --chown=php:php ./api /home/php/api

# COPY --from=dev --chown=php:php composer*.json ./
COPY --from=dev /usr/bin/composer /usr/bin/composer
COPY --from=dev /usr/local/etc/php/php.ini /usr/local/etc/php/php.ini
RUN composer install --no-scripts --no-autoloader

# 非ルートユーザーに切り替え
RUN addgroup -S -g "1000" "php" && \
    adduser -u "1000" -G "php" -D "php"

USER php

VOLUME ["/var/run/php-fpm"]

CMD ["php-fpm", "-R"]