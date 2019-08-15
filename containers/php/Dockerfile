
FROM php:7.2.21-apache
RUN a2enmod rewrite
# RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"
COPY ./conf/overrides.ini $PHP_INI_DIR/conf.d/
RUN apt-get update -y && apt-get install -y libpng-dev nano
RUN docker-php-ext-install pdo_mysql mysqli gd exif