

### 1 - Install DOMPDF Package

    composer require barryvdh/laravel-dompdf

### 2 - Modified `config/app.php`

```php
<?php 
'providers' => [
    ....
    Barryvdh\DomPDF\ServiceProvider::class,
],

'aliases' => [
    ....
    'PDF' => Barryvdh\DomPDF\Facade::class,
]
```
### 3 - Add route

    Route::get('/manufacture/pdf', [ManufactureController::class, 'createPdf']);

### 4 - Add Controller method

```php 
<?php 
public function createPdf()
{
    $manufacture = Manufacture::find(1);
    return Pdf::loadView('manufacture.pdf_manufacture_data', ['name' => 'Nama Manufacture'])
    ->setPaper('A4', 'landscape')
    ->stream('data.pdf');
    // return $pdf->download('data.pdf');

}
```
### 5 - Blade - Create view file

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        body{font-family: Arial, Helvetica, sans-serif;font-size: 12px}
    </style>
</head>
<body>
    <table class="table">
        <tr>
            <td>Name</td>
            <td>{{ $name }}</td>
        </tr>
        <tr>
            <td>Address</td>
            <td></td>
        </tr>
        <tr>
            <td>Logo</td>
            <td></td>
        </tr>   
    </table>

</body>
</html>
```

### API Reference

[https://github.com/barryvdh/laravel-dompdf](https://github.com/barryvdh/laravel-dompdf)