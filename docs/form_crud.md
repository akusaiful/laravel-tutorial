# CRUD 

Create controller

    php artisan make:controller ContactController -r

untuk melaksanakan operasi CRUD, 7 method dan verb seperti berikut diperlukan :

1. `index()` - `GET`
2. `create()` - `GET`
3. `store()` - `POST`
4. `show()` - `GET`
5. `edit()` - `GET`
6. `update()` - `PUT`
7. `destroy()` - `DELETE`

## Index 

Route

```php
<?php
Route::get('contacts/index', [Contactontroller::class, 'index'])->name('contacts.index');
```

Controller

```php
<?php
public function index()
{
    return view('contact.index', ['contacts' => Contact::paginate(10)]);
}
```

View

```html
<table class="table">
  <thead>
    <th>Name</th>
    <th>Email</th>
    <th></th>
  </thead>
  @if($contacts->total()) @foreach ($contacts as $contact)
  <tr>
    <td>{{ $contact->name }}</td>
    <td>{{ $contact->email }}</td>
    <td><a href="{{ route('contacts.show', $contact->id) }}">View</a></td>
  </tr>
  @endforeach @else
  <tr>
    <td colspan="3">Tiada rekod</td>
  </tr>
  @endif
</table>

{{ $contacts->links() }}
```

!!! tips "TIPS : Render pagination page"
    See [Pagination](pagination.md) topic

## View 

View - link to route

```html
<a
  href="{{ route('contacts.view', $contact->id) }}"
  class="btn btn-sm btn-circle btn-outline-info"
  title="Show"
  ><i class="fa fa-eye"></i
></a>
```

Route

```php
<?php
Route::get('/contacts/{id}', [\App\Http\Controllers\ContactController::class, 'view'])->name('contacts.view');
```

Controller

```php
<?php
public function view($id)
{
    $contact = \App\Models\Contact::findOrFail($id);
    return view('contacts.view', compact('contact'));
}
```

Blade - view record

```html
<div class="card-body">
  <div class="row">
    <div class="col-md-12">
      <div class="form-group row">
        <label for="name" class="col-md-3 col-form-label">Name</label>
        <div class="col-md-9">
          <p class="form-control-plaintext text-muted">
            {{ $contact->name }}
          </p>
        </div>
      </div>

      <div class="form-group row">
        <label for="email" class="col-md-3 col-form-label">Email</label>
        <div class="col-md-9">
          <p class="form-control-plaintext text-muted">{{ $contact->email }}</p>
        </div>
      </div>

      <div class="form-group row">
        <label for="phone" class="col-md-3 col-form-label">Phone</label>
        <div class="col-md-9">
          <p class="form-control-plaintext text-muted">{{ $contact->phone }}</p>
        </div>
      </div>

      <div class="form-group row">
        <label for="name" class="col-md-3 col-form-label">Address</label>
        <div class="col-md-9">
          <p class="form-control-plaintext text-muted">
            {{ $contact->address }}
          </p>
        </div>
      </div>
      
      <hr />
      <div class="form-group row mb-0">
        <div class="col-md-9 offset-md-3">
          <a href="{{ route('contacts.edit', $contact->id) }}" class="btn btn-info">Edit</a>
          <a href="#" class="btn btn-outline-danger">Delete</a>
          <a href="{{ route('contacts.index') }}" class="btn btn-outline-secondary">Cancel</a>
        </div>
      </div>
    </div>
  </div>
</div>
```

## Update

View - link to route

```html
<a href="{{ route('contacts.edit', $contact->id) }}" class="btn btn-sm btn-circle btn-outline-info" title="Show">Edit</a>
```

Route

```php
<?php
// Edit - show form
Route::get('/contacts/edit/{id}', [\App\Http\Controllers\ContactController::class, 'edit'])->name('contacts.edit');

// Update
Route::put('/contacts/{id}', [\App\Http\Controllers\ContactController::class, 'update'])->name('contacts.update');
```

Form

```html
<form action="{{ route('contacts.update', $contact->id) }}" method="POST">
  @method('PUT') @csrf
  <div class="row">
    <div class="col-md-12">
      <div class="form-group row">
        <label for="name" class="col-md-3 col-form-label">Name</label>
        <div class="col-md-9">
          <input
            type="text"
            name="name"
            id="name"
            class="form-control @error('name') is-invalid @enderror"
            value="{{ old('name', $contact->name) }}"
          />
          @error('name')
          <div class="invalid-feedback">{{ $message }}</div>
          @enderror
        </div>
      </div>

      <div class="form-group row mb-0">
        <div class="col-md-9 offset-md-3">
          <button type="submit" class="btn btn-primary">
            {{ $contact->exists? 'Update' : 'Save' }}
          </button>
          <a
            href="{{ route('contacts.index') }}"
            class="btn btn-outline-secondary"
            >Cancel</a
          >
        </div>
      </div>
    </div>
  </div>
</form>
```

Model

```php
// senarai property untuk massive assign dari $request->all()
protected $fillable = ['name', 'email', 'phone', 'address', 'company_id'];
```

Controller

```php
<?php

public function edit($id)
{
    return view('contact.edit', ['contact' => Contact::findOrFail($id)]);
}

public function update(Request $request, $id)
{
    return $this->save($request, $id);
}

...

private function save(Request $request, $id = '')
{
    $request->validate([
        'name' => 'required',
        'email' => 'required | email',
        'address' => 'required',
        'company_id' => 'required|exists:companies,id'

    ]);

    if($id) {
        Contact::query()->find($id)->update($request->all());
        $msg = "Contact has been updated successfully.";
    }else{
        Contact::create($request->all());
        $msg = "Contact has been added successfully.";
    }

    return redirect()->route('contacts.index')->with('message', $msg);
}
```

## Create

View

```html
<a href="{{ route('contacts.create') }}">Create</a>
```

Route

```php
// Create
Route::get('/contacts/create', [ContactController::class, 'create'])->name('contacts.create');

// Store data
Route::post('/contacts/store', [\App\Http\Controllers\ContactController::class, 'store'])->name('contacts.store');
```

Letakkan meta di main layout (optional)

    <meta name="csrf-token" content="{{ csrf_token() }}">

Create form

```html
<form action="{{ route('contacts.store') }}" method="POST">
  // Insert hidden field @csrf // inlucde contact form view
  @include('contacts._form')
</form>
```

Model 

    protected $fillable = ['name', 'email', 'phone', 'address', 'company_id'];

Form 

Validation untuk INPUT 

```html 
<div class="row">
  <div class="col-md-12">
    <div class="form-group row">
      <label for="name" class="col-md-3 col-form-label">Name</label>
      <div class="col-md-9">
        <input
          type="text"
          name="name"
          id="name"
          class="form-control @error('name') is-invalid @enderror"
          value="{{ old('name') }}"
        />
        @error('name')
        <div class="invalid-feedback">{{ $message }}</div>
        @enderror
      </div>
    </div>
  </div>
</div>
````

Validation untuk SELECT

```html
<div class="form-group row">
    <label for="company_id" class="col-md-3 col-form-label">Company</label>
    <div class="col-md-9">
        <select name="company_id" id="company_id" class="form-control @error('company_id') is-invalid @enderror">
            @foreach($companies as $id => $name)
                <option {{ $id == old('company_id')? 'selected' : '' }} value="{{ $id }}">{{ $name }}</option>
            @endforeach
        </select>
            @error('company_id')
            <div class="invalid-feedback">
                {{ $message }}
            </div>
            @enderror
    </div>
</div>
```

Create controller untuk menyimpan rekod

```php
<?php
use Illuminate\Validation\Rule;

public function store(Request $request)
{
    // form rules
    $request->validate([
        'first_name' => 'required',
        // unique:table,column,ignore-record-id
        // 'code' => 'required|unique:book_types,code,' . $request->id,
        'code' => ['required', Rule::unique('book_types')->ignore($request->id)],
        'last_name' => 'required',
        'email' => 'required | email',
        'address' => 'required',
        'company_id' => 'required|exists:companies,id'

    ]);
    // $contact = new Contact();
    // $contact->create($requst->all());
    $contact = Contact::create($request->all());
    return redirect()->route('contacts.show', $contact->id)->with('msg', 'Success');

    // dd($request->all());
    // dd($request->only('first_name', 'last_name'));
    // dd($request->except('first_name', 'last_name'));
}
```

## Delete

1.  Delete using POST

    View

    ```html
    <form action="{{ route('phones.destroy', $phone->id) }}" method="POST" class="form-inline-block" id="frm-delete">
      @method('DELETE') @csrf
      <a href="{{ route('phones.edit', $phone->id) }}" class="btn btn-outline-primary">Edit</a>
      <a href="{{ session('prev-url') }}" class="btn btn-outline-primary">Back</a>
      <input type="submit" value="Delete" class="btn btn-outline-danger" />
    </form>
    ```

    Route

        Route::delete('phones/destroy', [PhoneController::class, 'destroy'])->name('phones.destroy)

    Controller

    ```php
    <?php
    public function destroy(Phone $phone)
    {
        $phone->delete();
        return redirect()->to(session('prev-url'))->with('message', 'Delete done');
    }
    ```

2.  Delete Using JS

    Route `web.php`

    ```php
    <?php
    Route::delete('/contacts/{id}', [\App\Http\Controllers\ContactController::class, 'delete'])->name('contacts.delete');
    ```

    Blade

    ```html
    <a
      href="{{ route('contacts.delete', $contact->id) }}"
      class="btn-delete btn btn-sm btn-circle btn-outline-danger"
      title="Delete"
      ><i class="fa fa-times"></i
    ></a>

    .....

    <form id="form-delete" method="POST" style="display: none">
      @method('DELETE') @csrf
    </form>
    ```

    JS `public\js\app.js`

    ```js
    document.querySelectorAll(".btn-delete").forEach((button) => {
      button.addEventListener("click", function (event) {
        event.preventDefault();
        if (confirm("Are you sure")) {
          let action = this.getAttribute("href");
          let form = document.getElementById("form-delete");
          form.setAttribute("action", action);
          form.submit();
        }
      });
    });
    ```

    Controller

    ```php
    <?php
    public function delete($id)
    {
        Contact::query()->findOrFail($id)->delete();
        return back()->with('message', 'Contact has been deleted successfully.');
    }
    ```



## Final Route

```php
<?php
// States
Route::get('state/index', [StateController::class, 'index'])->name('state.index');
Route::get('state/create', [StateController::class, 'create'])->name('state.create');
Route::get('state/{id}', [StateController::class, 'show'])->name('state.show');
Route::get('state/edit/{id}', [StateController::class, 'edit'])->name('state.edit');
Route::post('state/store', [StateController::class, 'store'])->name('state.store');
Route::put('state/{id}', [StateController::class, 'update'])->name('state.update');
Route::delete('state/{id}', [StateController::class, 'destroy'])->name('state.destroy');
```
