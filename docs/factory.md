### Factory

Factory digunakan untuk menjana data bagi tujuan pembangunan dan pengujian yang lebih pantas dan mudah.

Generate factory class 

    php artisan make:factory PhoneFactory

Code berikut akan dijana di `database/factories`

```php 
<?php

namespace Database\Factories;

use Illuminate\Database\Eloquent\Factories\Factory;

class PhoneFactory extends Factory
{
    /**
     * Define the model's default state.
     *
     * @return array
     */
    public function definition()
    {
        return [
            //
        ];
    }
}
```

Masukkan factory definition

```php
<?php

public function definition()
    $manufacture = Manufacture::inRandomOrder()->first();        
    return [
        'name' => $this->faker->name . $manufacture->name,
        'number' => $this->faker->phoneNumber,
        'is_deleted' => rand(0,1),
        'manufacture_id' => $manufacture->id
    ];
}
```

Generate rekod factory di database seeder `database\seeders\DatabaseSeeder.php`

```php 
<?php 
namespace Database\Seeders;

use App\Models\Manufacture;
use App\Models\Phone;
use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    /**
     * Seed the application's database.
     *
     * @return void
     */
    public function run()
    {        
        // seed table manufactures first
        $this->call(ManufactureSeeder::class);

        // seed table user then phone, every user have 10 phone
        // akan generate 100 rekod phone
        \App\Models\User::factory(10)
            ->has(Phone::factory()->count(10))
            ->create();        
    }
}
```

Run menggunakan artisan

    php artisan db:seed



