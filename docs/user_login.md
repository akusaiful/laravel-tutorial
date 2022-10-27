## Authentication Scaffolding

Laravel telah menyediakan proses authentication yang asas untuk proses login, logout, forgot password dan reset password. Laravel menggunakan migration table berikut : 

1. `database\migrations\2014_10_12_000000_create_users_table` 
2. `database\migrations\2014_10_12_100000_create_password_resets_table` 

untuk melaksanakan proses automasi ini dan `app\Models\User.php` to handle user

### Install 

Untuk authentication dalam laravel akan guna package `laravel/ui`. Install `laravel/ui` package

    composer require laravel/ui --dev

Run `php artisan` to verify available package command 

    ui                     Swap the front-end scaffolding for the application
    .
    .
    ui
        ui:auth            Scaffold basic login and registration views and routes
        ui:controllers     Scaffold the authentication controllers

Generate authentication scaffolding package (`ai:auth` akan guna default bootstrap) 

    php artisan ui:auth

package will install controller and views related to auth, including `HomeController` and in `routes\web.php`
 
    app\Http\Controllers\Auth
    app\Http\Controllers\HomeController.php
    resources\views\auth
    resources\views\layout\app.blade.php

    // Modified web.php
    Auth::routes();
    Route::get('/home', [App\Http\Controllers\HomeController::class, 'index'])->name('home');

Verify new route for authentication

    php artisan list


Login to system and modified content 

    /login

Access auth object

    auth()->user()->name

> Test installation dengan ekses ke URL `http://localhost:8000/login` atau `http://localhost:8000/register`

> :bulb: Modified page mengikut bootstrap dengan menggunakan layout yang digunakan di dalam sistem.

## Control Navigation Visibility 

    @auth
    // protected content for authentication user
    @endauth

Only available for guest

    @guest
    // this content will hide after user authenticated
    @endguest

    // or 

    @guest
    // for guest
    @else
    // for auth
    @endguest


For logout, use POST method


```html
<a class="dropdown-item" href="{{ route('logout') }}" 
    onclick="event.preventDefault(); document.getElementById('logout-form').submit();">
    {{ __('Logout') }}
</a>

<form id="logout-form" action="{{ route('logout') }}" method="POST" class="d-none">
    @csrf
</form>
```

## Retrieving The Authenticated User

Laravel also provide some alternative ways like so:

1. Using `Auth` Facade
2. Using `Request` instance or `request` helper


### Using Auth Facade

You can retrieve the current user's instance using this syntax:

```php
<?php 
use Illuminate\Support\Facades\Auth;

$user = Auth::user(); // this is equivalent with auth()->user()
```

Blade 

    {{ auth()->user()->name }}
    
If you want to get only the authenticated user's id you can use this syntax:

```php
<?php 
use Illuminate\Support\Facades\Auth;

$user = Auth::id(); // this is equivalent with auth()->id()
```

You can also determine if the user already logged in by using check() method. This method will return true if the user authenticated. Otherwise it will return false.

```php
<?php 
use Illuminate\Support\Facades\Auth;

if (Auth::check()) {
// The user is logged in.
} else {
// The user is not logged in.
}
```

### Using Request Instance

You can retrieve the current user's instance via Request instance like this.

```php
<?php 
use Illuminate\Http\Request;

public function store(Request $request)
{
    $user = $request->user();
}
```

You can also retrieve the current user's instance via request global helper function like this.

```php
<?php 
public function store()
{
    $user = request()->user();
}
```
