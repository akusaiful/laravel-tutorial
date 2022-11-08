# Routing

## Basic

Verb route 

```php
<?php
Route::get($uri, $callback);
Route::post($uri, $callback);
Route::put($uri, $callback);
Route::patch($uri, $callback);
Route::delete($uri, $callback);
Route::options($uri, $callback);
```

Response to multiple verb

```php
<?php
Route::match(['get', 'post'], '/', function () {
    //
});
```

Response to any verb

```php
<?php
Route::any('/', function () {
    //
});
```

Return string 

```php
<?php
Route::get('contacts', function (){
    return "<h1>All Contacts</h1>";
});    
```

Routing with `view()` 

```php
<?php
Route::get('/', function () {
    return view('welcome');
});
```

Named routes

```php
<?php
Route::get('/', function () {
    return view('welcome');
})->name('contacts.index);
```

Return data menggunakan model

```php
<?php
Route::get('/contacts/{id}', function($id) {
    return \App\Models\Contact::find($id);
});
```


## Route using controller class 

Index

    Route::get('contacts', [\App\Http\Controllers\ContactController::class, 'index'])->name('contacts.index');

Create 

    Route::get('/contacts/create', [\App\Http\Controllers\ContactController::class, 'create'])->name('contacts.create');

View ID
    
    Route::get('/contacts/{id}', [\App\Http\Controllers\ContactController::class, 'view'])->name('contacts.view';

## Implicit - Route model binding

Change following method 

```php
<?php
public function view($id)
{
    $contact = \App\Models\Contact::findOrFail($id);
    return view('contacts.view', compact('contact'));
}
```

to 

```php
<?php
public function view(Contact $contact)
{
    return view('contacts.view', compact('contact'));
}
```

Change route `web.php`

```php
<?php
Route::get('/contacts/{id}', [\App\Http\Controllers\ContactController::class, 'view'])->name('contacts.view');
```

to 

```php
<?php
Route::get('/contacts/{contact}', [\App\Http\Controllers\ContactController::class, 'view'])->name('contacts.view');
```

## Explicit - Route Model Binding

Update file `app\Providers\RouteServiceProvider.php`. Dengan memasukkan injector model kita tidak perlu lagi define model yang akan digunakan dibahagian controller method parameter. 

```php
<?php
public function boot()
{
    $this->configureRateLimiting();

    $this->routes(function () {
        Route::prefix('api')
            ->middleware('api')
            ->namespace($this->namespace)
            ->group(base_path('routes/api.php'));

        Route::middleware('web')
            ->namespace($this->namespace)
            ->group(base_path('routes/web.php'));
    });

    // route model binding
    Route::model('contact', Contact::class);

    // route model binding with customization query
    Route::bind('contact', function($value){
        return Contact::where('first_name', $value)->firstOrFail();
    });
}
```

Route akan ditulis seperti berikut : 

```php
<?php
Route::get('/calc/{model}', function(Request $request, $model){
    $number = empty($request->number)? 2 : $request->number;
    return view('home.calc')->with('number', $number)->with('phone', $model);
});
```

Controller method boleh ditulis seperti berikut:

```php
<?php
public function view($contact)
{
    return view('contacts.view', compact('contact'));
}
```

Panggilan ke atas properties untuk object `contact` boleh dibuat seperti biasa seperti berikut `$contact->name` sebagai contoh.

## Customize key ID bagi route yang menggunakan teknik model binding

Sekiranya implicit model yang dibuat tidak merujuk column `id` sebagai referece key perlulah dibuat perubahan kepada model terlebih dahulu. Rujukan column `id` yang dibaca oleh model boleh ditukarkan kepada column lain untuk melaksanakan query seperti column `email` atau `nric`. Terdapat 2 kaedah yang boleh digunakan.

### Option 1

Override method `getRouteKey()` dalam model `Contact.php`

```php
<?php
public function getRouteKeyName()
{        
    // rujukan kepada column yg akan digunakan oleh query
    return 'first_name';
}
```

### Option 2

Guna route `web.php` (laravel 7+)

```php
<?php
Route::get('/contacts/{contact:first_name}', [\App\Http\Controllers\ContactController::class, 'view'])->name('contacts.view');
```
!!! tips
    
    Untuk Troubleshoot route, anda boleh menggunakan function `dd()` : 

        Route::get('/contacts/{contact:first_name}', function (\App\Models\Contact $contact){
            dd($contact);
        });

## Resource Route

Untuk method yang sama dalam controller bagi tugasan CRUD boleh menggunakan `Route::resource()` yang lebih mudah. Dengan kaedah ini, tidak perlu lagi define setiap route yang diperlukan. 

    Route::resource('contacts', \App\Http\Controllers\ContactController::class);

> route definintion method mestilah sama dengan yang disenaraikan oleh `php artisan route:list --name=contacts`. 

atau boleh ditulis seperti berikut menggunakan `[]` :

```php
<?php
Route::resources([
    'contacts' => \App\Http\Controllers\ContactController::class,
    'companies' => \App\Http\Controllers\CompanyController::class
]);  
```

## Partial Resource Route

Expose sebahagian resource menggunakan method `only()` : 

    Route::resource('contacts', \App\Http\Controllers\ContactController::class)->only(['index', 'create']);

Expose semua resource kecuali menggunakan method `except()`

    Route::resource('contacts', \App\Http\Controllers\ContactController::class)->except(['index', 'create']);

## Nested resources

Children of onother resource, route untuk capture one to many relationship. 

    Route::resource('/companies.contacts', ContactController::class);

🥵 ## Naming Resource Route

    Route::resource('/contacts', ContactController::class)->names([
        'index' => 'contacts.all',
        'show' => 'contacts.view'
    ]);

## API Resourceful Route

Untuk API controller, dicadangkan create API yang khusus untuk handle request dan gunakan namespace `App\Http\Controllers\Api\*`

    Route::apiResource('contacts', \App\Http\Controllers\ContactController::class);

Verify using artisan

    php artisan route:list --name=contacts --path=api

Atau boleh guna `[]` bersama `Route::apiResources()`

```php
<?php
Route::apiResources([
    'contacts' => \App\Http\Controllers\Api\ContactController::class,
    'companies' => \App\Http\Controllers\Api\CompanyController::class
]);  
```

## Print all route 

    php artisan route:list 

