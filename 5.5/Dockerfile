FROM php:5.5-fpm

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        libpq-dev \
        libpng-dev \
        php-gettext \
        nano \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install \
        pdo_pgsql \
        pdo_mysql \
        pgsql \
        gettext \ 
        gd \
        exif

RUN pecl install mongo

RUN apt-get update -qq && apt-get install -y locales -qq && dpkg-reconfigure locales \
    && sed -i 's/# ru_RU.UTF-8 UTF-8$/ru_RU.UTF-8 UTF-8/' /etc/locale.gen \
    && sed -i 's/# en_US.UTF-8 UTF-8$/en_US.UTF-8 UTF-8/' /etc/locale.gen \
    && locale-gen

# Install sSMTP for mail support
RUN apt-get update \
    && apt-get install -y -q --no-install-recommends \
        ssmtp \
    && apt-get clean \
    && rm -r /var/lib/apt/lists/*

RUN apt-get update && apt-get -y install cron
RUN touch /var/log/cron.log

CMD php-fpm && cron && tail -f /var/log/cron.log


