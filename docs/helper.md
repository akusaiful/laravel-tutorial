----

`Helper` adalah method/class untuk membantu memudahkan penulisan code seperti `request()` dan `auth()`.

1. Create folder `app\Helpers`

2. Create file `app\Helpers\Helpers.php` and add code : 

```php
<?php
if(!function_exists('getName')){
    // get name from user model
    function getName()
    {
        return auth()->user()->name;
    }
}
```  
3. Add to `composer.json'

```json
"autoload": {
    "psr-4": {
        "App\\": "app/",
        "Database\\Factories\\": "database/factories/",
        "Database\\Seeders\\": "database/seeders/"
    },
    "files": [
        "app/helpers/Helpers.php"
    ]
},
```
4. run 

    composer dump-autoload

Use code in view or controller

    // view
    {{  getName() }}

