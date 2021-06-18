FROM php:7.3-apache

# Download and configure install-php-extensions tool
ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/
RUN chmod +x /usr/local/bin/install-php-extensions && sync

# Configure Apache
COPY ./vhost.conf /etc/apache2/sites-available/000-default.conf
COPY ./php-ini-overrides.ini /usr/local/etc/php/conf.d/php-ini-overrides.ini

RUN a2enmod rewrite headers deflate expires mime
RUN a2ensite 000-default

# Install composer and php extensions (including system dependencies)
RUN install-php-extensions @composer
RUN install-php-extensions bcmath gd gmp imagick intl mcrypt mysqli pdo pdo_mysql zip

# Finish setup
RUN chown -R www-data:www-data /var/www/html
WORKDIR /var/www/html