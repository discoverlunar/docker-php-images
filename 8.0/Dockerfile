FROM php:8.0-apache

# Configure Apache
COPY ./vhost.conf /etc/apache2/sites-available/000-default.conf
COPY ./php-ini-overrides.ini /usr/local/etc/php/conf.d/php-ini-overrides.ini

RUN a2enmod rewrite headers deflate expires mime
RUN a2ensite 000-default

RUN apt-get update && apt-get install -y curl libmcrypt-dev libzip-dev
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install extensions
RUN apt-get update && apt-get install -y libmagickwand-dev --no-install-recommends && rm -rf /var/lib/apt/lists/*
RUN mkdir -p /usr/src/php/ext/imagick; \
        curl -fsSL https://github.com/Imagick/imagick/archive/06116aa24b76edaf6b1693198f79e6c295eda8a9.tar.gz | tar xvz -C "/usr/src/php/ext/imagick" --strip 1;
RUN pecl install mcrypt
RUN docker-php-ext-install gd pdo pdo_mysql mysqli imagick zip bcmath intl
RUN docker-php-ext-enable mcrypt

# Finish setup
RUN chown -R www-data:www-data /var/www/html
WORKDIR /var/www/html