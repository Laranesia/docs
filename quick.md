# Memulai Kilat Laravel

- [Pemasangan](#installation)
- [*Routing*](#routing)
- [Creating A View](#creating-a-view)
- [Creating A Migration](#creating-a-migration)
- [Eloquent ORM](#eloquent-orm)
- [Displaying Data](#displaying-data)

<a name="installation"></a>
## Pemasangan

Untuk memasang *framework* Laravel, masukkan perintah berikut ke dalam *terminal*:

	composer create-project laravel/laravel nama-proyek-anda --prefer-dist

Atau, anda dapat juga mengunduh salinan [*repository* dari Github](https://github.com/laravel/laravel/archive/master.zip). Selanjutnya, setelah [pemasangan Composer](http://getcomposer.org), jalankan perintah `composer install` pada direktori *root* proyek anda. Perintah ini akan mengunduh dan memasang semua ketergantungan ( *dependencies* ) *framework*.

Setelah memasang *framework*, silakan buka dan lihat-lihat proyek untuk membiasakan diri dengan struktur direktorinya. Direktori `app` mengandung direktori seperti `views`, `controllers`, dan `models`. Sebagian besar kode aplikasi anda akan berada pada direktori ini. Mungkin anda ingin menjelajahi direktori `app/config` dan di sinilah tempat pilihan pengaturan tersedia untuk anda.

<a name="routing"></a>
## *Routing*

Untuk memulai, mari ciptakan *route* perdana kita. Pada Laravel, *route* paling sederhana adalah *route* ke *Closure*. Buka berkas `app/routes.php` dan tambahkan *route* berikut pada akhir berkas:

	Route::get('users', function()
	{
		return 'Pengguna!';
	});

Sekarang, bila anda tuju *route* `/users` pada peramban web, anda akan melihat `Pengguna!` ditampilkan sebagai balasan. Hebat! Anda telah menciptakan *route* perdana anda.

*Route* juga dapat disematkan pada kelas *controller*. Sebagai contoh:

	Route::get('users', 'UserController@getIndex');

*Route* ini memberitahu *framework* bahwa permintaan ke *route* `/users` akan menjalankan metode `getIndex` dari kelas `UserController`. Untuk informasi lanjutan mengenai *controller routing*, baca [dokumentasi *controller*](/docs/controllers).

<a name="creating-a-view"></a>
## Membuat *View*

Selanjutnya, kita akan membuat *view* sederhana untuk menampilkan data pengguna. *View* terletak di direktori `app/views` dan berisi HTML dari aplikasi anda. Kita akan meletakkan dua *view* baru pada direktori ini: `layout.blade.php` dan `users.blade.php`. Pertama, mari buat berkas `layout.blade.php`:

	<html>
		<body>
			<h1>Memulai Kilat Laravel</h1>

			@yield('content')
		</body>
	</html>

Kemudian, kita buat *view* `users.blade.php`:

	@extends('layout')

	@section('content')
		Pengguna!
	@stop

Mungkin anda merasa beberapa sintaks terlihat ganjil. Ini karena kita menggunakan Blade --sistem templatnya Laravel. Blade sangat cepat, ini karena hanya sedikit *regular expression* sederhana yang dijalankan untuk menghimpun template menjadi PHP murni. Blade menyediakan fungsi yang sangat berguna, semisal pewarisan templat, beberapa sintak penting pada struktur pembatasan PHP seperti `if` dan `for`. Baca [dokumentasi Blade](/docs/templates) untuk lebih rinci.

Sekarang kita telah memiliki *view*, ayo kita kembalikan *view* tersebut dari *route* `/users`. Alih-alih mengembalikan `Pengguna!`, dari *route* mengembalikan *view*:

	Route::get('users', function()
	{
		return View::make('users');
	});

Keren! Sekarang kita telah menyusun *view* sederhana yang meluaskan *layout*. Selanjutnya, mari mulai bekerja dengan lapis basis data.

<a name="creating-a-migration"></a>
## Creating A Migration

To create a table to hold our data, we'll use the Laravel migration system. Migrations let you expressively define modifications to your database, and easily share them with the rest of your team.

First, let's configure a database connection. You may configure all of your database connections from the `app/config/database.php` file. By default, Laravel is configured to use SQLite, and an SQLite database is included in the `app/database` directory. If you wish, you may change the `driver` option to `mysql` and configure the `mysql` connection credentials within the database configuration file.

Next, to create the migration, we'll use the [Artisan CLI](/docs/artisan). From the root of your project, run the following from your terminal:

	php artisan migrate:make create_users_table

Next, find the generated migration file in the `app/database/migrations` folder. This file contains a class with two methods: `up` and `down`. In the `up` method, you should make the desired changes to your database tables, and in the `down` method you simply reverse them.

Let's define a migration that looks like this:

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

Next, we can run our migrations from our terminal using the `migrate` command. Simply execute this command from the root of your project:

	php artisan migrate

If you wish to rollback a migration, you may issue the `migrate:rollback` command. Now that we have a database table, let's start pulling some data!

<a name="eloquent-orm"></a>
## Eloquent ORM

Laravel ships with a superb ORM: Eloquent. If you have used the Ruby on Rails framework, you will find Eloquent familiar, as it follows the ActiveRecord ORM style of database interaction.

First, let's define a model. An Eloquent model can be used to query an associated database table, as well as represent a given row within that table. Don't worry, it will all make sense soon! Models are typically stored in the `app/models` directory. Let's define a `User.php` model in that directory like so:

	class User extends Eloquent {}

Note that we do not have to tell Eloquent which table to use. Eloquent has a variety of conventions, one of which is to use the plural form of the model name as the model's database table. Convenient!

Using your preferred database administration tool, insert a few rows into your `users` table, and we'll use Eloquent to retrieve them and pass them to our view.

Now let's modify our `/users` route to look like this:

	Route::get('users', function()
	{
		$users = User::all();

		return View::make('users')->with('users', $users);
	});

Let's walk through this route. First, the `all` method on the `User` model will retrieve all of the rows in the `users` table. Next, we're passing these records to the view via the `with` method. The `with` method accepts a key and a value, and is used to make a piece of data available to a view.

Awesome. Now we're ready to display the users in our view!

<a name="displaying-data"></a>
## Displaying Data

Now that we have made the `users` available to our view. We can display them like so:

	@extends('layout')

	@section('content')
		@foreach($users as $user)
			<p>{{ $user->name }}</p>
		@endforeach
	@stop

You may be wondering where to find our `echo` statements. When using Blade, you may echo data by surrounding it with double curly braces. It's a cinch. Now, you should be able to hit the `/users` route and see the names of your users displayed in the response.

This is just the beginning. In this tutorial, you've seen the very basics of Laravel, but there are so many more exciting things to learn. Keep reading through the documentation and dig deeper into the powerful features available to you in [Eloquent](/docs/eloquent) and [Blade](/docs/templates). Or, maybe you're more interested in [Queues](/docs/queues) and [Unit Testing](/docs/testing). Then again, maybe you want to flex your architecture muscles with the [IoC Container](/docs/ioc). The choice is yours!
