## Latihan Penghasilan Page dan Penggunaan Link

Buat 3 page berikut menggunakan template bizland

- Page 1 : About Us
- Page 2 : Contact
- Page 3 : History

Guna routing berikut : 

1. /about-us
2. /contact
3. /history


** Step to create new page in laravel
1. Create Controller `HomeController.php` in `app/Http/Controllers` directory.
2. Define a method `aboutUs` that returns a view named `home.about-us`.
3. Ensure the `HomeController` extends the base `Controller` class.
4. The view `home.about-us` should be created in the `resources/views/home` directory.
5. The view should contain a simple message indicating that this is the About Us page.
6. Create '/about-us' route in web.php
