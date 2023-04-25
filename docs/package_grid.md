Grid repository

    https://github.com/itstructure/laravel-grid-view

!!! Info
    For more info [https://github.com/itstructure/laravel-grid-view](https://github.com/itstructure/laravel-grid-view)

Install menggunakan composer

    composer require itstructure/laravel-grid-view "~1.0.11"

Register service provider in config/app.php

    Itstructure\GridView\GridViewServiceProvider::class,

Pindahkan asset ke layouts/views/vendor bagi membolehkan modification dibuat ke atas grid

    php artisan grid_view:publish --only=views

Paparan views/contract/index

````php 
<?php
    @php
    $gridData = [
        'dataProvider' => $dataProvider,
        'title' => 'Panel title',
        'useFilters' => false,
        'columnFields' => [
            'id',
            'active',
            'icon',
            'created_at'
        ]
    ];
    @endphp

    @gridView($gridData)
````

Untuk menggunakan action button

```php
<?php 
@gridView([
'dataProvider' => $dataProvider,
'columnFields' => [
    [
        'label' => 'Actions', // Optional
        'class' => Itstructure\GridView\Columns\ActionColumn::class, // Required
        'actionTypes' => [ // Required
            'view',
            'edit' => function ($data) {
                return '/admin/pages/' . $data->id . '/edit';
            },
            [
                    'class' => Itstructure\GridView\Actions\Delete::class, // Required
                    'url' => function ($data) {
                        // Optional
                        return route('profile.destroy', $data);
                        //return '/profile/' . $data->id . '/delete';
                    },
                    'htmlAttributes' => [
                        // Optional
                        // 'target' => '_blank',
                        //'onclick' => 'confirmGridDelete()',
                        'class' => 'btn btn-danger btn-delete'
                        // 'style' => 'color: yellow; font-size: 16px;',
                        // 'onclick' => 'return window.confirm("Are you sure you want to delete?");',
                    ],
                ],
        ]
    ]
]
])
```

Modified `views/vendor/grid_view/actions/delete.blade.php` (remove `class` dekat `<a>` sahaja)

````php 
<?php
<div class="col-lg-{!! $bootstrapColWidth !!}">
<a href="{!! $url !!}" @if(!empty($htmlAttributes)) {!! $htmlAttributes !!} @endif >
    <svg width="1.2em" height="1.5em" viewBox="0 0 16 16" class="bi bi-trash" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
        <path d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6z"/>
        <path fill-rule="evenodd" d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1zM4.118 4L4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118zM2.5 3V2h11v1h-11z"/>
    </svg>
</a>&nbsp
</div>
<div class="d-lg-none">&nbsp</div>
````

Masukkan hidden `form` dan `querySelectorAll` di `views/vendor/grid_view/grid.blade.php`. Letak sebelum tag `<script>`

````php 
<?php
    ...

    <form style="display: none" id="form-delete" method="POST">
    @csrf
    @method('DELETE')
    </form>

    <script type="text/javascript">
        document.addEventListener("DOMContentLoaded", function() {
            $('#grid_view_checkbox_main').click(function(event) {
                $('input[role="grid-view-checkbox-item"]').prop('checked', event.target.checked);
            });

            $('#grid_view_search_button').click(function() {
                $('#grid_view_filters_form').submit();
            });

            $('#grid_view_reset_button').click(function() {
                $('input[role="grid-view-filter-item"]').val('');
                $('select[role="grid-view-filter-item"]').prop('selectedIndex', 0);
            });
        });

        document.querySelectorAll('.btn-delete').forEach((button) => {
            button.addEventListener('click', function(event) {
                event.preventDefault();
                if (confirm('Delete?')) {
                    let action = this.getAttribute('href');
                    let form = document.getElementById('form-delete');
                    form.setAttribute('action', action);
                    form.submit();
                }           
            });
        });
    </script>
````

Add `routes/web.php` 

    Route::delete('/profile/{id}/delete', [ProfileController::class, 'destroy'])->name('profile.destroy');

Tukar script js jika menggunakn sweetalert seperti berikut : 

````php
<?php
    <form style="display: none" id="form-delete" method="POST">
    @csrf
    @method('DELETE')
    </form>

    <script type="text/javascript">
        document.addEventListener("DOMContentLoaded", function() {
            $('#grid_view_checkbox_main').click(function(event) {
                $('input[role="grid-view-checkbox-item"]').prop('checked', event.target.checked);
            });

            $('#grid_view_search_button').click(function() {
                $('#grid_view_filters_form').submit();
            });

            $('#grid_view_reset_button').click(function() {
                $('input[role="grid-view-filter-item"]').val('');
                $('select[role="grid-view-filter-item"]').prop('selectedIndex', 0);
            });
        });

        document.querySelectorAll('.btn-delete').forEach((button) => {
            button.addEventListener('click', function(event) {
                event.preventDefault();
                Swal.fire({
                    title: 'Are you sure?',
                    icon: 'warning',
                    text: "You won't be able to revert this!",
                    type: 'warning',
                    showCancelButton: true,
                    confirmButtonColor: '#3085d6',
                    cancelButtonColor: '#d33',
                    confirmButtonText: 'Yes, delete it!'
                }).then((result) => {
                    if (result.value) {
                        let action = this.getAttribute('href');
                        let form = document.getElementById('form-delete');
                        form.setAttribute('action', action);
                        form.submit();
                    }
                });
            });
        });
    </script>
````