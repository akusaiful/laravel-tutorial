Untuk authentication dalam laravel akan guna package `laravel/ui`. Install package

    composer require laravel/ui --dev 

Verify package

    php artisan

Verify following package available

    ui                          Swap the front-end scaffolding for the application

    ....


    ui
        ui:auth                Scaffold basic login and registration views and routes
        ui:controllers         Scaffold the authentication controllers

Generate auth scaffolding (`ai:auth` akan guna default bootstrap) 

    php artisan ui:auth

Route akan dimasukkan ke dalam `web.php` 

    Auth::routes();
    Route::get('/home', [App\Http\Controllers\HomeController::class, 'index'])->name('home');

> Test installation dengan ekses ke URL `http://localhost:8000/login` atau `http://localhost:8000/register`

> :bulb: Modified page mengikut bootstrap dengan menggunakan layout yang digunakan di dalam sistem.

## Control Visibility Navbar 

Protect navigation yang perlu dipaparkan hanya untuk user yang telah login

    @auth
    ..
    @endauth

Papar navagation untuk guest

    @guest
    .
    . // untuk public
    .
    @else
    .
    . // after authenticate user
    .
    @endguest

## User Auth

Get user profile after authenticate 

    {{ auth()->user()->name }}

### Using Auth Facade

You can retrieve the current user's instance using this syntax:

    use Illuminate\Support\Facades\Auth;
 
    $user = Auth::user(); // this is equivalent with auth()->user()

If you want to get only the authenticated user's id you can use this syntax:

    use Illuminate\Support\Facades\Auth;
 
    $user = Auth::id(); // this is equivalent with auth()->id()

You can also determine if the user already logged in by using check() method. This method will return true if the user authenticated. Otherwise it will return false.

    use Illuminate\Support\Facades\Auth;
    
    if (Auth::check()) {
    // The user is logged in.
    } else {
    // The user is not logged in.
    }

### Using Request Instance

You can retrieve the current user's instance via Request instance like this.

    use Illuminate\Http\Request;
    
    public function store(Request $request)
    {
        $user = $request->user();
    }

You can also retrieve the current user's instance via request global helper function like this.

    public function store()
    {
        $user = request()->user();
    }

## Custom Authentication Redirection

Simple method, change property `$redirectTo`. `app\Http\Controllers\Auth\LoginController.php`.

    protected $redirectTo = RouteServiceProvider::HOME;

Untuk registration redirect change `app\Http\Providers\RouteServiceProvider.php`

    public const HOME = '/home';

Logout. Redirect user ke page login. Tambahkan method berikut ke `app\Http\Controllers\Auth\LoginController.php`

    protected function loggedOut(Request $request)
    {
        return redirect('/login');
    }

## Protecting Route

###  Method 1 : Attaching the `auth` middleware in routes definition

Append middleware ke route 

    Route::get('contacts', [\App\Http\Controllers\ContactController::class, 'index'])->name('contacts.index')->middleware('auth');

atau masukkan semua route dalam group seperti berikut

    Route::middleware('auth')->group(function () {
        Route::get('/contacts', [ContactController::class, 'index'])->name('contacts.index');
    }

### Method 2 : Calling the `auth` middleware in controller's constructor

Before we call the auth middleware in our constructor's controller, make sure you're not call the middleware('auth') in your route definition.

    class ContactController extends Controller
    {
        public function __construct()
        {
            $this->middleware('auth');
        }
    
        // other methods definition
        // ...
    }

apply auth to certain method

    public function __construct()
    {
        $this->middleware('auth')->only('create', 'update', 'destroy');
    }

or `except` method

    public function __construct()
    {
        $this->middleware('auth')->except('index', 'show');
    }


## Email verification

Change `User` model to implement `mustVerifyEmail`

    class User extends Authenticatable implements MustVerifyEmail
    {
    }

Add `verify` option ke dalam `web.php` 

    Auth::routes(['verify' => true]);

Add to controller. `ContactController.php`.

    public function __construct()
    {
        $this->middleware(['auth', 'verified']);
    }

atau guna group dalam `web.php`

    Route::middleware(['auth', 'verified'])->group(function () {
        Route::get('/contacts', 'ContactController@index')->name('contacts.index');
        Route::post('/contacts', 'ContactController@store')->name('contacts.store');
        // ...
    });

## Displaying Authenticated User Data

Change controller `ContactController.php'

    $contacts = Contact::query()->orderBy('first_name', 'asc')->filter()->paginate(8); 
    $companies = Company::orderBy('name')->pluck('name', 'id')->prepend('All Companies', '');   

to

    $contacts = \auth()->user()->contacts()->orderBy('first_name', 'asc')->filter()->paginate(8);
    $companies = \auth()->user()->companies()->orderBy('name')->pluck('name', 'id')->prepend('All Companies', '');

 Change`user_id`  to property `fillable` in model `contact.php`
    
    protected $fillable = ['first_name', 'last_name', 'email', 'phone', 'address', 'company_id', 'user_id'];

Change save method in `Contactcontroller.php`

    // first method
    Contact::create($request->all() + ['user_id' => \auth()->id()]);

    // Second method
    // Update 
    $request->user()->contacts()->find($id)->update($request->all());
    // Create new
    $request->user()->contacts()->create($request->all());
    
## Password Confirm Page 

Laravel added new feature in the Authentication System to force user to confirm their password before accessing a certain page in your application

In you open the app/Http/Kernel.php you will find in the routeMiddleware array the password.confirm middleware has been defined.

    protected $routeMiddleware = [
        // ....
        'password.confirm' => \Illuminate\Auth\Middleware\RequirePassword::class,
        // ...
    ];

Create a new controller

    php artisan make:controller Settings\\AccountController

Add method `index`

    class AccountController extends Controller
    {
        public function index()
        {
            return "<h1>Account Settings</h1>";
        }
    }

Define new route

    Route::get('/settings/account', [AccountController::class, 'index']);

Implement middleware the `password.confirm`

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

You can configure the length of time by changing the value of the `password_timeout` in your `config/auth.php` file.

    password_timeout' => 10800,