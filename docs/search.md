## Search

View - Search Form

```html
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
```

Controller

```php
<?php
public function index()
{
    $phones = Phone::query()->orderBy('name', 'asc')->where(function ($query){
        if($companyId = \request('company_id')){
            $query->where('company_id', $companyId);
        }

        if($queryText = \request('query_text')){
            $query->where('name', 'LIKE', "%{$queryText}%");
        }
    })->paginate(8);

    return view('phones.index', ['phones' => $phones]);
}
```

View - Display search result

```html
<table class="table table-bordered table-condensed table-hover">
  <thead class="thead-dark">
    <tr>
      <th scope="col">Phone Name</th>
      <th>Number</th>
      <th>Active</th>
      <th></th>
    </tr>
  </thead>
  @forelse ($phones as $phone)
  <tr>
    <td>{{ $phone->name }}</td>
    <td>{{ $phone->number }}</td>
    <td>{{ $phone->is_deleted }}</td>
    <td style="width: 20%">
      <form action="{{ route('phones.destroy', $phone->id) }}" method="POST">
        @method('DELETE') @csrf
        <a
          href="{{ route('phones.show', $phone->id) }}"
          class="btn btn-outline-primary"
          >View</a
        >
        <a
          href="{{ route('phones.edit', $phone->id) }}"
          class="btn btn-outline-primary"
          >Edit</a
        >
        <button type="submit" class="btn btn-outline-danger">Delete</button>
      </form>
    </td>
  </tr>
  @empty
  <tr>
    <td colspan="3">Tiada rekod</td>
  </tr>
  @endforelse
</table>
{{ $phones->appends(['query' => $query ])->links() }}
```
