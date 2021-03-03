# Docker PHP-FPM 8.0 & Nginx 1.18 on Alpine Linux
Example PHP-FPM 8.0 & Nginx 1.18 setup for Docker, build on [Alpine Linux](https://www.alpinelinux.org/).
The image is only +/- 35MB large.

Based on: https://github.com/TrafeX/docker-php-nginx


* Built on the lightweight and secure Alpine Linux distribution
* Very small Docker image size (+/-35MB)
* Uses PHP 7.4 for better performance, lower CPU usage & memory footprint
* Optimized for 100 concurrent users
* Optimized to only use resources when there's traffic (by using PHP-FPM's on-demand PM)
* The servers Nginx, PHP-FPM and supervisord run under a non-privileged user (nobody) to make it more secure
* The logs of all the services are redirected to the output of the Docker container (visible with `docker logs -f <container name>`)
* Follows the KISS principle (Keep It Simple, Stupid) to make it easy to understand and adjust the image to your needs

## Usage

Start the Docker container:

    docker run -p 80:8080 verictas/alpine-nginx-php8

See the PHP info on http://localhost, or the static html page on http://localhost/test.html

Put your own code into /var/www/html.

You can mount /etc/nginx/conf.d/server.conf, /etc/php8/conf.d/settings.ini and /etc/php8/php-fpm.d/server.conf.

### Building this container

Build with `docker build . -t verictas/alpine-nginx-php8`.


### Building with composer

```Dockerfile
FROM composer AS composer

# copying the source directory and install the dependencies with composer
COPY <your_directory>/ /app

# run composer install to install the dependencies
RUN composer install \
  --optimize-autoloader \
  --no-interaction \
  --no-progress

# continue stage build with the desired image and copy the source including the
# dependencies downloaded by composer
FROM verictas/alpine-nginx-php8
COPY --chown=nginx --from=composer /app /var/www/html
```
