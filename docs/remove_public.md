Rename `server.php` to `index.php`

Copy `public/.htaccess` to root and paste below code

    Options -MultiViews -Indexes
    RewriteEngine On
    # Handle Authorization Header
    RewriteCond %{HTTP:Authorization} .
    RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
    # Redirect Trailing Slashes If Not A Folder...
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_URI} (.+)/$
    RewriteRule ^ %1 [L,R=301]
    # Handle Front Controller...
    RewriteCond %{REQUEST_URI} !(\.css|\.js|\.png|\.jpg|\.gif|robots\.txt)$ [NC]
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^ index.php [L]
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_URI} !^/public/
    RewriteRule ^(css|js|img)/(.*)$ public/$1/$2 [L,NC]

Note : all css to image background mesti menggunakan path seperti berikut : 

    #top {    
        background: url('../img/background3.jpg') no-repeat center;    
    }

!!!Reference

    [https://medium.com/@laraveltuts/laravel-9-remove-public-from-url-using-htaccess-527f22961556](https://medium.com/@laraveltuts/laravel-9-remove-public-from-url-using-htaccess-527f22961556)