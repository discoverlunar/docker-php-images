# Lunar's PHP-Apache images
___
## Introduction
Below you will find the Docker images Lunar uses to power local development using PHP and Apache, based on the official
PHP images on Docker hub. To use a specific version when setting up your docker-compose.yml, append the version number 
when specifying the image name.

## Available tags
Currently the following tags are available (latest being equal to 8.1):
- `lunarbe/php:5.6`
- `lunarbe/php:7.0`
- `lunarbe/php:7.1`
- `lunarbe/php:7.2`
- `lunarbe/php:7.3`
- `lunarbe/php:7.4`
- `lunarbe/php:8.0`
- `lunarbe/php:8.1`
- `lunarbe/php:latest`

In the nearby future, we will start offering PHP-FPM images as well, for now all of the above are based on mod-php in Apache.

Each image has the core extensions available and/or enabled, and additionally has the following non-core extensions enabled:
- `bcmath` 
- `gd` 
- `gmp` 
- `imagick` 
- `intl` 
- `mcrypt` 
- `mysqli` 
- `pdo` 
- `pdo_mysql`
- `soap`
- `sockets`
- `zip`

Additionally, composer will be installed for you as well.

## Usage
At Lunar, we use these images in conjunction with [DNSmasq](https://thekelleys.org.uk/dnsmasq/doc.html) and [nginx-proxy](https://github.com/nginx-proxy/nginx-proxy) to
resolve our local domain names to their respective containers using the VIRTUAL_HOST environment variable. However, feel
free to use these images in your own stacks, as they will work just as well without the aforementioned setup.

For example, to use one of our images in your setup, add the following to your docker-compose.yml:
```
services:
  httpd:
    container_name: <your_container_name>
    image: lunarbe/php:7.1
```
Apache will be listening to port 80 and will not bind to any of the host ports. Make sure you set up your port 
bindings accordingly, or access the container via its IP, e.g. `172.0.10.45:80`.