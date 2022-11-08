## 1. Create translation file

Laravel akan melihat fail translation di lokasi berikut : `resources/lang/<languagecode>/<filename>.php`.

Create languagecode `malay` dan nama filename `phone.php` untuk membuat translation berkaitan module phone sebagai contoh.

```php
<?php

return [
    'title' => 'Directori Telefon'
];
```

## 2. Masukkan translation ke view __()

Tukar statik text berikut : 

    <div class="d-flex justify-content-between align-items-center">
        <h2>Phone Directory</h2>
        <ol>
            <li><a href="index.html">Home</a></li>
            <li>Inner Page</li>
        </ol>
    </div>

Menggunakan __() translation facade

    <div class="d-flex justify-content-between align-items-center">
        <h2>{{ __('phone.title') }}</h2>    
        <ol>
            <li><a href="index.html">Home</a></li>
            <li>Inner Page</li>
        </ol>
    </div>

## 3. Ubah language kepada 'malay' dalam `config/app.php'


    'locale' => 'malay',

ekses ke URL `/phones` untuk menguji smada locale berfungsi dengan baik atau tidak.


