
## Custom Authentication Redirection

### After login

Simple method, change property `$redirectTo` di file `app\Http\Controllers\Auth\LoginController.php`.

    protected $redirectTo = RouteServiceProvider::HOME;

### After logout

Redirect user back to login page . Tambahkan method berikut ke file `app\Http\Controllers\Auth\LoginController.php`

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

// or
Route::get('/contacts', [ContactController::class, 'index'])->name('contacts.index')->middleware('auth');
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
