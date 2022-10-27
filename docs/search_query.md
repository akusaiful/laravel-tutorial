## Search query

A Query
```php 
<?php 
if ($companyId = \request('company_id')) {
    $query->where('company_id', $companyId);
}
```

A and B or B1.. query

```php 
<?php     
if ($companyId = \request('company_id')) {
    $query->where('company_id', $companyId);
}

if ($queryText = \request('query_text')) {                
    $query->where('first_name', 'LIKE', "%{$queryText}%");
    $query->orWhere('last_name', 'LIKE', "%{$queryText}%");
    $query->orWhere('email', 'LIKE', "%{$queryText}%");     
};
```

A and (B or B1..) Query

```php 
<?php 
if ($companyId = \request('company_id')) {
    $query->where('company_id', $companyId);
}

// group query  -> AND (other query)
if ($queryText = \request('query_text')) {                
    $query->where(function ($query) use ($queryText) {
        $query->where('first_name', 'LIKE', "%{$queryText}%");
        $query->orWhere('last_name', 'LIKE', "%{$queryText}%");
        $query->orWhere('email', 'LIKE', "%{$queryText}%");
    });
}
```

A and (B or B1..) query with relation

```php
<?php 

public function scopeFilter(\Illuminate\Database\Eloquent\Builder $query)
{
    if ($companyId = \request('company_id')) {
        $query->where('company_id', $companyId);
    }

    // group query  -> AND (other query)
    if ($queryText = \request('query_text')) {                
        $query->where(function ($query) use ($queryText) {
            $query->where('first_name', 'LIKE', "%{$queryText}%");
            $query->orWhere('last_name', 'LIKE', "%{$queryText}%");
            $query->orWhere('email', 'LIKE', "%{$queryText}%");
        });

        // search relation model
        $query->orWhereHas('company', function ($query) use ($queryText) {
            $query->where('name', 'LIKE', "%{$queryText}%");
        });

    }

    return $query;
}
```