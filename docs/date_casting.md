---

Date Casting

**Model** :  

    protected $casts = [
        'activated_at' => 'date:m/d/Y',
        'deactivated_at' => 'date:m/d/Y',
    ];

**Akse from view** : 

    $project->toArray()['activated_at'];


Read More : 
[https://laraveldaily.com/post/laravel-datetime-to-carbon-dates-casts](https://laraveldaily.com/post/laravel-datetime-to-carbon-dates-casts)

