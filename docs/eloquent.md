
# Eloquent Command    

Eloquent is an object relational mapper (ORM) that is included by default within the Laravel framework. An ORM is software that facilitates handling database records by representing data as objects, working as a layer of abstraction on top of the database engine used to store an application’s data.

Read more [https://laravel.com/docs/9.x/eloquent](https://laravel.com/docs/9.x/eloquent)

!!! note
    Class `Company` adalah merupakan nama model, tukarkan nama class kepada nama model yang bersesuaian mengikut pembangunan sistem.

## Select

List all data

    Company::all()

Take 3 data 

    Company::take(3)->get()

Take 3 data in all 

    Company::take(3)->get()->all()

Get first data 

    Company::first()

Get single record with ID

    Company::find(1)

Get multiple record with ID

    Company::find([1,2,3])

Take 3 latest 

    User::latest()->take(3)->get()

To Array

    Manufacture::get()->toArray()

## Where

    Company::where('website', 'lowe.com')->first()

or 

    Company::whereWebsite('lowe.com')->first()


    Manufacture::whereRaw('name like ?', ['Apple'])->get();


## Delete

    $company = Company::find(1)
    $company->delete();

or 

    Company::destroy(11)
    Company::destroy([1,2,3])

## GroupBy

    Phone::groupBy('name')->selectRaw('name, count(id) as total')->get()

## OrderBy

    hone::groupBy('name')->selectRaw('name, count(id) as total')->orderBy('total', 'desc')->get()

## Mass Assigment

Letakkan seperti berikut untuk membolehkan property di assign 

    protected $guarded = [];

Senaraikan property yang hanya digunakan dalam mass assignment, jika ada property yang dimasukkan tapi tidak wujud dalam table, still tiada error akan dipaparkan

```php
<?php 
protected $fillable = [
    'name',
    'address',
    'website',
    'email'
];
```

## Relationships

Relationship adalah **1 to \* ** (1 company mempunyai banyak contact). Define relationshop dalam migration contacts

```php
<?php 
public function up()
{
    Schema::create('contacts', function (Blueprint $table) {
        $table->id();
        $table->string('first_name');
        $table->string('last_name');
        $table->string('phone')->nullable();
        $table->string('email');
        $table->string('address');
        // $table->unsignedBigInteger('company_id');
        // $table->foreignId('company_id')->references('id')->on('companies')->onDelete('cascade');
        $table->foreignId('company_id')->constrained()->onDelete('cascade');
        $table->timestamps();
    });
}
```

Define relationship method in **contact model**, eloquent akan assume column adalah company_id based pada nama method 

    public function company()
    {
        return $this->belongsTo(Company::class);
    }

sekiranya nama column foreign key adalah bukan company_id 

    public function company()
    {
        return $this->belongsTo(Company::class, 'nama_column_foreign_key');
    }


Define relationship method in **company model**

    public function contacts()
    {
        return $this->hasMany(Contact::class);
    }

Saving contacts to model, $contacts is array of contact property

    $company->contacts()->createMany($contacs)


### Querying relationship

Return Illuminate\Database\Eloquent\Collection

    $company->contacts()->get()

Find contact with ID

    $copmany->contacts()->find(7)

Find last conctact in relationship

    $copmany->contacts()->orderBy('id', 'desc')->first()

Refresh relationship 

    $company->load('contacts)

atau 

    $company->refresh()

Delete first contact in relation

    $company->contacts()->first()->delete()

Delete all contacts in relation

    $company->contacts()->delete()

## Pluck

    Companies::orderBy('name')->pluck('name', 'id')

## Eager Loding : with

Eager loading - load data dan relation dalam single query, eloquent by default akan load guna teknik lazy loading - hanya load bila ekses kepada property/method dalam relation. Untuk kaedah eager loading bertujuan untuk optimize query untuk performance sistem

    Contact::with('company')->get();

## Nested eager loading

    User::with('companies', 'companies.contacts')->get()

boleh juga load selepas dah create object

    $user = User::take(2)->get();
    $user->load('contacts');

## without

Untuk remove relation yang eager loading guna property `with`

    User::without('contacts')->get()

## Counting record relation using `withCount`

    User::withCount(['contacts','companies'])->take(3)->get()

    // define column as
    User::withCount(['contacts as contact_number','companies'])->take(3)->get()

    // with count condition 
    User::withCount(['contacts as contact_number','companies' => function($query){
        $query->where('name', 'like', '%and%');
    }])->take(3)->get()

atau boleh load selepas object

    $user->loadCount('relation')

    // atau
    $user->loadCount(['relation'])