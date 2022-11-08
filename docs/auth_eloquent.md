
## Query owner record

Change controller `ContactController.php'

```php
<?php 
$contacts = Contact::query()->orderBy('first_name', 'asc')->filter()->paginate(8); 
$companies = Company::orderBy('name')->pluck('name', 'id')->prepend('All Companies', '');   
```

to

```php
<?php 
$contacts = \auth()->user()->contacts()->orderBy('first_name', 'asc')->filter()->paginate(8);
$companies = \auth()->user()->companies()->orderBy('name')->pluck('name', 'id')->prepend('All Companies', '');
```

## Create and Save Record with user_id (Owner record)

Add `user_id` to property `fillable` in model `contact.php`
    
    protected $fillable = ['first_name', 'last_name', 'email', 'phone', 'address', 'company_id', 'user_id'];

Change save method in `Contactcontroller.php`

```php
<?php 
// first method
Contact::create($request->all() + ['user_id' => \auth()->id()]);

// Second method
// Update 
$request->user()->contacts()->find($id)->update($request->all());
// Create new
$request->user()->contacts()->create($request->all());
```
