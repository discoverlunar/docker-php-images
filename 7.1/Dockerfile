FROM php:7.1-apache

COPY ./vhost.conf /etc/apache2/sites-available/000-default.conf

WORKDIR /var/www

RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y libmagickwand-dev imagemagick libmcrypt-dev
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN pecl install imagick
RUN docker-php-ext-install pdo pdo_mysql mysqli mcrypt
RUN docker-php-ext-enable imagick

RUN chown -R www-data:www-data /var/www

RUN a2enmod rewrite
RUN a2enmod headers