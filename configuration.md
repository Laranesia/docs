# Configuration

- [Pengantar](#introduction)
- [Environment Configuration](#environment-configuration)
- [Maintenance Mode](#maintenance-mode)

<a name="introduction"></a>
<!-- ## Introduction -->
## Pengantar

<!-- All of the configuration files for the Laravel framework are stored in the `app/config` directory. Each option in every file is documented, so feel free to look through the files and get familiar with the options available to you. -->
Semua file-file konfigurasi untuk kerangka Laravel disimpan dalam `app / config` direktori. Setiap pilihan dalam setiap file didokumentasikan, sehingga merasa bebas untuk melihat melalui file dan mendapatkan akrab dengan pilihan yang tersedia untuk Anda.

<!-- Sometimes you may need to access configuration values at run-time. You may do so using the `Config` class: -->
Kadang-kadang Anda mungkin perlu untuk mengakses nilai konfigurasi pada saat run-time. Anda dapat melakukannya dengan menggunakan `kelas` Config:

<!-- **Accessing A Configuration Value** -->
**Mengakses Nilai Konfigurasi**

	Config::get('app.timezone');

<!-- You may also specify a default value to return if the configuration option does not exist: -->
Anda juga dapat menentukan nilai default untuk kembali jika opsi konfigurasi tidak ada:

	$timezone = Config::get('app.timezone', 'UTC');

<!-- Notice that "dot" style syntax may be used to access values in the various files. You may also set configuration values at run-time: -->
Perhatikan bahwa sintaks gaya "dot" dapat digunakan untuk mengakses nilai dalam berbagai file. Anda juga dapat menetapkan nilai konfigurasi saat run-time:

<!-- **Setting A Configuration Value** -->
**Menetapkan Sebuah Nilai Konfigurasi**

	Config::set('database.default', 'sqlite');

<!-- Configuration values that are set at run-time are only set for the current request, and will not be carried over to subsequent requests. -->
Nilai konfigurasi yang ditetapkan pada saat run-time hanya ditetapkan untuk permintaan saat ini, dan tidak akan dibawa ke permintaan berikutnya.

<a name="environment-configuration"></a>
<!-- ## Environment Configuration -->
## Konfigurasi Lingkungan

<!-- It is often helpful to have different configuration values based on the environment the application is running in. For example, you may wish to use a different cache driver on your local development machine than on the production server. It is easy to accomplish this using environment based configuration. -->
Hal ini sering membantu untuk memiliki nilai konfigurasi yang berbeda berdasarkan pada lingkungan aplikasi berjalan masuk Sebagai contoh, Anda mungkin ingin menggunakan driver cache yang berbeda pada mesin pengembangan lokal Anda daripada di server produksi. Sangat mudah untuk melakukannya dengan menggunakan konfigurasi berbasis lingkungan.

<!-- Simply create a folder within the `config` directory that matches your environment name, such as `local`. Next, create the configuration files you wish to override and specify the options for that environment. For example, to override the cache driver for the local environment, you would create a `cache.php` file in `app/config/local` with the following content: -->
Cukup membuat folder dalam `config` direktori yang sesuai dengan nama lingkungan Anda, seperti `lokal. Selanjutnya, membuat file konfigurasi Anda ingin menimpa dan menentukan pilihan untuk lingkungan tersebut. Misalnya, untuk menimpa sopir cache untuk lingkungan setempat, Anda akan menciptakan sebuah cache.php `File` di `app / config / local` dengan konten berikut:

	<?php

	return array(

		'driver' => 'file',

	);

<!-- > **Note:** Do not use 'testing' as an environment name. This is reserved for unit testing. -->
> ** Catatan: ** Jangan gunakan 'percobaan' sebagai nama lingkungan. Ini disediakan untuk unit testing.

<!-- Notice that you do not have to specify _every_ option that is in the base configuration file, but only the options you wish to override. The environment configuration files will "cascade" over the base files. -->
Perhatikan bahwa Anda tidak harus menentukan pilihan _every_ yang ada di file konfigurasi dasar, tapi hanya pilihan yang Anda ingin menimpa. File-file konfigurasi lingkungan akan "cascade" atas file dasar.

<!-- Next, we need to instruct the framework how to determine which environment it is running in. The default environment is always `production`. However, you may setup other environments within the `bootstrap/start.php` file at the root of your installation. In this file you will find an `$app->detectEnvironment` call. The array passed to this method is used to determine the current environment. You may add other environments and machine names to the array as needed. -->
Selanjutnya, kita perlu untuk menginstruksikan kerangka cara menentukan lingkungan itu berjalan masuk lingkungan default selalu `produksi. Namun, Anda mungkin men-setup lingkungan lainnya dalam `bootstrap / start.php` file pada akar instalasi. Dalam file ini Anda akan menemukan `$ app-> detectEnvironment` panggilan. Array dilewatkan ke metode ini digunakan untuk menentukan kondisi saat itu. Anda dapat menambahkan lingkungan lain dan nama mesin ke array yang diperlukan.

    <?php

    $env = $app->detectEnvironment(array(

        'local' => array('your-machine-name'),

    ));

<!-- You may also pass a `Closure` to the `detectEnvironment` method, allowing you to implement your own environment detection: -->
Anda juga dapat melewati `Penutupan` ke `detectEnvironment` metode, memungkinkan Anda untuk menerapkan deteksi lingkungan Anda sendiri:

	$env = $app->detectEnvironment(function()
	{
		return $_SERVER['MY_LARAVEL_ENV'];
	});

<!-- You may access the current application environment via the `environment` method: -->
Anda dapat mengakses lingkungan aplikasi saat ini melalui metode lingkungan ``:

<!-- **Accessing The Current Application Environment** -->
**Mengakses The Current Lingkungan Aplikasi**

	$environment = App::environment();

<a name="maintenance-mode"></a>
<!-- ## Maintenance Mode -->
## pemeliharaan Modus

<!-- When your application is in maintenance mode, a custom view will be displayed for all routes into your application. This makes it easy to "disable" your application while it is updating. A call to the `App::down` method is already present in your `app/start/global.php` file. The response from this method will be sent to users when your application is in maintenance mode. -->
Bila aplikasi Anda dalam modus pemeliharaan, tampilan kustom akan ditampilkan untuk semua rute ke dalam aplikasi Anda. Hal ini membuat mudah untuk "menonaktifkan" aplikasi Anda ketika sedang memperbarui. Panggilan untuk `App :: Metode bawah` sudah ada di app / start / global.php `file`. Tanggapan dari metode ini akan dikirim ke pengguna ketika aplikasi Anda dalam modus pemeliharaan.

<!-- To enable maintenance mode, simply execute the `down` Artisan command: -->
Untuk mengaktifkan modus pemeliharaan, cukup jalankan `turun` perintah Artisan:

	php artisan down

<!-- To disable maintenance mode, use the `up` command: -->
Untuk menonaktifkan modus pemeliharaan, gunakan perintah `up`:

	php artisan up

<!-- To show a custom view when your application is in maintenance mode, you may add something like the following to your application's `app/start/global.php` file: -->
Untuk menampilkan tampilan kustom saat aplikasi Anda dalam modus pemeliharaan, Anda dapat menambahkan sesuatu seperti berikut ini untuk aplikasi Anda `app / start / global.php` berkas:

	App::down(function()
	{
		return Response::view('maintenance', array(), 503);
	});
