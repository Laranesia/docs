<!-- # Installation -->
# Pemasangan

<!--
- [Install Composer](#install-composer)
- [Install Laravel](#install-laravel)
- [Server Requirements](#server-requirements)
- [Configuration](#configuration)
- [Pretty URLs](#pretty-urls)
-->
- [Memasang Composer](#install-composer)
- [Memasang Laravel](#install-laravel)
- [Server Requirements](#server-requirements)
- [Configuration](#configuration)
- [Pretty URLs](#pretty-urls)

<a name="install-composer"></a>
<!-- ## Install Composer -->
## Memasang Composer

<!-- Laravel utilizes [Composer](http://getcomposer.org) to manage its dependencies. First, download a copy of the `composer.phar`. Once you have the PHAR archive, you can either keep it in your local project directory or move to `usr/local/bin` to use it globally on your system. On Windows, you can use the Composer [Windows installer](https://getcomposer.org/Composer-Setup.exe). -->
Laravel menggunakan [Composer](http://getcomposer.org) untuk mengelola dependensinya. Pertama, unduh salinan `composer.phar`. Setelah Anda memiliki arsip PHAR, Anda dapat menyimpannya dalam map proyek lokal atau dipindahkan ke `usr/local/bin` untuk penggunaan global pada sistem Anda. Pada Windows, Anda dapat menggunakan Composer [Windows *installer*](https://getcomposer.org/Composer-Setup.exe).

<a name="install-laravel"></a>
<!-- ## Install Laravel -->
## Memasang Laravel

<!-- ### Via Composer Create-Project -->
### Dengan Cara Composer Create-Project

<!-- You may install Laravel by issuing the Composer `create-project` command in your terminal: -->
Anda dapat memasang Laravel dengan mengeluarkan Composer dengan perintah `create-project` di dalam *terminal*:

	composer create-project laravel/laravel --prefer-dist

<!-- ### Via Download -->
### Dengan Cara Unduh

<!-- Once Composer is installed, download the [latest version](https://github.com/laravel/laravel/archive/master.zip) of the Laravel framework and extract its contents into a directory on your server. Next, in the root of your Laravel application, run the `php composer.phar install` (or `composer install`) command to install all of the framework's dependencies. This process requires Git to be installed on the server to successfully complete the installation. -->
Setelah Composer terpasang, unduh [versi terbaru](https://github.com/laravel/laravel/archive/master.zip) *framework* Laravel dan ekstrak isinya ke dalam map pada peladen Anda. Selanjutnya, di *root* aplikasi Laravel, jalankan perintah `php composer.phar install` (atau `composer install`) untuk memasang semua depedensi *framework*. Untuk menyelesaikan proses pemasangan ini, dibutuhkan Git terpasang pada peladen.

<!-- If you want to update the Laravel framework, you may issue the `php composer.phar update` command. -->
Jika Anda ingin memperbarui *framework* Laravel, Anda dapat menggunakan perintah `php composer.phar update`.

<a name="server-requirements"></a>
<!-- ## Server Requirements -->
## Persyaratan Peladen

<!-- The Laravel framework has a few system requirements: -->
*Framework* Laravel memiliki beberapa persyaratan sistem:

- PHP >= 5.3.7
- Ekstensi PHP MCrypt

<a name="configuration"></a>
<!-- ## Configuration -->
## Pengaturan

<!-- Laravel needs almost no configuration out of the box. You are free to get started developing! However, you may wish to review the `app/config/app.php` file and its documentation. It contains several options such as `timezone` and `locale` that you may wish to change according to your application. -->
Laravel hampir tidak membutuhkan pengaturan. Anda bebas untuk memulai membangun! Namun, Anda mungkin ingin menilik berkas `app/config/app.php` dan dokumentasinya. Di dalamnya berisi beberapa pengaturan seperti `timezone` dan `locale`, mungkin saja Anda ingin mengubahnya sesuai dengan kebutuhan aplikasi Anda.

<a name="permissions"></a>
<!-- ### Permissions -->
### Izin

<!-- Laravel requires one set of permissions to be configured - folders within app/storage require write access by the web server. -->
Laravel membutuhkan sepaket izin yang perlu diatur - map dalam `app/storage` membutuhkan akses tulis oleh peladen web.

<a name="paths"></a>
<!-- ### Paths -->
### Jalur (Paths)

<!-- Several of the framework directory paths are configurable. To change the location of these directories, check out the `bootstrap/paths.php` file. -->
Beberapa jalur map *framework* dapat diubahsuai. Untuk mengubah lokasi map ini, silakan periksa berkas `bootstrap/paths.php`.

<!-- > **Note:** Laravel is designed to protect your application code, and local storage by placing only files that are necessarily public in the public folder.  It is recommended that you either set the public folder as your site's documentRoot (also known as a web root) or to place the contents of public into your site's root directory and place all of Laravel's other files outside the web root.  -->
> **Catatan:** Laravel dirancang untuk melindungi kode aplikasi Anda, dengan menempatkan berkas yang diakses umum hanya dalam map public (*local storage*). Anda juga disarankan mengatur map public sebagai *documentRoot* situs Anda (juga dikenal sebagai *web root*), atau dengan menempatkan isi dari public ke map *root* situs Anda dan tempatkan semua berkas Laravel yang lain di luar *web root*.

<a name="pretty-urls"></a>
<!-- ## Pretty URLs -->
## URLs Bersih

<!-- The framework ships with a `public/.htaccess` file that is used to allow URLs without `index.php`. If you use Apache to serve your Laravel application, be sure to enable the `mod_rewrite` module. -->
Laravel dikemas bersama sebuah berkas `public/.htaccess` yang digunakan untuk memungkinkan URL tanpa `index.php`. Jika Anda menggunakan Apache untuk melayani aplikasi Laravel Anda, pastikan untuk mengaktifkan modul `mod_rewrite`.

<!-- If the `.htaccess` file that ships with Laravel does not work with your Apache installation, try this one: -->
Jika `.htaccess` berkas yang dikemas bersama Laravel tidak bekerja dengan instalasi Apache Anda, coba yang ini:

	Options +FollowSymLinks
	RewriteEngine On

	RewriteCond %{REQUEST_FILENAME} !-d
	RewriteCond %{REQUEST_FILENAME} !-f
	RewriteRule ^ index.php [L]
