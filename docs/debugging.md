---

## SQL Debugging Eloquent Queries

Terdapat beberapa cara untuk debug SQL Query:

**Option 1** :  `enableQueryLog()`

    \DB::enableQueryLog();
    // query here
    dd(\DB::getQueryLog());

**Option 2** : Replace `->get()` kepada `->toSql()`

    Contact::toSql()

**Option 3** : add code dalam `app\Providers\AppServiceProvider`

```php
<?php
if (env('APP_DEBUG')) {
    DB::listen(function ($query) {
        Log::info($query->sql, $query->bindings, $query->time);
    });
}
```

lihat log : `storage\logs`    

**Option 4** : Install laravel debugbar

    https://github.com/barryvdh/laravel-debugbar

