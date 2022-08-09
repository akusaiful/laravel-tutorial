# Form validation

## Create

[Subscribe to our newsletter](#){ .md-button .md-button--primary }

Route

    Route::post('/contacts/store', [\App\Http\Controllers\ContactController::class, 'store'])->name('contacts.store');

Letakkan meta di main layout 

    <meta name="csrf-token" content="{{ csrf_token() }}">

Create form

    <form action="{{ route('contacts.store') }}" method="POST">
        // Insert hidden field 
        @csrf

        // inlucde contact form view
        @include('contacts._form')
    </form>

Model

    protected $fillable = ['first_name', 'last_name', 'email', 'phone', 'address', 'company_id'];

Validation untuk INPUT

    <div class="row">
    <div class="col-md-12">
        <div class="form-group row">
            <label for="first_name" class="col-md-3 col-form-label">First Name</label>
            <div class="col-md-9">
                <input type="text" name="first_name" id="first_name"
                       class="form-control @error('first_name') is-invalid @enderror" value="{{ old('first_name') }}">
                @error('first_name')
                <div class="invalid-feedback">
                    {{ $message }}
                </div>
                @enderror
            </div>
        </div>
    </div>
    </div>

Validation untuk SELECT

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

Create controller untuk menyimpan rekod 

    public function store(Request $request)
    {
        // form rules
        $request->validate([
            'first_name' => 'required',
            'last_name' => 'required',
            'email' => 'required | email',
            'address' => 'required',
            'company_id' => 'required|exists:companies,id'

        ]);
        // dd($request->all());
        // dd($request->only('first_name', 'last_name'));
        // dd($request->except('first_name', 'last_name'));
    }

Masukkan validation di dalam form 

    <input type="text" name="first_name" id="first_name" class="form-control @error('first_name') is-invalid @enderror" value="{{ old('first_name') }}">
    
    @error('first_name')
    <div class="invalid-feedback">
        {{ $message }}
    </div>
    @enderror

## View

Route 

    Route::get('/contacts/{id}', [\App\Http\Controllers\ContactController::class, 'view'])->name('contacts.view');

Blade - link to route

    <a href="{{ route('contacts.view', $contact->id) }}" class="btn btn-sm btn-circle btn-outline-info" title="Show"><i class="fa fa-eye"></i></a>
    

Controller

    public function view($id)
    {
        $contact = \App\Models\Contact::findOrFail($id);
        return view('contacts.view', compact('contact'));
    }

Blade - view record

    <div class="form-group row">
        <label for="phone" class="col-md-3 col-form-label">Phone</label>
        <div class="col-md-9">
        <p class="form-control-plaintext text-muted">{{ $contact->phone }}</p>
        </div>
    </div>


## Update 

Route

    Route::put('/contacts/update/{id}', [\App\Http\Controllers\ContactController::class, 'update'])->name('contacts.update');

Blade 

    <form action="{{ route('contacts.update', $contact->id) }}" method="POST">
        @method('PUT')
        @csrf
        @include('contacts._form')
    </form>


Controller

    public function update($id, Request $request)
    {
        return $this->save($request, $id);
    }

    ...

    private function save(Request $request, $id = '')
    {
        $request->validate([
            'first_name' => 'required',
            'last_name' => 'required',
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


## Delete record (Using JS style)

Route !#web.php

    Route::delete('/contacts/{id}', [\App\Http\Controllers\ContactController::class, 'delete'])->name('contacts.delete');

Blade

    <a href="{{ route('contacts.delete', $contact->id) }}" class="btn-delete btn btn-sm btn-circle btn-outline-danger" title="Delete"><i class="fa fa-times"></i></a>

    .....

    <form id="form-delete" method="POST" style="display: none">
    @method('DELETE')
    @csrf
    </form>

JS app.js

    document.querySelectorAll('.btn-delete').forEach((button) => {
        button.addEventListener('click', function (event) {
            event.preventDefault();
            if(confirm("Are you sure")){
                let action = this.getAttribute('href');
                let form = document.getElementById('form-delete');
                form.setAttribute('action', action);
                form.submit();
            }
        });
    });

Controller

    public function delete($id)
    {
        Contact::query()->findOrFail($id)->delete();
        return back()->with('message', 'Contact has been deleted successfully.');
    }


## Search

Blade

    <form>
        <div class="row">
            <div class="col">
                <select id="filter_company_id" class="custom-select">
                    @foreach($companies as $id=>$name)
                    <option {{ $id == request('company_id')? 'selected' : '' }} value="{{ $id }}">{{ $name }}</option>
                    @endforeach

                </select>
            </div>
            <div class="col">
                <div class="input-group mb-3">
                    <input type="text" name="query_text" id="query_text" class="form-control" placeholder="Search..."
                            aria-label="Search..." aria-describedby="button-addon2" value="{{ request('query_text') }}">
                    <div class="input-group-append">
                        <button class="btn btn-outline-secondary" type="button">
                            <i class="fa fa-refresh"></i>
                        </button>
                        <button class="btn btn-outline-secondary" type="submit"
                                id="button-addon2">
                            <i class="fa fa-search"></i>
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </form>


Controller

    $contacts = Contact::query()->orderBy('first_name', 'asc')->where(function ($query){
        if($companyId = \request('company_id')){
            $query->where('company_id', $companyId);
        }

        if($queryText = \request('query_text')){
            $query->where('first_name', 'LIKE', "%{$queryText}%");
        }
    })->paginate(8);