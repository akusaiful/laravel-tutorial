## Tugasan 

```php 
<?php
Route::get('/email', function(){
    Mail::to('akusaiful@gmail.com')->send(new MailSender([
        'title' => 'test',
        'content' => 'content'
    ]));        
});
```

1. Gantikan penggunaan `route` diatas dengan menggunakan controller untuk membuat penghantaran email.
2. Gunakan view file
3. Email dihantar setelah button di tekan 
4. Maklumkan status email berjaya dihantar kepada pengguna.
