ARG PHP_IMAGE_VERSION=fpm-alpine
FROM php:${PHP_IMAGE_VERSION}

RUN apk upgrade && \
    apk update && \
    apk add --update --no-cache \
      autoconf \
      build-base \
      oniguruma-dev \
      icu-dev && \
    apk add \
      vim \
      git \
      composer && \
    pecl install \
      xdebug && \
    docker-php-ext-configure \
      intl && \
    docker-php-ext-install \
      pdo_mysql \
      mysqli \
      mbstring \
      intl && \
    docker-php-ext-enable \
      xdebug

WORKDIR /var/www/html