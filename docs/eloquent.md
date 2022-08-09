
# Basic Eloquent Command    

>Class Company adalah merupakan nama model, tukarkan nama class kepada nama model yang bersesuaian mengikut pembangunan sistem

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

## Where

    Company::where('website', 'lowe.com')->first()

or 

    Company::whereWebsite('lowe.com')->first()

## Delete

    $company = Company::find(1)
    $company->delete();

or 

    Company::destroy(11)
    Company::destroy([1,2,3])

## Mass Assigment

Letakkan seperti berikut untuk membolehkan property di assign 

    protected $guarded = [];

Senaraikan property yang hanya digunakan dalam mass assignment, jika ada property yang dimasukkan tapi tidak wujud dalam table, still tiada error akan dipaparkan

    protected $fillable = [
        'name',
        'address',
        'website',
        'email'
    ];

## Relationships

Relationship adalah **1 to \* ** (1 company mempunyai banyak contact). Define relationshop dalam migration contacts

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
