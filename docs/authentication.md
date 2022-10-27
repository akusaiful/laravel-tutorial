
## Custom Authentication Redirection

Simple method, change property `$redirectTo`. `app\Http\Controllers\Auth\LoginController.php`.

    protected $redirectTo = RouteServiceProvider::HOME;

Untuk registration redirect change `app\Http\Providers\RouteServiceProvider.php`

    public const HOME = '/home';

Logout. Redirect user ke page login. Tambahkan method berikut ke `app\Http\Controllers\Auth\LoginController.php`

```php 
<?php 
protected function loggedOut(Request $request)
{
    return redirect('/login');
}
```

## Protecting Route

###  Method 1 : Attaching the `auth` middleware in routes definition

Append middleware ke route 

```php
<?php 
Route::get('contacts', [\App\Http\Controllers\ContactController::class, 'index'])->name('contacts.index')->middleware('auth');
```

atau masukkan semua route dalam group seperti berikut

```php
<?php 
Route::middleware('auth')->group(function () {
    Route::get('/contacts', [ContactController::class, 'index'])->name('contacts.index');
}
```

### Method 2 : Calling the `auth` middleware in controller's constructor

Before we call the auth middleware in our constructor's controller, make sure you're not call the middleware('auth') in your route definition.

```php
<?php 
class ContactController extends Controller
{
    public function __construct()
    {
        $this->middleware('auth');
    }

    // other methods definition
    // ...
}
```

apply auth to certain method

```php
<?php 
public function __construct()
{
    $this->middleware('auth')->only('create', 'update', 'destroy');
}
```

or `except` method

```php
<?php 
public function __construct()
{
    $this->middleware('auth')->except('index', 'show');
}
```

## Email verification

Change `User` model to implement `mustVerifyEmail`

```php
<?php 
class User extends Authenticatable implements MustVerifyEmail
{
}
```

Add `verify` option ke dalam `web.php` 

    Auth::routes(['verify' => true]);

Add to controller. `ContactController.php`.

```php
<?php 
public function __construct()
{
    $this->middleware(['auth', 'verified']);
}
```

atau guna group dalam `web.php`

```php
<?php 
Route::middleware(['auth', 'verified'])->group(function () {
    Route::get('/contacts', 'ContactController@index')->name('contacts.index');
    Route::post('/contacts', 'ContactController@store')->name('contacts.store');
    // ...
});
```

## Displaying Authenticated User Data

Change controller `ContactController.php'

```php
<?php 
$contacts = Contact::query()->orderBy('first_name', 'asc')->filter()->paginate(8); 
$companies = Company::orderBy('name')->pluck('name', 'id')->prepend('All Companies', '');   
```

to

```php
<?php 
$contacts = \auth()->user()->contacts()->orderBy('first_name', 'asc')->filter()->paginate(8);
$companies = \auth()->user()->companies()->orderBy('name')->pluck('name', 'id')->prepend('All Companies', '');
```

Add `user_id` to property `fillable` in model `contact.php`
    
    protected $fillable = ['first_name', 'last_name', 'email', 'phone', 'address', 'company_id', 'user_id'];

Change save method in `Contactcontroller.php`

```php
<?php 
// first method
Contact::create($request->all() + ['user_id' => \auth()->id()]);

// Second method
// Update 
$request->user()->contacts()->find($id)->update($request->all());
// Create new
$request->user()->contacts()->create($request->all());
```

## Password Confirm Page 

Laravel added new feature in the Authentication System to force user to confirm their password before accessing a certain page in your application

In you open the app/Http/Kernel.php you will find in the routeMiddleware array the password.confirm middleware has been defined.

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