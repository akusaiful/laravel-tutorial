## Authentication Scaffolding

Laravel telah menyediakan proses authentication yang asas untuk proses login, logout, forgot password dan reset password. Laravel menggunakan migration table berikut : 

1. `database\migrations\2014_10_12_000000_create_users_table` 
2. `database\migrations\2014_10_12_100000_create_password_resets_table` 

untuk melaksanakan proses automasi ini dan menggunakan eloquent `app\Models\User.php` to menguruskan pengguna.

## Install 

Untuk authentication dalam laravel akan guna package `laravel/ui`. Install `laravel/ui` package

    composer require laravel/ui --dev

Run  

    php artisan

verify available package command 

    ui                     Swap the front-end scaffolding for the application
    .
    .
    ui
        ui:auth            Scaffold basic login and registration views and routes
        ui:controllers     Scaffold the authentication controllers

Generate authentication scaffolding package (`ui:auth` akan guna default bootstrap) 

    php artisan ui:auth

package will install controller and views related to auth, including `HomeController` and in `routes\web.php`
 
```php 
 <?php 
app\Http\Controllers\Auth
app\Http\Controllers\HomeController.php
resources\views\auth
resources\views\layout\app.blade.php

// Modified web.php
Auth::routes();
Route::get('/home', [App\Http\Controllers\HomeController::class, 'index'])->name('home');
```

Verify new route for authentication

    php artisan list

Login to system and modified content 

    /login

Access auth object

    auth()->user()->name

!!! info
    Test installation dengan ekses ke URL `http://localhost:8000/login` atau `http://localhost:8000/register`

    > :bulb: Modified page mengikut bootstrap dengan menggunakan layout yang digunakan di dalam sistem.


## Remove certain functionality

Auth module boleh untuk remove beberapa bahagian yang didatangkan secara default oleh leravel, lihat pada fail `routes/web.app` 

    Auth::routes([
        'register' => false,
        'reset'  => false,
        'verify' => false
    ]);
