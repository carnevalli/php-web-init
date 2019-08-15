FROM php:7.2.21-cli-alpine3.10
COPY ./conf/overrides.ini $PHP_INI_DIR/conf.d/
COPY ./conf/crontab /etc/crontabs/root
RUN apk update && apk add libpng-dev nano
RUN docker-php-ext-install pdo_mysql mysqli gd exif
CMD ["crond", "-f", "-d", "8"]