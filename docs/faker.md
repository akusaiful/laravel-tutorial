### Faker library

Faker library juga boleh digunakan untuk menjana data dummy yang lebih dinamik. 

Masukkan code berikut di method `run()` dalam file seeder     

```php
<?php

namespace Database\Seeders;

use Faker\Factory;
use Illuminate\Database\Seeder;
use Illuminate\Support\Facades\DB;

class StockSeeder extends Seeder
{
    /**
    * Run the database seeds.
    *
    * @return void
    */
    public function run()
    {            
        DB::table('stocks')->truncate();

        $companies = [];
        $faker = Factory::create();

        foreach (range(1, 1000) as $key){
            $companies[] = [
                'name' => $faker->name,
                'address' => $faker->address,
                'website' => $faker->domainName,
                'email' => $faker->email,
                'created_at' => now(),
                'updated_at' => now()
            ];
        }

        DB::table('stocks')->insert($companies);
    }
}
```

Faker library API [https://github.com/fzaninotto/Faker](https://github.com/fzaninotto/Faker)