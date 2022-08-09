
## Route

    Route::get($uri, $callback);
    Route::post($uri, $callback);
    Route::put($uri, $callback);
    Route::patch($uri, $callback);
    Route::delete($uri, $callback);
    Route::options($uri, $callback);

Response to multiple verb

    Route::match(['get', 'post'], '/', function () {
        //
    });

Response to any verb

    Route::any('/', function () {
        //
    });

## Route model binding - Implicit

Change following method 

    public function view($id)
    {
        $contact = \App\Models\Contact::findOrFail($id);
        return view('contacts.view', compact('contact'));
    }

to 

    public function view(Contact $contact)
    {
        return view('contacts.view', compact('contact'));
    }

Change route `web.php`

    Route::get('/contacts/{id}', [\App\Http\Controllers\ContactController::class, 'view'])->name('contacts.view');

to 

    Route::get('/contacts/{contact}', [\App\Http\Controllers\ContactController::class, 'view'])->name('contacts.view');


## Customizing the key out route model binding

Change ID column to perform the query. Boleh juga guna column lain untuk buat query seperti `email` atau `nric` column
terdapat 2 kaedah untuk dipilih.

### Option 1

Ovveride method `getRouteKey()` dalam model `contact.php`

    public function getRouteKeyName()
    {        
        return 'first_name';
    }

### Option 2

Guna route `web.php` (laravel 7+)

    Route::get('/contacts/{contact:first_name}', [\App\Http\Controllers\ContactController::class, 'view'])->name('contacts.view');

Troubleshoot route only, you can write like this: 

    Route::get('/contacts/{contact:first_name}', function (\App\Models\Contact $contact){
        dd($contact);
    });


## Route Model Binding (Explicit)

Update file `app\Providers\RouteServiceProvider.php`. Dengan memasukkan injector model kita tidak perlu lagi define model yang akan digunakan dibahagian controller method parameter. 

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

        Route::model('contact', Contact::class);

        // atau
        Route::bind('contact', function($value){
            return Contact::where('first_name', $value)->firstOrFail();
        });
    }

atau boleh seperti berikut. Kaedah ini membolehkan customization dibuat keatas query

    Route::bind('contact', function($value){
        return Contact::where('first_name', $value)->firstOrFail();
    });

Controller method boleh ditulis seperti berikut:

    public function view($contact)
    {
        return view('contacts.view', compact('contact'));
    }

## Resource Route

Register resource controller at once. Dengan kaedah ini, tidak perlu lagi define setiap route yang diperlukan. change `web.php`. 

    Route::resource('contacts', \App\Http\Controllers\ContactController::class);

> route definintion method mestilah sama dengan yang disenaraikan oleh `php artisan route:list --name=contacts`. 

atau boleh ditulis seperti berikut :

    Route::resources([
        'contacts' => \App\Http\Controllers\ContactController::class,
        'companies' => \App\Http\Controllers\CompanyController::class
    ]);  

## Partial Resource Route

Only this route

    Route::resource('contacts', \App\Http\Controllers\ContactController::class)->only(['index', 'create']);

All route, Except

    Route::resource('contacts', \App\Http\Controllers\ContactController::class)->except(['index', 'create']);

## API Resourceful Route

Untuk API controller, dicadangkan create API yang khusus untuk handle request dan gunakan namespace `App\Http\Controllers\Api\*`

    Route::apiResource('contacts', \App\Http\Controllers\ContactController::class);

Verify using artisan

    php artisan route:list --name=contacts --path=api


Atau boleh guna resources

     Route::apiResources([
        'contacts' => \App\Http\Controllers\Api\ContactController::class,
        'companies' => \App\Http\Controllers\Api\CompanyController::class
    ]);  

## Nested resources

Children of onother resource, route untuk capture one to many relationship. 

    Route::resource('/companies.contacts', ContactController::class);

## Naming Resource Route

    Route::resource('/contacts', ContactController::class)->names([
        'index' => 'contacts.all',
        'show' => 'contacts.view'
    ]);