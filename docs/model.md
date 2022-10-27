    
# Understanding Model : Eloquent

## Create Model

Create table model, use naming convetion camel case

```php
php artisan make:model <ModelName>
```

Create table model with migration table

    php artisan make:model <Model> -m    

Basic Eloquent model yang dijana : 

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Stock extends Model
{
    use HasFactory;
}
```

!!! tips

    `php artisan make:model Note -a`

    `-a` : to create model, migration, resource controller and factory  

    `php artisan make:model Note -mcrf 

    `-mcr` : to create model, controller, resource controller and factory

## Key Concepts

### tableName

    protected $tableName = 'Your table name';
## primaryKey

    protected $primaryKey = "Column_Name_Primary_Key";

## incrementing

Primary ID not incrementing set to false;

    public $incrementing = false;

### fillable 

    protected $fillable = ['your', 'massize', 'assign', 'attribute', 'list'];

### timestamps

Jika table tidak memmpunya attribute `created_at, updated_at`

    protected $timestamps = false;

### guarded

    proctedted $guarded = [];

### with()    

`with` - load automatically with eager loading when calling object

    protected $with = ['contacts', 'companies'];

## withCount()

Controller

    $company = Company::withCount('contacts')->paginate(10);

View

    @foreach($company as $item){
        {{ $item->contacts_count }}    
    @endforeach

!!! tip "More examples of withCount() method - in the official Laravel documentation"

    [https://laravel.com/docs/master/eloquent-relationships#counting-related-models](https://laravel.com/docs/master/eloquent-relationships#counting-related-models)   


## Hello Model 

Execute tinker

    php artisan tinker

Load model

    namespace App\Models

Get all data

    Model::all()

for more query [Eloquent](eloquent.md)  