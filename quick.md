<!-- # Laravel Quickstart -->
# Memulai Kilat Laravel

<!--
- [Installation](#installation)
- [Routing](#routing)
- [Creating A View](#creating-a-view)
- [Creating A Migration](#creating-a-migration)
- [Eloquent ORM](#eloquent-orm)
- [Displaying Data](#displaying-data)
-->

- [Pemasangan](#installation)
- [Routing](#routing)
- [Membuat View](#creating-a-view)
- [Membuat Migration](#creating-a-migration)
- [Eloquent ORM](#eloquent-orm)
- [Menampilkan Data](#displaying-data)

<a name="installation"></a>
<!-- ## Installation -->
## Pemasangan

<!-- The Laravel framework utilizes [Composer](http://getcomposer.org) for installation and dependency management. If you haven't already, start by [installing Composer](http://getcomposer.org/doc/00-intro.md). -->
*Framework* Laravel memanfaatkan [Composer](http://getcomposer.org) untuk pemasangan dan pengaturan ketergantungan (*dependency*). Jika Anda belum memiliki Composer, mulailah dengan [memasang Composer](http://getcomposer.org/doc/00-intro.md).

<!-- Now you can install Laravel by issuing the following command from your terminal: -->
Sekarang Anda dapat memasang Laravel dengan memberikan perintah berikut ke terminal Anda:

	composer create-project laravel/laravel nama-proyek-anda --prefer-dist

<!-- This command will download and install a fresh copy of Laravel in a new `your-project-name` folder within your current directory. -->
Perintah ini akan mengunduh dan memasang salinan baru Laravel ke map baru `nama-proyek-anda` di dalam direktori kini.

<!-- If you prefer, you can alternatively download a copy of the [Laravel repository from Github](https://github.com/laravel/laravel/archive/master.zip) manually. Next run the `composer install` command in the root of your manually created project directory. This command will download and install the framework's dependencies. -->
Bila Anda suka, cara lain Anda dapat mengunduh secara manual salinan [repositori Laravel dari Github](https://github.com/laravel/laravel/archive/master.zip). Kemudian jalankan perintah `composer install` di dalam direktori *root* proyek yang Anda buat secara manual. Perintah ini akan mengunduh dan memasang semua ketergantungan *framework*.

<!-- ### Permissions -->
### Izin

<!-- After installing Laravel, you may need to grant the web server write permissions to the `app/storage` directories. See the [Installation](/docs/installation) documentation for more details on configuration. -->
Setelah memasang Laravel, Anda perlu memberi peladen web izin tulis pada direktori-direktori di dalam `app/storage`. Lihat dokumentasi [Pemasangan](/docs/installation) untuk rincian pengaturan yang lebih rinci.

<a name="routing"></a>
<!-- ## Routing -->
## *Routing*

<!-- To get started, let's create our first route. In Laravel, the simplest route is a route to a Closure. Pop open the `app/routes.php` file and add the following route to the bottom of the file: -->
Untuk memulai, mari membuat *route* perdana kita. Pada Laravel, *route* paling sederhana adalah *route* ke sebuah *Closure*. Buka berkas `app/routes.php` dan tambahkan *route* berikut pada akhir berkas:

	Route::get('users', function()
	{
		return 'Para pengguna!';
	});

<!-- Now, if you hit the `/users` route in your web browser, you should see `Users!` displayed as the response. Great! You've just created your first route. -->
Sekarang, bila Anda tuju *route* `/users` pada peramban web, Anda akan melihat tampilan `Para pengguna!` sebagai balasan. Hebat! Anda telah membuat *route* perdana Anda.

<!-- Routes can also be attached to controller classes. For example: -->
*Route* juga dapat disematkan pada kelas *controller*. Sebagai contoh:

	Route::get('users', 'UserController@getIndex');

<!-- This route informs the framework that requests to the `/users` route should call the `getIndex` method on the `UserController` class. For more information on controller routing, check out the [controller documentation](/docs/controllers). -->
*Route* ini memberitahukan *framework* bahwa permintaan ke *route* `/users` akan menjalankan metode `getIndex` pada kelas `UserController`. Untuk informasi lanjut mengenai *controller routing*, baca [dokumentasi *controller*](/docs/controllers).

<a name="creating-a-view"></a>
<!-- ## Creating A View -->
## Membuat *View*

<!-- Next, we'll create a simple view to display our user data. Views live in the `app/views` directory and contain the HTML of your application. We're going to place two new views in this directory: `layout.blade.php` and `users.blade.php`. First, let's create our `layout.blade.php` file: -->
Selanjutnya, kita akan membuat *view* sederhana untuk menampilkan data pengguna. *View* terletak di direktori `app/views` dan berisi HTML aplikasi Anda. Kita akan meletakkan dua *view* baru pada direktori ini: `layout.blade.php` dan `users.blade.php`. Pertama, mari buat berkas `layout.blade.php`:

	<html>
		<body>
			<h1>Memulai Kilat Laravel</h1>

			@yield('content')
		</body>
	</html>

<!-- Next, we'll create our `users.blade.php` view: -->
Kemudian, kita buat *view* `users.blade.php`:

	@extends('layout')

	@section('content')
		Para pengguna!
	@stop

<!-- Some of this syntax probably looks quite strange to you. That's because we're using Laravel's templating system: Blade. Blade is very fast, because it is simply a handful of regular expressions that are run against your templates to compile them to pure PHP. Blade provides powerful functionality like template inheritance, as well as some syntax sugar on typical PHP control structures such as `if` and `for`. Check out the [Blade documentation](/docs/templates) for more details. -->
Mungkin Anda merasa beberapa sintaks terlihat ganjil. Ini karena kita menggunakan Blade --sistem templatnya Laravel. Blade sangat cepat, ini karena Blade hanya menjalankan sedikit *regular expression* sederhana untuk menghimpun templat menjadi PHP murni. Blade menyediakan fungsi yang sangat berguna, semisal pewarisan templat, begitu pun beberapa sintak penting pada struktur kendali PHP seperti `if` dan `for`. Baca [dokumentasi Blade](/docs/templates) untuk lebih rinci.

<!-- Now that we have our views, let's return it from our `/users` route. Instead of returning `Users!` from the route, return the view instead: -->
Sekarang kita telah memiliki *view*, coba kita jadikan *view* tersebut sebagai kembalian dari *route* `/users`. Alih-alih mengembalikan `Para pengguna!`, dari *route* mengembalikan *view*:

	Route::get('users', function()
	{
		return View::make('users');
	});

<!-- Wonderful! Now you have setup a simple view that extends a layout. Next, let's start working on our database layer. -->
Keren! Sekarang Anda telah menyusun *view* sederhana yang mana memperluas sebuah tatanan. Selanjutnya, mari mulai bekerja dengan lapis basis data.

<a name="creating-a-migration"></a>
<!-- ## Creating A Migration -->
## Membuat *Migration*

<!-- To create a table to hold our data, we'll use the Laravel migration system. Migrations let you expressively define modifications to your database, and easily share them with the rest of your team. -->
Untuk membuat tabel sebagai penampung data kita, kita akan menggunakan sistem migrasi Laravel. Migrasi memungkinkan untuk mengekpresikan definisi perubahan basis data Anda, dan memudahkan membagikannya keseluruh anggota tim Anda.

<!-- First, let's configure a database connection. You may configure all of your database connections from the `app/config/database.php` file. By default, Laravel is configured to use MySQL, and you will need to supply connection credentials within the database configuration file. If you wish, you may change the `driver` option to `sqlite` and it will use the SQLite database included in the `app/database` directory. -->
Pertama, mari atur koneksi basis data. Anda dapat mengatur seluruh koneksi basis data yang Anda punyai dari berkas `app/config/database.php`. Secara asali, Laravel diatur untuk bekerja menggunakan MySQL, dan Anda perlu memberikan mandat koneksi pada berkas pengaturan basis data. Bila Anda mau, Anda dapat mengubah pilihan `driver` menjadi `sqlite` dan Laravel akan menggunakan basis data SQLite yang telah disertakan pada direktori `app/database`.

<!-- Next, to create the migration, we'll use the [Artisan CLI](/docs/artisan). From the root of your project, run the following from your terminal: -->
Selanjutnya, untuk membuat migrasi, kita akan menggunakan [*Artisan CLI*](/docs/artisan). Dari *root* proyek Anda, jalankan perintah berikut dari terminal:

	php artisan migrate:make create_users_table

<!-- Next, find the generated migration file in the `app/database/migrations` folder. This file contains a class with two methods: `up` and `down`. In the `up` method, you should make the desired changes to your database tables, and in the `down` method you simply reverse them. -->
Kemudian, cari berkas migrasi hasil ciptaan tadi pada map `app/database/migrations`. Berkas ini mengandung sebuah kelas dengan dua metode: `up` dan `down`. Anda dapat membuat perubahan yang diinginkan pada tabel basis data pada metode `up` dan pada metode `down` untuk mengembalikannya.

<!-- Let's define a migration that looks like this: -->
Mari kita definisikan migrasi seperti berikut:

	public function up()
	{
		Schema::create('users', function($table)
		{
			$table->increments('id');
			$table->string('email')->unique();
			$table->string('name');
			$table->timestamps();
		});
	}

	public function down()
	{
		Schema::drop('users');
	}

<!-- Next, we can run our migrations from our terminal using the `migrate` command. Simply execute this command from the root of your project: -->
Lanjut, kita dapat menjalankan migrasi melalui terminal menggunakan perintah `migrate`. Eksekusi saja perintah ini dari *root* proyek Anda:

	php artisan migrate

<!-- If you wish to rollback a migration, you may issue the `migrate:rollback` command. Now that we have a database table, let's start pulling some data! -->
Bila Anda menginginkan memutar balik migrasi, Anda dapat memberikan perintah `migrate:rollback`. Nah, sekarang kita sudah memiliki tabel basis data, mari mulai menyisipkan beberapa data!

<a name="eloquent-orm"></a>
<!-- ## Eloquent ORM -->
## Eloquent ORM

<!-- Laravel ships with a superb ORM: Eloquent. If you have used the Ruby on Rails framework, you will find Eloquent familiar, as it follows the ActiveRecord ORM style of database interaction. -->
Laravel dikemas bersama sebuah ORM yang luar biasa: Eloquent. Jika Anda pernah menggunakan *framework* Ruby on Rails, pasti Anda merasa tidak asing dengan Eloquent, karena Eloquent mengikuti gaya ActiveRecord ORM dalam interaksi basis data.

<!-- First, let's define a model. An Eloquent model can be used to query an associated database table, as well as represent a given row within that table. Don't worry, it will all make sense soon! Models are typically stored in the `app/models` directory. Let's define a `User.php` model in that directory like so: -->
Petama, mari definisikan sebuah model. Sebuah model Eloquent dapat digunakan untuk *query* sebuah tabel basis data yang terkait, maupun mewakili baris yang disajikan pada tabel tersebut. Jangan khawatir, sebentar lagi pasti faham! Model biasanya disimpan dalam direktori `app/models`. Mari definisikan sebuah model `User.php` pada direktori tersebut, sebagai berikut:

	class User extends Eloquent {}

<!-- Note that we do not have to tell Eloquent which table to use. Eloquent has a variety of conventions, one of which is to use the plural form of the model name as the model's database table. Convenient! -->
Perhatikan bahwa kita tidak memberitahu Eloquent mana tabel yang digunakan. Eloquent memiliki ragam kaidah, salah satunya adalah dengan menggunakan bentuk jamak dari nama model sebagai tabel basisdatanya. Enak sekali!

<!-- Using your preferred database administration tool, insert a few rows into your `users` table, and we'll use Eloquent to retrieve them and pass them to our view. -->
Gunakan piranti adminstrasi basis data yang Anda sukai, sisipkan beberapa baris ke dalam tabel `users`, dan kita akan menggunakan Eloquent untuk memungut baris-baris tersebut dan menyampaikannya ke *view* kita.

<!-- Now let's modify our `/users` route to look like this: -->
Sekarang mari ubah *route* `/users` menjadi seperti berikut:

	Route::get('users', function()
	{
		$users = User::all();

		return View::make('users')->with('users', $users);
	});

<!-- Let's walk through this route. First, the `all` method on the `User` model will retrieve all of the rows in the `users` table. Next, we're passing these records to the view via the `with` method. The `with` method accepts a key and a value, and is used to make a piece of data available to a view. -->
Mari kita runut *route* ini. Permata, metode `all` dalam model `User` akan memungut seluruh baris dalam tabel `users`. Kemudian kita sampaikan data ini ke *view* melalui metode `with`. Metode `with` menerima sebuah *key* dan sebuah *value*, dan ini digunakan untuk membuat bagian data tersedia ke sebuah *view*.

<!-- Awesome. Now we're ready to display the users in our view! -->
Keren. Sekarang kita siap untuk menampilkan data pengguna tadi pada *view*!

<a name="displaying-data"></a>
<!-- ## Displaying Data -->
## Menampilkan Data

<!-- Now that we have made the `users` available to our view. We can display them like so: -->
Sekarang kita telah menjadikan data `users` tersedia pada *view*. Kita dapat menampilkannya seperti berikut:

	@extends('layout')

	@section('content')
		@foreach($users as $user)
			<p>{{ $user->name }}</p>
		@endforeach
	@stop

<!-- You may be wondering where to find our `echo` statements. When using Blade, you may echo data by surrounding it with double curly braces. It's a cinch. Now, you should be able to hit the `/users` route and see the names of your users displayed in the response. -->
Anda mungkin bertanya di mana pernyataan `echo`-nya. Bila menggunakan Blade, Anda dapat mengkumandang data dengan melingkupinya dengan kurung kurawal ganda. Ini tentunya memudahkan. Sekarang, Anda dapat menuju ke *route* `/users` dan melihat nama para pengguna Anda ditampilkan sebagai balasan.

<!-- This is just the beginning. In this tutorial, you've seen the very basics of Laravel, but there are so many more exciting things to learn. Keep reading through the documentation and dig deeper into the powerful features available to you in [Eloquent](/docs/eloquent) and [Blade](/docs/templates). Or, maybe you're more interested in [Queues](/docs/queues) and [Unit Testing](/docs/testing). Then again, maybe you want to flex your architecture muscles with the [IoC Container](/docs/ioc). The choice is yours! -->
Ini hanya permulaan. Dalam tutorial ini, Anda telah melihat Laravel tingkat sangat dasar, tetapi masih banyak hal menarik untuk dipelajari. Lanjutkan membaca dokumentasi ini dan gali lebih dalam fitur-fitur berguna tersedia untuk Anda dalam [Eloquent](/docs/eloquent) dan [Blade](/docs/templates). Atau, mungkin lebih tertarik pada [Queues](/docs/queues) dan [Unit Testing](/docs/testing). Dan lagi, mungkin juga Anda ingin mengokohkan arsitektur Anda dengan [IoC Container](/docs/ioc). Silakan pilih sesuka hati!