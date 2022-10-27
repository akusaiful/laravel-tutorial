# Controllers

## Create Controller

Create controller with resource method

    php artisan make:controller CompanyController --resource
    // php artisan make:controller CompanyController -r

Create controller with full resource method and model binding

    php artisan make:controller CompanyController --model=Company

## Redirect from controller

Redirect to other named route with keyname varible as session

    redirect()->route('name.route', [$paramenter])->with('keyname', $value);

Untuk paparkan message dalam view 

    $message = session('keyname');

## Contructor call middleware

    public function __construct()
    {   
        $this->middleware('auth');
    }
