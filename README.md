# php-web-init
A skeleton of Docker containers for Web projects. It includes Nginx as reverse proxy, Apache, PHP, MariaDB, Composer and two cron-based workers for PHP and NodeJs scripts. Just clone and customize.

## Installation
First, install the dependecies:

* Docker
* Docker Compose

Then, clone this project, navigate to `php-web-init/containers` folder and run `docker-compose up`. Docker will build all containers and, at the end, you should be able to access the your `phpinfo()` result from `http://localhost` on `port 80`.

## Project folders
This project builds a complete environment for Web applications. In `/app` folder, you can place your PHP files, a Laravel project, for instance. By placing your `composer.json` here, Composer will check your application dependencies on every container start up.  In `/www` just place your public `index.php` and public assets. Use the `/node` folder to place your NodeJs scripts used by the NodejS cron worker.

## Running Composer
Composer runs once when the containers are started up and reads `composer.json` file in `/app` folder. To run composer again, just type in terminal, from `/containers` folder: 
```console
docker-compose start composer
```
## MariaDB Configuration
Inside `/containers/mariadb` folder, you should find three folders:
* **conf**, which contains the `my.cnf` file. Add to this file any configuration that you might want to override on your MariaDB. As an example, the default `my.cnf` sets `innodb-buffer-pool-size=2G`;
* **data**, where MariaDB will store database's data;
* **scripts**, where you can place .sh, .sql and .sql.gz files to be executed in alphabetical order when the container is started for the first time. Read [MariaDB docs at Docker Hub](https://hub.docker.com/_/mariadb) for more information.

## PHP Configuration
Navigate to `/containers/php/conf` and change any PHP settings in `overrides.ini` file. 

By default, PHP runs with the development version of `php.ini` file. To change for the production version, navigate to `/containers/php/Dockerfile` and uncomment the following line:

```console
# RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"
```

You must rebuild the container to changes in `php.ini` and `overrides.ini` take effect:
```console
docker-compose stop php
docker-compose build php
docker-compose start php
```


## Nginx Configuration
Inside `/containers/nginx/conf` you can find a `nginx.conf` file.

## Settings crontab for PHP and NodeJS
Use the `node-cron` and `php-cli-cron` containers in order to run cron tasks. Inside `/containers/node-cron/conf` and `/containers/php-cli-cron/conf` you should see a `crontab` file. This file is exactly the same as used by `crontab -e` command.
Just place your tasks in the `crontab` and execute a rebuild command to changes take effect.

Example of an crontab file (do not forget to add an empty line at the end of this file!):
```console
* * * * * echo "hello" >> /cron-test-file 2>&1
# remember to end this file with an empty new line
```

After every change in `crontab` file, you must execute a `build` command to rebuild the targeted container:
```console
docker-compose stop php-cli-cron
docker-compose build php-cli-cron
docker-compose start php-cli-cron
```