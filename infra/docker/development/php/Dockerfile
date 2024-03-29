FROM php:8.1-fpm-bullseye
SHELL ["/bin/bash", "-oeux", "pipefail", "-c"]

ENV TZ='Asia/Tokyo'\
  COMPOSER_ALLOW_SUPERUSER=1 \
  COMPOSER_HOME=/composer

COPY --from=composer:2.2 /usr/bin/composer /usr/bin/composer

RUN apt-get update && \
  apt-get -y install git libicu-dev libonig-dev libzip-dev unzip locales jq && \
  apt-get clean && \
  pecl install xdebug && \
  docker-php-ext-enable xdebug && \
  rm -rf /var/lib/apt/lists/* && \
  locale-gen en_US.UTF-8 && \
  localedef -f UTF-8 -i en_US en_US.UTF-8 && \
  mkdir /var/run/php-fpm && \
  docker-php-ext-install intl pdo_mysql zip bcmath && \
  composer config -g process-timeout 3600 && \
  composer config -g repos.packagist composer https://packagist.org

COPY ./infra/docker/development/php/php-fpm.d/zzz-www.conf /usr/local/etc/php-fpm.d/zzz-www.conf
COPY ./infra/docker/development/php/php.ini /usr/local/etc/php/php.ini

WORKDIR /work/laravel
