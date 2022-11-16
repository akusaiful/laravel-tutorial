
Code yang sudah lengkap sepanjang kelas akan dikongsi di URL ini : 

Day 1 : Route + Calculator + Template

    https://github.com/akusaiful/laravel-code

Day 2, 3 dan 4 : Complete System

    https://github.com/akusaiful/laravel-crud.git


## Clone Project Step

Langkah untuk clone dari github

#### 1. Clone Project 

Clone dekat folder yang dikehendaki

    github clone https://github.com/akusaiful/laravel-crud.git

#### 2 . Run composer

Masuk ke directory `laravel-crud` selepas clone. Run arahan berikut : 

    composer update

#### 3. Copy/rename file `.env.example`

Copy file `.env.example` kepada `.env` . Lengkapkan file config seperti nama database, db password dan sebagainya. Pastikan nama database telah dibuat di phpmyadmin/db

    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=laraveldb-create-db-dalam-mysql
    DB_USERNAME=root
    DB_PASSWORD=

#### 4. Jana laravel key

    php artisan key:generate

#### 5. Run migration table

Migration table akan create semua table yang diperlukan oleh sistem ke database

    php artisan migrate

