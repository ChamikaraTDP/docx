# install php 


### phpbrew not working use manual installation
## install phpbrew
https://github.com/phpbrew/phpbrew/wiki/Requirement

## guide
https://github.com/phpbrew/phpbrew?tab=readme-ov-file


## install php
`
phpbrew install 5.4.0 +default
`


## manual installation

### install php 7.2

```
- sudo add-apt-repository ppa:ondrej/php

- sudo apt update

- sudo apt install php7.2 php7.2-common php7.2-cli php7.2-fpm
```

### install composer
install composer dependencies

- apt install gzip unrar unzip

- https://getcomposer.org/download/

- composer

### install laravel 

dependency
- sudo apt install php7.2-dom

- configure php config file at /etc/php/7.2/cli/php.ini

- composer create-project laravel/laravel [name]


## toggle php version

edit php symlink on /usr/bin

- sudo ln -sf php7.2 php


## Docker php

### image
php:7.4-alpine

### Install extensions
docker-php-ext-install [ext]

```
docker-php-ext-install pdo_mysql mysqli
```


[php docker img](https://hub.docker.com/_/php)
