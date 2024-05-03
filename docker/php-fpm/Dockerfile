FROM php:8.2-fpm

ARG WWW_DATA_UID
ARG WWW_DATA_GID

RUN test $WWW_DATA_GID && groupmod -g $WWW_DATA_GID www-data && test $WWW_DATA_UID && usermod -u $WWW_DATA_UID www-data || echo XXX

RUN apt-get update \
  && apt-get install -y \
    wget git unzip \
    libzip-dev libbz2-dev libicu-dev libmcrypt-dev libxml2-dev libaio1 libpng-dev \
  && docker-php-ext-install calendar dba pcntl shmop sockets sysvmsg sysvsem sysvshm zip bz2 intl gd bcmath \
  && docker-php-source extract \
  && cd /usr/src/php/ext/ \
  && pecl bundle redis \
  && docker-php-ext-configure redis --disable-redis-igbinary --enable-redis-lzf \
  && docker-php-ext-install redis \
  && docker-php-source delete

RUN apt-get update && \
    apt-get install -y libpq-dev && \
    docker-php-ext-install pdo pdo_pgsql pgsql

RUN curl -sS https://getcomposer.org/installer | php -- \
        --filename=composer \
        --install-dir=/usr/local/bin && \
        echo "alias composer='composer'" >> /root/.bashrc

ADD www.conf /usr/local/etc/php-fpm.d/www.conf
