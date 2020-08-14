FROM php:5.6-apache
MAINTAINER Martin Alsinet <martin@alsinet.com.ar>

COPY php.ini /usr/local/etc/php/
RUN apt-get update
RUN apt-get install -y libpq-dev libmcrypt-dev libfreetype6-dev libjpeg62-turbo-dev
RUN a2enmod rewrite
RUN apt-get update && apt-get install -y zlib1g-dev
RUN docker-php-ext-install pgsql pdo pdo_mysql pdo_pgsql mcrypt zip
RUN docker-php-ext-install gd
RUN mkdir /tmp/build
WORKDIR /tmp/build
RUN curl -O https://xdebug.org/files/xdebug-2.5.5.tgz
RUN tar xvfz xdebug-2.5.5.tgz
WORKDIR /tmp/build/xdebug-2.5.5
RUN phpize
RUN ./configure --enable-xdebug
RUN make && make install
RUN echo "zend_extension=/usr/local/lib/php/extensions/no-debug-non-zts-20131226/xdebug.so" >> /usr/local/etc/php/conf.d/xdebug.ini
RUN echo "alias ll='ls -alhG'" >> /root/.bashrc
RUN curl https://getcomposer.org/download/1.5.2/composer.phar -o /usr/local/bin/composer
RUN chmod +x /usr/local/bin/composer
ENV SHELL=/bin/bash
ENV PATH=$PATH:/var/www/html/vendor/bin

WORKDIR /var/www/html

EXPOSE 80
CMD ["apache2-foreground"]

