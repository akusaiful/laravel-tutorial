Package Repository `https://github.com/spatie/laravel-web-tinker`

Install package

    composer require spatie/laravel-web-tinker --dev

Publish assets

    php artisan web-tinker:install

Publish config file (optional)

    php artisan vendor:publish --provider="Spatie\WebTinker\WebTinkerServiceProvider" --tag="config"

Visit `/tinker` in your local environment of your app to view the tinker page.