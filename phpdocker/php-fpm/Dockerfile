FROM phpdockerio/php73-fpm:latest
WORKDIR "/application"
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
    && apt-get -y --no-install-recommends install  php7.3-mysql php7.3-pgsql php7.3-bz2 php7.3-gd php-mongodb php7.3-phpdbg php7.3-sybase php7.3-xmlrpc php7.3-xsl php-yaml \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*
RUN apt-get update \
    && apt-get -y install git \
    && apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*
FROM node:stretch-slim
RUN npm i @symfony/webpack-encore 