# Installation Setup

## Pre-Install

Check PHP version, pastikan php setup di environment path

    php -v

required version  >= 7.2.5


Check composer 

    composer -v

!!! todo
    Install composer [https://getcomposer.org/](https://getcomposer.org/)

## Create Application

Create application

    composer create-project laravel/laravel hello-laravel

Go to folder

    cd hello-laravel

## Running application

Laravel comes bundled with a PHP-based web server which can be started by running
 
    php artisan serve

or serve with custom port
    
    php artisan serve --port 8484

Default using `localhost`
    
    php artisan serve --host=192.168.0.100 --port=8080    