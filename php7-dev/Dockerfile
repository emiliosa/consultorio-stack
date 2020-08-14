FROM php:7.2-apache

# enviroment
ENV SHELL=/bin/bash
ENV PATH=$PATH:/var/www/html/vendor/bin

# alias
RUN echo "alias ll='ls -alhG'" >> /root/.bashrc

RUN apt-get update &&\
        apt-get install -y \
        libpq-dev \
        libmcrypt-dev \
        libreadline-dev \
        zlib1g-dev \
        iputils-ping \
        unzip \
        git \
        libcurl4-openssl-dev \
        pkg-config \
        libssl-dev \
        libpng-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libgd-dev \
        libwebp-dev \
        libxpm-dev \
        sqlite


# configure GD
RUN docker-php-ext-configure gd \
        --with-gd \
        --with-webp-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ \
        --with-png-dir=/usr/include/ \
        --with-freetype-dir=/usr/include/ \
        --with-zlib-dir \
        --with-xpm-dir

# php extensions
RUN docker-php-ext-install \
        gd \
        pgsql \
        pdo \
        pdo_mysql \
        pdo_pgsql \
        pcntl \
        sysvshm \
        sysvsem \
        sysvmsg \
        zip

# install redis and xdebug
RUN pecl install \
        redis \
        xdebug

# enable redis and xdebug
RUN docker-php-ext-enable \
        redis \
        xdebug

# PHPUnit
RUN cd /tmp &&\
        curl -LO https://phar.phpunit.de/phpunit.phar > phpunit.phar &&\
        chmod +x phpunit.phar &&\
        mv /tmp/phpunit.phar /usr/local/bin/phpunit

# composer
#COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
RUN cd /tmp &&\
        curl -sS https://getcomposer.org/installer | php &&\
        mv /tmp/composer.phar /usr/local/bin/composer

# Composer speedup
RUN composer global require hirak/prestissimo

RUN a2enmod rewrite

WORKDIR /var/www/html

EXPOSE 80
EXPOSE 8000

CMD ["apache2-foreground"]
