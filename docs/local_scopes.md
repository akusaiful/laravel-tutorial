# Scope Filter

Scope digunakan dalam model bagi mengurangkan pengulangan code yang sering digunakan

Model

```php
<?php 
public functon scopePublished($query){
    return $query->where('published_at', '<=', now());
}
```

Controller

    Post::latestFirst()->published()->get();

### Query Searching 

Model

```php 
<?php 
public function scopeFilter(\Illuminate\Database\Eloquent\Builder $query)
{
    if($companyId = \request('company_id')){
        $query->where('company_id', $companyId);
    }

    if($queryText = \request('query_text')){
        $query->where('first_name', 'LIKE', "%{$queryText}%");
    }

    return $query;
}
```

Controller

    $contacts = Contact::query()->orderBy('first_name', 'asc')->filter()->paginate(8);