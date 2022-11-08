
## Password Confirm Page : Protected sensitive area

Laravel added new feature in the authentication system to force user to confirm their password before accessing a certain page in your application

In you open the `app/Http/Kernel.php` you will find in the routeMiddleware array the password.confirm middleware has been defined.

```php
<?php 
protected $routeMiddleware = [
    // ....
    'password.confirm' => \Illuminate\Auth\Middleware\RequirePassword::class,
    // ...
];
```

Create a new controller

    php artisan make:controller Settings\\AccountController

Add method `index`

```php
<?php 
class AccountController extends Controller
{
    public function index()
    {
        return "<h1>Account Settings</h1>";
    }
}
```

Define new route

    Route::get('/settings/account', [AccountController::class, 'index']);

Implement middleware the `password.confirm`

```php
<?php 
class AccountController extends Controller
{
    public function __construct()
    {
        $this->middleware(['auth', 'password.confirm']);
    }

    public function index()
    {
        return "<h1>Account Settings</h1>";
    }
}
```

You can configure the length of time by changing the value of the `password_timeout` in your `config/auth.php` file.

    password_timeout' => 10800,