FROM php:7.4-apache

# Configure Apache
COPY ./vhost.conf /etc/apache2/sites-available/000-default.conf
RUN a2enmod rewrite headers deflate expires mime
RUN a2ensite 000-default

RUN apt-get update && apt-get install -y curl libmcrypt-dev imagemagick libmagickwand-dev
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install extensions
RUN pecl install imagick mcrypt
RUN docker-php-ext-install gd pdo pdo_mysql mysqli
RUN docker-php-ext-enable imagick mcrypt

# Finish setup
RUN chown -R www-data:www-data /var/www/html
WORKDIR /var/www/html