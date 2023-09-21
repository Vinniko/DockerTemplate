FROM php:7.4-fpm
USER root

ENV TZ=Europe/Moscow
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt -y update
RUN apt -y --no-install-recommends install unzip wget curl libaio1 openssl locales locales-all
RUN apt -y --no-install-recommends install telnet net-tools iputils-ping vim
RUN apt -y --no-install-recommends install \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        zlib1g-dev \
        libicu-dev \
        g++ \
        zip \
        unixodbc-dev \
        libxml2-dev \
        libaio-dev \
        freetds-dev \
        libssl-dev \
        procps \
        libzip-dev \
        libldap2-dev \
        unixodbc \
        unixodbc-dev
RUN apt -y --no-install-recommends install mc

RUN sed -i 's/# \(en_US\.UTF-8 .*\)/\1/' /etc/locale.gen && \
    locale-gen
ENV LANG="en_US.UTF-8"
ENV LANGUAGE="en_US:en"
ENV LC_ALL="en_US.UTF-8"

# PHP EXTENSIONS
RUN docker-php-ext-install mysqli pdo pdo_mysql && docker-php-ext-enable pdo_mysql # Заменить на другую базу данных при необходимости
RUN docker-php-ext-install gd

RUN mkdir -p /tmp/redis
COPY redis-5.3.7.tgz /tmp/redis/redis-5.3.7.tgz
RUN cd /tmp/redis/ && tar -xvf redis-5.3.7.tgz && cd redis-5.3.7 && phpize && ./configure && make install && touch /usr/local/etc/php/conf.d/20-redis.ini && echo 'extension=redis' > /usr/local/etc/php/conf.d/20-redis.ini

RUN docker-php-ext-enable redis

RUN mkdir -p /tmp/xdebug
COPY xdebug-3.1.4.tgz /tmp/xdebug/xdebug-3.1.4.tgz
RUN cd /tmp/xdebug/ && tar -xvf xdebug-3.1.4.tgz && cd xdebug-3.1.4 && phpize && ./configure && make install && touch /usr/local/etc/php/conf.d/30-xdebug.ini && echo 'zend_extension=xdebug' > /usr/local/etc/php/conf.d/30-xdebug.ini

RUN docker-php-ext-enable xdebug

COPY ./php.ini /usr/local/etc/php/php.ini
COPY ./php-fpm.conf /usr/local/etc/php-fpm.d/php-fpm.conf

WORKDIR /app

RUN curl -sS https://getcomposer.org/installer -o composer-setup.php
RUN php composer-setup.php --install-dir=/usr/local/bin --filename=composer
RUN rm ./composer-setup.php

CMD ["php-fpm"]

EXPOSE 9000
