## Artisan - Laravel command-line interface

Publish vendor 

    php artisan vendor:publish

Serve using port

    php artisan serve --port=8282

Senaraikan semua route 

    php artisan route:list  
    php artisan route:list --compact 
    php artisan route:list --name=logout

    # untuk lihat yang path api (api route)
    php artisan route:list --name=contacts --path=api

Clear config cache `env`. Jika modified environment

    php artisan config:clear

Check Laravel framework version

    php artisan --version

Link to storage (create sym link)

    php artisan storage:link




