FROM node:14-buster as node
FROM php:7.4-apache-buster
SHELL ["/bin/bash", "-oeux", "pipefail", "-c"]


COPY ./php/php.ini /usr/local/etc/php
COPY ./apache/httpd.conf /etc/apache2/sites-available/000-default.conf

COPY --from=composer:2.0 /usr/bin/composer /usr/bin/composer
# node command
COPY --from=node /usr/local/bin /usr/local/bin
# npm command
COPY --from=node /usr/local/lib /usr/local/lib
# yarn command
COPY --from=node /opt /opt

RUN apt-get update && \
  apt-get -y install git libicu-dev libonig-dev libzip-dev unzip locales && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/* && \
  locale-gen en_US.UTF-8 && \
  localedef -f UTF-8 -i en_US en_US.UTF-8 && \
  a2enmod rewrite && \
  docker-php-ext-install intl pdo_mysql zip bcmath && \
  composer config -g process-timeout 3600 && \
  composer config -g repos.packagist composer https://packagist.org \
  composer update



ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

WORKDIR /var/www/html

RUN composer global require "laravel/installer"