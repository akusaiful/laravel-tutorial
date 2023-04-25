Package repository `https://realrashid.github.io/sweet-alert/`

Install package

    composer require realrashid/sweet-alert

Masukkan dekat master `layout/main.blade.php` sebelum tag `</body>`

    @include('sweetalert::alert')

Publish alert to `views/vendor`

    php artisan sweetalert:publish

Masukkan ke `.env` 

    SWEET_ALERT_AUTO_DISPLAY_ERROR_MESSAGES=true
    SWEET_ALERT_ALWAYS_LOAD_JS=true

!!! info
    For more info sweetalert [https://sweetalert2.github.io/](https://sweetalert2.github.io/)
