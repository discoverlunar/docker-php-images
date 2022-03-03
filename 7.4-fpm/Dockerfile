FROM php:7.4-fpm-buster

# Download and configure install-php-extensions tool
ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/
RUN chmod +x /usr/local/bin/install-php-extensions && sync

# Install and configure Apache
RUN apt update -y && apt install apache2 -y && rm -rf /var/lib/apt/lists/*
COPY ./vhost.conf /etc/apache2/sites-available/000-default.conf
COPY ./php-ini-overrides.ini /usr/local/etc/php/conf.d/php-ini-overrides.ini
COPY ./start.sh start.sh
RUN a2enmod rewrite headers deflate expires mime proxy proxy_fcgi && a2ensite 000-default

# Install composer and php extensions (including system dependencies)
RUN install-php-extensions @composer && install-php-extensions bcmath gd gmp imagick intl mcrypt mysqli pdo pdo_mysql soap sockets zip

# Finish setup
EXPOSE 80
RUN chown -R www-data:www-data /var/www/html
WORKDIR /var/www/html

# Start Apache
CMD ["/bin/bash", "./start.sh"]