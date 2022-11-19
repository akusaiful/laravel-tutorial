### 1 - Configure mail

Configuration email berada di dalam fail `config/mail.php` dengan menggunakan fail `.env`.
Untuk tujuan kelas akan menggunakan konfigurasi berikut :

    MAIL_MAILER=smtp
    MAIL_HOST=smtp.hostinger.com
    MAIL_PORT=465
    MAIL_USERNAME=training2@danconsult.my
    MAIL_PASSWORD=qwertyuiOP25$$
    MAIL_ENCRYPTION=null
    MAIL_FROM_ADDRESS=training@danconsult.my
    MAIL_FROM_NAME="${APP_NAME}"

### 2 - Create Mail Sender

    php artisan make:mail MailSender

Masukkan public property `$data` 

```php
<?php

namespace App\Mail;

use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Mail\Mailable;
use Illuminate\Queue\SerializesModels;

class MailSender extends Mailable
{
    use Queueable, SerializesModels;

    public $data;

    /**
     * Create a new message instance.
     *
     * @return void
     */
    public function __construct($data)
    {
        $this->data = $data;
    }

    /**
     * Build the message.
     *
     * @return $this
     */
    public function build()
    {
        return $this->view('emails.test');
    }
}
```


### 3 - Create view fail

Create folder `emails` dalam `resources\views`. Namakan sebagai `test.blade.php`

```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>
    </head>
    <body>
        <h1>{{ $data['title'] }}</h1>
        <p>
            {{ $data['content'] }}
        </p>
    </body>
    </html>
```

### 4 - Create route 

```php 
<?php
Route::get('/email', function(){
    Mail::to('akusaiful@gmail.com')->send(new MailSender([
        'title' => 'test',
        'content' => 'content'
    ]));        
});
```

### 5 - Test Send email

Access ke `/email` dan email akan dihantar menggunakan template view yang telah dibuat. 

