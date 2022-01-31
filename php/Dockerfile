
ARG PHP_VERSION=8.0

FROM php:8.0-fpm

# Add a non-root user to prevent files being created with root permissions on host machine.
ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}

RUN groupadd -g ${PGID} symfony && \
        useradd -l -u ${PUID} -g symfony -m symfony  && \
        usermod -p "*" symfony -s /bin/bash

RUN apt update \
    && apt install -y libpq-dev zlib1g-dev g++ git libicu-dev zip libzip-dev zip \
    && docker-php-ext-install intl opcache pdo pdo_mysql pdo_pgsql \
    && pecl install apcu \
    && docker-php-ext-enable apcu \
    && docker-php-ext-configure zip \
    && docker-php-ext-install zip

WORKDIR /var/www

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN curl -sS https://get.symfony.com/cli/installer | bash
RUN mv /root/.symfony/bin/symfony /usr/local/bin/symfony

RUN cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini

RUN git config --global user.email "jrppl.dev@example.com" \
    && git config --global user.name "jrppl"
