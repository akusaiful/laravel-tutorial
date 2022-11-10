
## Hello Laravel - First Flight

Taip code berikut di file `routes/web.php` untuk dapatkan paparan di browser dan ekses melalui `/hello`

```php
<?php
Route::get('hello', function(){
    return 'Hello laravel';
});
```

Masukkan kod HTML 

```php
<?php
Route::get('hello', function(){
    return "<h1>Hello</h1>
        <p>Ini adalah permulaan code</p>
    ";
});
```

Mengunakan view

```php
<?php
Route::get('hello', function(){
    return view('hello');
});
```

Passing variable to view
    
```php
<?php
Route::get('hello', function(){
    return view('hello')->with('name', 'Siti Nur Farhan');
});
```

Get variable from URI

```php
<?php
Route::get('hello/{name}', function($name){    
    return view('hello', compact('name'));
});
```

Get variable from `Request` object

```php
<?php
use Illuminate\Http\Request;
Route::get('hello/{name}', function(Request $request){    
    return view('hello')->with('name', $request->name);
});
```

Multiple varible passing

```php
<?php
Route::get('hello/{name}/{age}', function(Request $request){    
    return view('hello')->with('name', $request->name)->with('age', $request->age);
});
```

Passing data using `with[]` ke view 

```php
<?php
Route::get('hello/{name}/{age}', function (Request $request) {
    return view('hello')->with([
        'name' =>  $request->name,
        'age' =>  $request->age
    ]);
});  
```

Optional parameter use `?`

```php
<?php
Route::get('hello/{name}/{age?}', function (Request $request) {
    return view('hello')->with([
        'name' =>  $request->name,
        'age' =>  $request->age
    ]);
}); 
```

Route view

```php
<?php   
Route::view('/hello', 'welcome', ['name' => 'Ahmad']);
```

For more documention about routing [https://laravel.com/docs/9.x/routing](https://laravel.com/docs/9.x/routing)

## :japanese_ogre: Latihan

1. Tulis satu route `/calc` untuk membolehkan pengiraan sifir 2 seperti berikut : 

        1 * 2 = 2
        2 * 2 = 4
        ..
        ..
        12 * 2 = 24

2. Jadikan pengiraan diatas sebagai variable yang membolehkan input dari user diambil untuk membuat pengiraan, contoh `/calc/4` akan mengira sifir 4 atau `/calc/12` akan mengira sifir 12    