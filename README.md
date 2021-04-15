# Lunar's PHP-FPM images
## Introduction
Below you will find the Docker images Lunar uses to power local development using Apache and PHP-FPM. 
To use a specific version when setting up your docker-compose.yml, append the version number when specifying 
the image name.

Currently the following versions are available (including legacy PHP versions):
- PHP 5.6
- PHP 7.0
- PHP 7.1
- PHP 7.2
- PHP 7.3
- PHP 7.4
- PHP 8.0

## Usage
To use one of our images in your setup, add the following to your docker-compose.yml:
```
httpd:
  container_name: <your_container_name>
  image: lunarbe/php:7.1
  environment:
    - VIRTUAL_PORT=80
    - VIRTUAL_HOST=<your_hostname>
```
