
## Control Navigation Visibility 

    @auth
    // protected content for authentication user
    @endauth

Only available for guest

    @guest
    // this content will hide after user authenticated
    @endguest

    // or 

    @guest
    // for guest
    @else
    // for auth
    @endguest


For logout, use POST method


```html
<a class="dropdown-item" href="{{ route('logout') }}" 
    onclick="event.preventDefault(); document.getElementById('logout-form').submit();">
    {{ __('Logout') }}
</a>

<form id="logout-form" action="{{ route('logout') }}" method="POST" class="d-none">
    @csrf
</form>
```

## Retrieving The Authenticated User

Laravel menyediakan 2 cara untuk mendapatkan auth user :

1. Using `Auth` Facade
2. Using `Request` instance or `request` helper


### Using Auth Facade

You can retrieve the current user's instance using this syntax:

```php
<?php 
use Illuminate\Support\Facades\Auth;

$user = Auth::user(); // this is equivalent with auth()->user()
```

atau `auth()`. Penggunaan dengan blade

    {{ auth()->user()->name }}
    
If you want to get only the authenticated user's id you can use this syntax:

```php
<?php 
use Illuminate\Support\Facades\Auth;

$user = Auth::id(); // this is equivalent with auth()->id()
```

You can also determine if the user already logged in by using check() method. This method will return true if the user authenticated. Otherwise it will return false.

```php
<?php 
use Illuminate\Support\Facades\Auth;

if (Auth::check()) {
// The user is logged in.
} else {
// The user is not logged in.
}
```

### Using Request Instance

You can retrieve the current user's instance via Request instance like this.

```php
<?php 
use Illuminate\Http\Request;

public function store(Request $request)
{
    $user = $request->user();
}
```

You can also retrieve the current user's instance via request global helper function like this.

```php
<?php 
public function store()
{
    $user = request()->user();
}
```
