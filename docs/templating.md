## Setup View Layout

Basic directory laravel

<img src="../img/view-directory.png" style="width:40%">

Create folder `resources\views\layouts\` and file `main.blade.php`. Sebagai main layout. Letakkan code berikut untuk content dari subview

    @yield('content')

di subview letakkan code berikut sebagai contoh di `resources\views\index.blade.php`

    @extends('layouts.main')

    @section('title', 'Tajuk here')

    @section('content')
    .
    .
    @endsection

dan call guna route berikut. Letak di file `routes\web.php` :

    Route::get('/', function(){
        return view('index');
    });
    
## Passing variable

    // First
    return view('folder.view_name')->with('variablename', $value)

    // Second
    return view('folder.view_name')->compact('variable_name')

    // Third
    return view('folder.view_name', [])


For more info [https://laravel.com/docs/5.5/blade](https://laravel.com/docs/5.5/blade)

