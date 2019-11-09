FROM ubuntu:bionic

# Fixes some weird terminal issues such as broken clear / CTRL+L
ENV TERM=linux

# Install Ondrej repos for Ubuntu Bionic, PHP7.3, composer and selected extensions - better selection than
# the distro's packages
RUN apt-get update \
    && apt-get install -y --no-install-recommends gnupg ca-certificates curl \
    && echo "deb http://ppa.launchpad.net/ondrej/php/ubuntu bionic main" > /etc/apt/sources.list.d/ondrej-php.list \
    && echo "deb https://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list \
    && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg \
    && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 4F4EA0AAE5267A6C \
    && apt-get update \
    && apt-get -y --no-install-recommends install \
        git \
        unzip \
        php-apcu \
        php-apcu-bc \
        php7.3-cli \
        php7.3-curl \
        php7.3-json \
        php7.3-mbstring \
        php7.3-opcache \
        php7.3-readline \
        php7.3-xml \
        php7.3-zip \
        php7.3-mysql \
        php7.3-fpm \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && composer global require hirak/prestissimo \
    && composer clear-cache \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* ~/.composer

RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -
RUN apt-get install -y \
    nodejs \
    yarn

CMD ["php", "-a"]

# PHP-FPM packages need a nudge to make them docker-friendly
COPY overrides.conf /etc/php/7.3/fpm/pool.d/z-overrides.conf

# PHP-FPM has really dirty logs, certainly not good for dockerising
# The following startup script contains some magic to clean these up
COPY php-fpm-startup /usr/bin/php-fpm
CMD /usr/bin/php-fpm

WORKDIR "/application"

ARG DEBIAN_FRONTEND=noninteractive

EXPOSE 9000
