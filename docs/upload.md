

## 1. Set form to `enctype="multipart/form-data"`

```php
<?php
<form action="{{ route('profile.update') }}" method="POST" enctype="multipart/form-data">
    @csrf
    @method('PUT')     
```

## 2. Set input file

Set input file dalam form `accept="image/*` for client side validation

```html
<input type="file" name="profile_picture" accept="image/*">
```

## 3. Create Server side validation

Create validation dekat bahagian controlller atau masukkan di bahagian request class. 

```php
<?php
public function rules()
{
    return [
        'name' => 'required',
        'email' => 'required|email',
        'profile_picture' => ['nullable', 'mimes:png,jpg,bmp']
    ];
}
```

## 4. Create folder `public/upload` 

Create folder sebagai tempat lokasi upload file. Setkan write permision untuk folder tersebut.

## 5. Update model property `$fillable`

     protected $fillable = [
        'name',
        'email',
        'password',
        'profile_picture',
        'address',
        'bio'
    ];

## 6. Update controller

```php
<?php
$request->profile_picture->move(public_path('upload'), $filename = $request->user()->id . '-avatar.jpg');
$data = $request->validated();
$data['profile_picture'] = $filename;
$request->user()->update($data);
```

laksanakan ujian untuk test upload.

### Usefull method

1. `$request->profile_picture->getClientOriginalName()` - return the original name of the uploaded file
2. `$request->profile_picture->getClientOriginalExtension()` - return extension
3. `$request->profile_picture->getClientSize()` - file size in byte
4. `$request->profile_picture->getClientMimeType()` - mime type
