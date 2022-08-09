# Routing

## Using route web.php
Basic routing

    Route::get('/', function () {
        return view('welcome');
    });

Named routes

    Route::get('/', function () {
        return view('welcome');
    })->name('contacts.index);

Return data

    Route::get('/contacts/{id}', function($id) {
        return \App\Models\Contact::find($id);
    });

Return string 

    Route::get('contacts', function (){
        return "<h1>All Contacts</h1>";
    });


## Using controller

Index

    Route::get('contacts', [\App\Http\Controllers\ContactController::class, 'index'])->name('contacts.index');

Create 

    Route::get('/contacts/create', [\App\Http\Controllers\ContactController::class, 'create'])->name('contacts.create');

View ID
    
    Route::get('/contacts/{id}', [\App\Http\Controllers\ContactController::class, 'view'])->name('contacts.view');

List all route 

    php artisan route:list 