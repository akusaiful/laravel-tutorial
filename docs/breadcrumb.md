# Breadcrumb

Breadcumb adalah navigation dibahagian atas setiap page yang memberikan bantuan navigasi. Ia akan membantu user untuk memahami dimanakah mereka berada.

Laksana arahan berikut

    php artisan make:component Breadcrumb

akan create file di lokasi berikut `app\View\Components\Breadcrumb.php` dan `resources\views\components\bredcrumb.blade.php`

Masukkan code berikut `app\View\Components\Breadcrumb.php`

````php
<?php 

namespace App\View\Components;

use DOMDocument;
use Illuminate\View\Component;

class Breadcrumb extends Component
{
    const HOME = 'home';

    public $breadStack = [];

    public $type;

    /**
     * Create a new component instance.
     *
     * @return void
     */
    public function __construct($type = '')
    {
        $this->breadStack = session()->get('breadstack', []);

        if ($type == 'root') {
            $this->breadStack = [];
        }
    }

    public function stack($slot)
    {
        $newStack = [];

        // Create a new DOM Document
        $xml = new DOMDocument();

        // Load the html contents into the DOM
        $xml->loadHTML($slot);

        // Empty array to hold all links to return
        $stack = array();

        //Loop through each <li> tag in the dom
        foreach ($xml->getElementsByTagName('li') as $li) {
            //Loop through each <a> tag within the li, then extract the node value
            if (!$li->getElementsByTagName('a')->item(0)) {
                $stack[url()->current()] = $this->item($li->nodeValue, url()->current());
            } else {
                foreach ($li->getElementsByTagName('a') as $links) {
                    $stack[$links->getAttribute('href')] = $this->item($links->nodeValue, $links->getAttribute('href'));
                }
            }
        }


        $tmpStack = array_merge($this->breadStack, $stack);
        foreach ($tmpStack as $key => $val) {
            $newStack[$key] = $val;
            $breadcrumb[$key] = $val;

            if (url()->current() == $key) {
                $breadcrumb[$key] = $this->item(strip_tags($val));
                break;
            }
        }

        session()->put('breadstack', $newStack);

        return implode($breadcrumb);
    }

    /**
     * Get the view / contents that represent the component.
     *
     * @return \Illuminate\Contracts\View\View|\Closure|string
     */
    public function render()
    {
        return view('components.breadcrumb');
    }

    private function item($item, $link = '')
    {
        if ($link) $item = "<a href='$link'>$item</a>";
        return "<li class='breadcrumb-item'>$item</li>";
    }
}
````

Masukkan code berikut di `resources\views\components\bredcrumb.blade.php`

````php 
<?php 
<ol {{ $attributes->merge([
    'class' => 'breadcrumb page-breadcrumb'
    ]) }}>
    <li class="breadcrumb-item"><a href="{{ route('home') }}">Home</a></li>
    {!! $stack($slot) !!}
    <li class="position-absolute pos-top pos-right d-none d-sm-block"><span class="js-get-date"></span></li>
</ol>
````

## Penggunaan 

Masukkan breadcrumb component di permulaan page contoh `index`  seperti berikut : 

````php
<?php 
<x-breadcrumb type="root">
    <li>Senarai Kontrak</li>
</x-breadcrumb>
````

Untuk page kedua selepas page `index`, `type="root"` tidak diperlukan :

````php
<?php
<x-breadcrumb>
    <li class="breadcrumb-item">Page 2</li>
</x-breadcrumb>
````

Apabila `page 2` diakses selepas page index breadcrumb akan menunjukkan navigation seperti berikut : 

    Home > Senarai Kontrak > Page 2


