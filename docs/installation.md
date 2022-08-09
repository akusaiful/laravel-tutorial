Create application

    composer create-project laravel/laravel example-app

Run

    cd example-app
 
    php artisan serve


![Voyager](img/voyager.png)

Follow instalation step to Install voyager.

     https://github.com/the-control-group/voyager 

If error like this

    SQLSTATE[42000]: Syntax error or access violation: 1071 Specified key was too long; max key length is 767 bytes (SQL: alter table `users` add unique `users_email_unique`(`email`))

update file `/app/Providers/AppServiceProvider.php`

    use Illuminate\Support\Facades\Schema;

    /**
    * Bootstrap any application services.
    *
    * @return void
    */
    public function boot()
    {
        Schema::defaultStringLength(191);
    }

>:bulb: Alternatively, you may enable the `innodb_large_prefix` option for your database. Refer to your database's documentation for instructions on how to properly enable this option.

---

