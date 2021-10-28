FROM node:14-buster as node
FROM php:7.4-apache-buster
SHELL ["/bin/bash", "-oeux", "pipefail", "-c"]


COPY ./php/php.ini /usr/local/etc/php
COPY ./apache/httpd.conf /etc/apache2/sites-available/000-default.conf

RUN apt-get update \
  && apt-get install -y zlib1g-dev libpq-dev mariadb-client unzip\
  && apt-get install -y libzip-dev \
  && docker-php-ext-install zip pdo_mysql mysqli \
  && docker-php-ext-enable mysqli

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
# node command
COPY --from=node /usr/local/bin /usr/local/bin
# npm command
COPY --from=node /usr/local/lib /usr/local/lib
# yarn command
COPY --from=node /opt /opt

ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

WORKDIR /var/www/html

RUN composer global require "laravel/installer"