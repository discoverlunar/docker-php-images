FROM php:7.1-fpm-alpine

ARG LUNAR_HOST_NAME
ARG LUNAR_DOCUMENT_ROOT
ENV LUNAR_HOST_NAME=${LUNAR_HOST_NAME}
ENV LUNAR_DOCUMENT_ROOT=${LUNAR_DOCUMENT_ROOT}

# Update and upgrade APK repositories
RUN apk update && apk upgrade

# Install zsh
RUN apk add zsh curl wget git openrc
RUN sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
RUN echo "export PROMPT=Docker$PROMPT" >> ~/.zshrc

# Install Apache
RUN apk add apache2 apache2-proxy

# Configure Apache
COPY ./vhost.conf /etc/apache2/sites-available/000-default.conf
RUN sed -i "s|ServerName HOST_NAME|ServerName ${LUNAR_HOST_NAME}|g" /etc/apache2/sites-available/000-default.conf
RUN sed -i "s|DocumentRoot DOCUMENT_ROOT|DocumentRoot ${LUNAR_DOCUMENT_ROOT}|g" /etc/apache2/sites-available/000-default.conf
RUN sed -i "s|<Directory \"DOCUMENT_ROOT\">|<Directory \"${LUNAR_DOCUMENT_ROOT}\">|g" /etc/apache2/sites-available/000-default.conf
RUN sed -i 's|Listen 80|Listen 8080|g' /etc/apache2/httpd.conf
RUN sed -i 's|DirectoryIndex index.html|DirectoryIndex index.php index.html|g' /etc/apache2/httpd.conf
RUN sed -i 's|^DocumentRoot|<FilesMatch "\.php$">\n\
    SetHandler "proxy:unix:/var/run/php-fpm7.sock\|fcgi://localhost"\n\
</FilesMatch>\n\nDocumentRoot|g' /etc/apache2/httpd.conf
RUN sed -i 's|#LoadModule rewrite_module modules/mod_rewrite.so|LoadModule rewrite_module modules/mod_rewrite.so|g' /etc/apache2/httpd.conf
RUN sed -i 's|#LoadModule headers_module modules/mod_headers.so|LoadModule headers_module modules/mod_headers.so|g' /etc/apache2/httpd.conf
RUN sed -i 's|#LoadModule deflate_module modules/mod_deflate.so|LoadModule deflate_module modules/mod_deflate.so|g' /etc/apache2/httpd.conf
RUN sed -i 's|#LoadModule expires_module modules/mod_expires.so|LoadModule expires_module modules/mod_expires.so|g' /etc/apache2/httpd.conf
RUN sed -i 's|#LoadModule proxy_module modules/mod_proxy.so|LoadModule proxy_module modules/mod_proxy.so|g' /etc/apache2/httpd.conf
RUN sed -i 's|#LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so|LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so|g' /etc/apache2/httpd.conf
RUN sed -i 's|#LoadModule vhost_alias_module modules/mod_vhost_alias.so|LoadModule vhost_alias_module modules/mod_vhost_alias.so|g' /etc/apache2/httpd.conf
RUN sed -i 's|#LoadModule mime_module modules/mod_mime.so|LoadModule mime_module modules/mod_mime.so|g' /etc/apache2/httpd.conf
RUN mkdir -p /etc/apache2/sites-enabled && ln -sfn /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-enabled/000-default.conf
RUN rmdir /var/www/html
# Install composer
RUN apk add composer wget curl

# Install PHP extensions
RUN apk add --no-cache file imagemagick-dev imagemagick libmcrypt-dev
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Temporarily generate build dependencies
RUN apk add --no-cache --virtual .build-deps g++ make autoconf

# Install extensions
RUN pecl install imagick
RUN docker-php-ext-install pdo pdo_mysql mysqli mcrypt
RUN docker-php-ext-enable imagick

# Delete and purge temporary build dependencies
RUN apk del --purg .build-deps

# Finish setup
RUN chown -R www-data:www-data /var/www
WORKDIR /var/www