### Seeder 

Seeder digunakan untuk menjana data dummy bagi tujuan memudahkan pembangunan sistem dilaksanakan. 

Create table seeder
     
     php artisan make:seeder CompaniesTableSeeder

Masukkan code untuk seeder table 

```php
<?php

namespace Database\Seeders;

use Illuminate\Database\Seeder;
use Illuminate\Support\Facades\DB;

class CompaniesTableSeeder extends Seeder
{
    /**
    * Run the database seeds.
    *
    * @return void
    */
    public function run()
    {
        //
        DB::table('companies')->truncate();

        $companies = [];

        foreach (range(1, 1000) as $key){
            $companies[] = [
                'name' => $name = "Company $key",
                'address' => "Address $name",
                'website' => "website{$key}.com",
                'email' => "email$key@gmail.com",
                'created_at' => now(),
                'updated_at' => now()
            ];
        }

        DB::table('companies')->insert($companies);
    }
}
```

Run Seeder table
     
     php artisan db:seed --class=CompaniesTableSeeder 

Data yang telah dijana oleh  seeder akan kelihatan seperti berikut : 

![Laravel](img/table_seeder.png)


or register seeder dalam file DatabaseSeeder.php

```php
<?php

namespace Database\Seeders;

use Illuminate\Database\Seeder;
use Illuminate\Support\Facades\DB;

class DatabaseSeeder extends Seeder
{
    /**
    * Seed the application's database.
    *
    * @return void
    */
    public function run()
    {        
        // \App\Models\User::factory(10)->create();
        // temp for development
        DB::statement('SET FOREIGN_KEY_CHECKS=0;');

        $this->call(CompaniesTableSeeder::class);
        
        DB::statement('SET FOREIGN_KEY_CHECKS=0;');
    }
}
```

Laksanakan arahan berikut 

     php artisan db:seeds


