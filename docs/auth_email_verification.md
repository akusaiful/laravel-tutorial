
## Email verification

### Configure mail 

Configuration email berada di dalam fail `config/mail.php` dengan menggunakan fail `.env`. 
Untuk tujuan kelas akan menggunakan konfigurasi berikut : 

    MAIL_MAILER=smtp
    MAIL_HOST=smtp.hostinger.com
    MAIL_PORT=587
    MAIL_USERNAME=training@danconsult.my
    MAIL_PASSWORD=qwertyuiOP25$$
    MAIL_ENCRYPTION=null
    MAIL_FROM_ADDRESS=training@danconsult.my
    MAIL_FROM_NAME="${APP_NAME}"

Change `User` model to implement `MustVerifyEmail`

```php
<?php 
class User extends Authenticatable implements MustVerifyEmail
{
}
```

Add `verify` option ke dalam `web.php` 

    Auth::routes(['verify' => true]);

Add to controller `ContactController.php`.

```php
<?php 
public function __construct()
{
    $this->middleware(['auth', 'verified']);
}
```

atau guna group dalam `web.php`

```php
<?php 
Route::middleware(['auth', 'verified'])->group(function () {
    Route::get('/contacts', 'ContactController@index')->name('contacts.index');
    Route::post('/contacts', 'ContactController@store')->name('contacts.store');
    // ...
});
```

Testing register new user dan kemudian access ke route contacts.