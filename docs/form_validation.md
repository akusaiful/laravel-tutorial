# Input validation

## Method Controller

Validation rules using method  controller

```php
<?php 
protected function validationRules(){
    return [
        'first_name' => 'required',
        'last_name' => 'required',
        'email' => 'required | email',
        'address' => 'required',
        'company_id' => 'required|exists:companies,id'
    ];
}
```

Call method from `save()`

```php
<?php 
private function save(Request $request, $id = '')
{
    $request->validate($this->validationRules());
    ..
    ..
}
```

## Request Class 

Validation using extended `FormRequest`

Create request object

    php artisan make:request ModelRequest

Customize request class

```php 
<?php 
namespace App\Http\Requests;
use Illuminate\Foundation\Http\FormRequest;
use Illuminate\Validation\Rule;
class ModelRequest extends FormRequest
{
    
    public function authorize()
    {
        return true;
    }
    
    public function rules()
    {
        return [
            // sample validation
            'code' => ['required', Rule::unique('book_types')->ignore($this->id)],
            'name' => 'required',
            'price' => 'required|numeric' 
            // 'name' => ['required','string','max:255']               
        ];
    }
}
```

Customize error message and attribute 

```php 
<?php 
/**
 * Customize attribute name
 */
public function attributes()
{
    return [            
        'name' => 'name',
        'price' => 'price'            
    ];        
}

/**
 * Customize error message
 */
public function messages()
{
    return [
        'required' => 'The :attribute field can not be blank',
        'price.numeric' => 'Masukkan number sahaja',
        'price.required' => 'Harga diperlukan ya tn/pn'
    ];
}
```

Boleh juga call `$this->route('contact')` atau `$this->method()` dari dalam object request untuk dapatkan information berkaitan request atau object yang akan dikemaskini berdasarkan kepada model binding dekat route.    