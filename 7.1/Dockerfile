FROM php:7.1-fpm

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        libpq-dev \
        libpng-dev \
        # php-gettext \
        nano \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-openssl=/usr/include/ \
    && docker-php-ext-install \
        pdo_pgsql \
        pdo_mysql \
        pgsql \
        gettext \ 
        gd \
        exif

RUN pecl install mongodb

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

# Install dependencies
RUN apt-get update && \
    apt-get install curl nano && \
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN apt-get update && \
    apt-get install zip unzip

RUN apt-get update && apt-get install -y cron
RUN touch /var/log/cron.log

RUN apt-get update && apt-get install -y supervisor
ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf

CMD ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
