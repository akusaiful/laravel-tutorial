---
title: Getting Started
summary: Dokumen ini menerangkan proses daftar masuk ke sistem mylatihan
authors:
    - akusaiful    
date: 2022-05-7
some_url: academy.jpj.gov.my/mylatihan
---

# Database Migration

    

## Migration

Install migration table

    php artisan migrate:install 


Install all/changes/new tables dalam folder **database/migrations**

    php artisan migrate

Create table migration

    php artisan make:migration create_companies_table  

Rollback table migration

    php artisan migrate:rollback

## Seeder 

Create table seeder
     
     php artisan make:seeder CompaniesTableSeeder

Masukkan code untuk seeder table 

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


Run Seeder table
     
     php artisan db:seed --class=CompaniesTableSeeder 


or register seeder dalam file DatabaseSeeder.php

    <?php

    namespace Database\Seeders;

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
            // \App\Models\User::factory(10)->create();
            $this->call(CompaniesTableSeeder::class);
        }
    }

Kemedian laksanakan arahan berikut tanpa option

     php artisan db:seed

## Faker Library

    <?php

    namespace Database\Seeders;

    use Faker\Factory;
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
            DB::table('companies')->truncate();

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

            DB::table('companies')->insert($companies);
        }
    }

Kemudian laksanakan arahan berikut 

     php artisan db:seed


Composer - clear cache 

    composer dump-autoload

## Model

Create table model, use naming convetion camel case

    php artisan make:model <Model>

Create table model with migration table

    php artisan make:model <Model> -m 

## Pluck

    Companies::orderBy('name')->pluck('name', 'id')