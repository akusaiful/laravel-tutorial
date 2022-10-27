

## Pagination

Add `apps\Provider\AppServiceProvider\` 

```php 
<?php 
public function boot()
{        
    Paginator::useBootstrap();
}
```

Publish paginator to views

    php artisan vendor:publish

Selepas run `vendor:publish`, code tambahan akan dimasukkan di `resources\views\vendor\`  membolehkan customization dibuat keatas code pagination yang digunakan oleh laravel

Controller 

```php
<?php
 public function index()
{
   // call paginate from controller
    $model = BookType::paginate(8);
    return view('state.index')->with('model', $model);
}
```
    
View - Loop data in view

    @foreach($model as $type)                                            
        {{ $type->id }}
    @endforeach

    // Generate pagination page
    {{ $model->links() }}

