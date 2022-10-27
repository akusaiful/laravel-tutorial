## Classic Method

Set form to `enctype="multipart/form-data"`

```php
<?php
<form action="{{ route('profile.update') }}" method="POST" enctype="multipart/form-data">
    @csrf
    @method('PUT')     
```
input file dalam form, `accept="image/*` for client side validation

```html
<input type="file" name="profile_picture" accept="image/*">
```

Server side validation, boleh letak dkt controller atau `$request`

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

Create folder in `public/upload` to move location 

Update model property `$fillable`

Update controller

```php
<?php
$request->profile_picture->move(public_path('upload'), $filename = $request->user()->id . '-avatar.jpg');
$data = $request->validated();
$data['profile_picture'] = $filename;
$request->user()->update($data);
```

Usefule method uploadfile class provided

1. `$request->profile_picture->getClientOriginalName()` - return the original name of the uploaded file
2. `$request->profile_picture->getClientOriginalExtension()` - return extension
3. `$request->profile_picture->getClientSize()` - file size in byte
4. `$request->profile_picture->getClientMimeType()` - mime type
