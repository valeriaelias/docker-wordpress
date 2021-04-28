# WordPress Local Development Environment with Docker & Docker Compose

Install docker https://docs.docker.com/get-docker/

Instructions on how to set up a WP local dev enviroment using this repository files:

1. Create your project folder
2. Download the files or clone the repo to your project folder
3. Replace all the containers' names, and database credentials in the docker-compose.yml file
4. Use your terminal to `cd` into your project folder and run the command `docker-compose up -d`
5. Open the Docker Desktop UI to check all 4 containers are mounted and running
6. To access the WordPress admin got to: http://localhost:8000/wp-admin
7. To access the website go to: http://localhost:8000
8. To access the PHPMyAdmin go to: http://localhost:8080

## Containers/Services

The docker-compose.yml file will mount 4 containers/services:

- DB (the database)
- PHP (includes PHP 7.3 and WordPress)
- NGinx (the server)
- PHPMyAdmin (free software to manage the database)

### DB Container

* Uses the `mariadb` docker image to install MySQL in the container.
* Set up MySQL database name, root password, and user. 
* Uses port 3306 in both docker host (your physical computer system) and inside the container. 
* Uses shared volume: 
  - `/wp-data` in the docker host (inside your project folder structure) shares files with `/var/lib/mysql` inside the container. That means that all mysql files created inside the container can be found in your project `wp-data` folder.

### PHP Container

* Uses the `wordpress:php7.3-fpm-alpine` docker image to install php and WordPress inside the container.
* Depends on the MariaDB container. It will only be mounted when the DB container is ready.
* Connects WordPress to MySQL using the same credentials configured in the MariaDB container. 
* Links to the DB container.
* Uses shared volume: 
  - `/wp-app` in the docker host shares files with `/var/www/html` inside the container. All the WordPress files created inside the container can be found in your project `wp-app` folder. And you will use this folder to create/update WP themes, plugins and templates.
  - `/server-conf/php` in the docker host shares files with `/usr/local/etc/php/conf.d/` inside the container. When you donwload/clone this repo it already comes with `/server-conf/php/my.ini` and we are now telling docker-compose to use this existing file as an extra config for php inside the container. It will mostly raise the php max_upload_size limits.

### NGinx Container

* Uses the `nginx` docker image to install nginx into the container.
* Links to the WordPress container.
* Uses port 8000 in the docker host and port 80 inside the container. That means that from your computer/browser you have to access http://localhost:8000 to visit the WordPress website.
* Uses shared volume: 
  - `/wp-app` in the docker host shares files with `/var/www/html` inside the container. 
  - `/logs/nginx` in the docker host shares files with `/var/log/nginx` inside the container. Then, the server logs generated at `/var/log/nginx` can automatically be found in your project `/logs/nginx`.
  - `/server-conf/nginx` in the docker host shares files with `/etc/nginx/conf.d` inside the container. When you donwload/clone this repo it already comes with `/server-conf/nginx/nginx.conf` and we are now telling docker-compose to use this existing file as an extra config for the nginx server inside the container. It tells the server to listen to port 80, it sets up the server name as `localhost` and the default website files as index.php, index.html or index.html. It determines the root path where the website/application files should be found inside the container. It determines the path for the server access and error logs inside the container.


### PHPMyAdmin Container

* Uses the `phpmyadmin/phpmyadmin` docker image to install PHPMyAdmin into the container.
* Links to the DB container.
* Uses port 8080 in the docker host and port 80 inside the container. That means that from your computer/browser you have to access http://localhost:8080 to access the PHPMyAdmin.




