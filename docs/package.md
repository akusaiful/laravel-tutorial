## Pogination (Fast pagination)

    https://github.com/hammerstonedev/fast-paginate

## Extension for vscode 

    https://themeselection.com/vs-code-laravel-extensions/

## Install laravel-ide-helper (Debugbar)

Composer

    composer require --dev barryvdh/laravel-ide-helper --dev

Git 

    https://github.com/barryvdh/laravel-debugbar

Add this conditional statement in your `AppServiceProvider` to register the helper class.

    public function register()
    {
        if ($this->app->environment() !== 'production') {
            $this->app->register(\Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider::class);
        }
        // ...
    }

Generate a file to help the IDE understand Facades. You will need to restart Visual Studio Code.
    
    php artisan ide-helper:generate

## Voyager

    https://voyager.devdojo.com/

## Composer

    https://packagist.org/

## Explore Package

1. https://github.com/spatie/laravel-permission
2. https://github.com/Zizaco/entrust
3. https://github.com/jrean/laravel-user-verification
4. https://github.com/laravelcollective/annotations
5. https://github.com/yajra/laravel-datatables
6. https://intervention.io (Image manipulation)
7. https://github.com/InfyOmLabs/laravel-generator
8.  

# Laravel Query Detector N+1
1. https://github.com/beyondcode/laravel-query-detector

Customize query detector to use debugbar as default alert. 

Publish package : 

    php artisan vendor:publish --provider="BeyondCode\QueryDetector\QueryDetectorServiceProvider"

    // atau - then select Querydetector
    php artisan vendor:publish

open `config/querydetector.php`. Change output to : 

    'output' => [
        \BeyondCode\QueryDetector\Outputs\Debugbar::class
        // \BeyondCode\QueryDetector\Outputs\Alert::class,
        // \BeyondCode\QueryDetector\Outputs\Log::class,
    ]

