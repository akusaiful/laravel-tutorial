### Controller 

```php
<?php

namespace App\Http\Controllers;

use App\Models\State;
use Illuminate\Http\Request;

class StateController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        session(['url.paginate' => url()->full()]);
        $states = State::paginate(8);
        return view('state.index')->with('states', $states);
    }

    /**
     * Show the form for creating a new resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function create()
    {
        return view('state.create')->with('state', new State());
    }

    /**
     * Store a newly created resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return \Illuminate\Http\Response
     */
    public function store(Request $request)
    {
        return $this->save($request);
    }

    /**
     * Display the specified resource.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function show($id)
    {
        return view('state.show')->with('state', State::find($id));
    }

    /**
     * Show the form for editing the specified resource.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function edit($id)
    {
        return view('state.edit')->with('state', State::find($id));
    }

    /**
     * Update the specified resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function update(Request $request, $id)
    {
        return $this->save($request);
    }

    /**
     * Remove the specified resource from storage.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function destroy($id)
    {
        State::destroy($id);
        return redirect()->route('state.index')->with('message','Deleted!');
    }

    public function save(Request $request)
    {
        $request->validate([
            'code' => 'required',
            'name' => 'required'
        ]);

        if($request->id){
            ($state = State::find($request->id))->update($request->all());
        }else{
            $state = State::create($request->all());
        }

        return redirect()->route('state.show', $state->id);
    }
}

```

### Model 

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class State extends Model
{
    use HasFactory;

    protected $fillable = ['code', 'name'];

    public $timestamps = false;

    public function warehouses()
    {
        return $this->hasMany(Warehouse::class);
    }

    public function getName()
    {
        return $this->id . ' - ' . $this->name;
    }

}

```

## View

### Index

```html
<div class="'Expressway Companies'-index">
  <p class="text-right">
    <a
      class="btn btn-outline-primary waves-effect waves-themed"
      href="{{ route('state.create') }}"
      >Create</a
    >
  </p>

  @if($message = session()->get('message'))
  <div class="alert bg-primary-200 text-white">{{ $message }}</div>
  @endif

  <div
    id="p0"
    data-pjax-container=""
    data-pjax-push-state=""
    data-pjax-timeout="1000"
  >
    <div id="w0" class="table-responsive">
      <table class="table table-striped table-bordered table-hover">
        <thead>
          <tr class="thead-dark">
            <th width="2%">ID</th>
            <th width="10%">Code</th>
            <th width="80%">Name</th>

            <th class="action-column">&nbsp;</th>
          </tr>
          <tr id="w0-filters" class="filters">
            <td>
              <input
                type="text"
                class="form-control"
                name="ExpresswayCompanySearch[id]"
              />
            </td>
            <td>
              <input
                type="text"
                class="form-control"
                name="ExpresswayCompanySearch[name]"
              />
            </td>
            <td>
              <input
                type="text"
                class="form-control"
                name="ExpresswayCompanySearch[address]"
              />
            </td>
            <td>&nbsp;</td>
          </tr>
        </thead>
        <tbody>
          @foreach($states as $type)
          <tr data-key="1">
            <td style="text-align:left">{{ $type->id }}</td>
            <td>{{ $type->code }}</td>
            <td>{{ $type->name }}</td>
            <td width="5%" style="text-align:center" nowrap="nowrap">
              <a
                href="{{ route('state.show', $type->id) }}"
                title="View"
                aria-label="View"
                data-pjax="0"
                ><span class="fal fa-search"></span
              ></a>
              <a
                href="{{ route('state.edit', $type->id) }}"
                title="Update"
                aria-label="Update"
                data-pjax="0"
                ><span class="fal fa-pencil"></span
              ></a>
              <a
                href="{{ route('state.destroy', $type->id) }} "
                title="Delete"
                class="btn-delete"
                ><span class="fal fa-trash-alt"></span
              ></a>
            </td>

            <form style="display: none" id="form-delete" method="POST">
              @csrf @method('DELETE')
            </form>
          </tr>
          @endforeach
        </tbody>
      </table>
    </div>
  </div>
  {{ $states->links() }}
</div>
```

### Create Form

```html
<div id="panel-1" class="panel">
  <div class="panel-hdr">
    <h2>
      Create <span class="fw-300"><i>Book Type</i></span>
    </h2>
    <div class="panel-toolbar">
      <button
        class="btn btn-panel waves-effect waves-themed"
        data-action="panel-collapse"
        data-toggle="tooltip"
        data-offset="0,10"
        data-original-title="Collapse"
      ></button>
      <button
        class="btn btn-panel waves-effect waves-themed"
        data-action="panel-fullscreen"
        data-toggle="tooltip"
        data-offset="0,10"
        data-original-title="Fullscreen"
      ></button>
      <button
        class="btn btn-panel waves-effect waves-themed"
        data-action="panel-close"
        data-toggle="tooltip"
        data-offset="0,10"
        data-original-title="Close"
      ></button>
    </div>
  </div>
  <div class="panel-container show">
    <div class="panel-content">
      <div class="panel-tag">
        Be sure to use an appropriate type attribute on all inputs (e.g., code
        <code>email</code> for email address or <code>number</code> for
        numerical information) to take advantage of newer input controls like
        email verification, number selection, and more.
      </div>

      <form action="{{ route('state.store') }}" method="POST">
        @csrf
        @include('state._form')
      </form>
    </div>
  </div>
</div>
```

### Partial _form.blade.php

```html
<div class="form-group">
    <label class="form-label" for="simpleinput">Code</label>
    <input type="text" id="code" name="code" class="form-control @error('code') is-invalid @enderror" value="{{ old('code', $state->code) }}">
    @error('code')
    <div class="invalid-feedback">
        {{ $message }}
    </div>

    @enderror
</div>
<div class="form-group">
    <label class="form-label" for="example-email-2">Name</label>
    <input type="text" id="name" name="name" class="form-control @error('name') is-invalid @enderror" placeholder="Name" value="{{ old('name', $state->name) }}">
    @error('name')
    <div class="invalid-feedback">
    {{ $message }}
    </div>
    @enderror
</div>
<div class="text-right">
    <a href="{{ session()->get('url.paginate', route('state.index')) }}" class="btn btn-outline-primary">Cancel</a>
    <button type="submit" class="btn btn-primary waves-effect waves-themed">Update</button>
</div>
```

### Update Form

```html
@extends('layouts.switch')

@section('content')
<div id="panel-1" class="panel">
    <div class="panel-hdr">
        <h2>
            Edit <span class="fw-300"><i>{{ $state->getName() }}</i></span>
        </h2>
        <div class="panel-toolbar">
            <button class="btn btn-panel waves-effect waves-themed" data-action="panel-collapse" data-toggle="tooltip" data-offset="0,10" data-original-title="Collapse"></button>
            <button class="btn btn-panel waves-effect waves-themed" data-action="panel-fullscreen" data-toggle="tooltip" data-offset="0,10" data-original-title="Fullscreen"></button>
            <button class="btn btn-panel waves-effect waves-themed" data-action="panel-close" data-toggle="tooltip" data-offset="0,10" data-original-title="Close"></button>
        </div>
    </div>
    <div class="panel-container show">
        <div class="panel-content">
            <div class="panel-tag">
                Be sure to use an appropriate type attribute on all inputs (e.g., code <code>email</code> for email address or <code>number</code> for numerical information) to take advantage of newer input controls like email verification, number selection, and more.
            </div>
            <form action="{{ route('state.update', $state->id) }}" method="POST">
                @method('PUT')
                @csrf
                @include('state._form')
            </form>
        </div>
    </div>
</div>
@endsection
```

### Show 

```html
@extends('layouts.switch')

@section('content')
<div id="panel-1" class="panel">
    <div class="panel-hdr">
        <h2>{{ $state->getName() }}</h2>
        <div class="panel-toolbar">
            <button class="btn btn-panel waves-effect waves-themed" data-action="panel-collapse" data-toggle="tooltip" data-offset="0,10" data-original-title="Collapse"></button>
            <button class="btn btn-panel waves-effect waves-themed" data-action="panel-fullscreen" data-toggle="tooltip" data-offset="0,10" data-original-title="Fullscreen"></button>
            <button class="btn btn-panel waves-effect waves-themed" data-action="panel-close" data-toggle="tooltip" data-offset="0,10" data-original-title="Close"></button>
        </div>
    </div>
    <div class="panel-container show">
        <div class="panel-content">
            <div class="panel-tag">
                Using the most basic table markup, here’s how <code>.table</code>-based tables look in SmartAdmin. You can inverse a table by using the class <code>.table-dark</code>
            </div>
            
            <div class="frame-wrap">
                <table class="table m-0">
                    <thead>
                        <tr>
                            <th width='10%'>NO</th>
                            <th>INFO</th>                            
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <th scope="row">ID</th>
                            <td>{{ $state->id }}</td>

                        </tr>
                        <tr>
                            <th scope="row">Code</th>
                            <td>{{ $state->code }}</td> 

                        </tr>
                        <tr>
                            <th scope="row">state</th>
                            <td>{{ $state->name }}</td>

                        </tr>                        

                    </tbody>
                </table>
            </div>

            <div class="text-right">
            <a href="{{ session()->get('url.paginate', route('state.index')) }}" class="btn btn-outline-primary">List</a>
            <a href="{{ route('state.edit', ['id' => $state->id] ) }}" class="btn btn-outline-primary">Update</a>
            </div>
            
        </div>
    </div>
</div>

@endsection
```

## Route

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