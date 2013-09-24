# Memulai Kilat Laravel

- [Pemasangan](#installation)
- [*Routing*](#routing)
- [Membuat *View*](#creating-a-view)
- [Membuat *Migration*](#creating-a-migration)
- [Eloquent ORM](#eloquent-orm)
- [Menampilkan Data](#displaying-data)

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

*Route* juga dapat disematkan pada *class* *controller*. Sebagai contoh:

	Route::get('users', 'UserController@getIndex');

*Route* ini memberitahu *framework* bahwa permintaan ke *route* `/users` akan menjalankan *method* `getIndex` dari *class* `UserController`. Untuk informasi lanjutan mengenai *controller routing*, baca [dokumentasi *controller*](/docs/controllers).

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

Mungkin anda merasa beberapa sintaks terlihat ganjil. Ini karena kita menggunakan Blade --sistem templatnya Laravel. Blade sangat cepat, ini karena Blade hanya menjalankan sedikit *regular expression* sederhana untuk menghimpun templat menjadi PHP murni. Blade menyediakan fungsi yang sangat berguna, semisal pewarisan templat, beberapa sintak penting pada struktur pembatasan PHP seperti `if` dan `for`. Baca [dokumentasi Blade](/docs/templates) untuk lebih rinci.

Sekarang kita telah memiliki *view*, ayo kita kembalikan *view* tersebut dari *route* `/users`. Alih-alih mengembalikan `Pengguna!`, dari *route* mengembalikan *view*:

	Route::get('users', function()
	{
		return View::make('users');
	});

Keren! Sekarang kita telah menyusun *view* sederhana yang meluaskan *layout*. Selanjutnya, mari mulai bekerja dengan lapis basis data.

<a name="creating-a-migration"></a>
## Membuat *Migration*

Untuk membuat tabel yang mana menampung data kita, kita akan menggunakan sistemm migrasi Laravel. Migrasi memungkinkan anda untuk menuliskan tetapan perubahan basis data, serta memudahkan memberikannya keseluruh anggota tim.

Petama, mari atur koneksi basis data. Anda dapat mengatur seluruh koneksi basis data anda dari berkas `app/config/database.php`. Secara asali, Laravel diatur untuk bekerja menggunakan SQLite, disertakan pula sebuah basis data SQLite pada direktori `app/database`. Bila diinginkan, anda dapat mengubah pilihan `driver` ke `mysql` dan atur mandat koneksi `mysql` pada berkas pengaturan basis data.

Selanjutnya, untuk membuat migrasi, kita akan menggunakan [*Artisan CLI*](/docs/artisan). Dari *root* proyek anda, jalankan perintah berikut dari *terminal*:

	php artisan migrate:make create_users_table

Kemudian, temukan berkas migrasi hasil ciptaan tadi pada map `app/database/migrations`. Berkas ini mengandung *class* dua *method*: `up` dan `down`. Anda dapat membuat perubahan tabel basis data sesuai keinginan pada *method* `up` dan pada *method* `down` untuk mengembalikannya.

Mari kita tentukan migrasi seperti berikut:

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

Lanjut, kita dapat menjalankan migrasi melalui *terminal* menggunakan perintah `migrate`. Eksekusi saja perintah berikut dari *root* proyek anda:

	php artisan migrate

Bila anda menginginkan memutar balik migrasi, anda dapat menggunakan perintah `migrate:rollback`. Nah, sekarang kita sudah memiliki tabel basis data, mari mulai menyisipkan beberapa data!

<a name="eloquent-orm"></a>
## Eloquent ORM

Laravel dikemas bersama sebuah ORM yang luar biasa: Eloquent. Jika anda pernah menggunakan *framework* Ruby on Rails, pasti anda tidak merasa asing dengan Eloquent, karena interaksi basis data Eloquent mengikuti gaya ActiveRecord ORM.

Petama, mari tetapkan sebuah model. Sebuah model Eloquent dapat digunakan untuk *query* sebuah tabel basis data yang terkait, maupun mewakili baris yang disajikan dalam tabel tersebut. Jangan khawatir, sebentar lagi pasti faham! Model disimpan dalam map `app/models`. Mari tetapkan sebuah model `User.php` pada map tersebut, sebagai berikut:

	class User extends Eloquent {}

Perhatikan bahwa kita tidak memberitahu Eloquent tentang tabel yang digunakan. Eloquent memiliki ragam kaidah, salah satunya adalah dengan menggunakan bentuk jamak dari nama model sebagai tabel basisdatanya. Enak sekali!

Gunakan piranti adminstrasi basis data yang anda sukai, sisipkan beberapa baris ke dalam tabel `users`, dan kita akan menggunakan Eloquent untuk mendapatkan baris-baris tersebut kembali dan menyampaikannya ke *view*.

Sekarang mari ubah *route* `/users` menjadi seperti berikut:

	Route::get('users', function()
	{
		$users = User::all();

		return View::make('users')->with('users', $users);
	});

Mari kita runut *route* ini. Permata, *method* `all` dalam model `User` akan mendapatkan seluruh baris dalam tabel `users`. Kemudian kita menyampaikan data ini ke *view* melalui *method* `with`. *Method* `with` menerima sebuah *key* dan sebuah *value*, dan ini digunakan untuk membuat bagian data terdapat di *view*.

Keren. Sekarang kita siap untuk menampilkan data pengguna tadi pada *view*!

<a name="displaying-data"></a>
## Menampilkan Data

Sekarang kita telah menjadikan data `users` tersedia pada *view*. Kita dapa menampilkannya seperti berikut:

	@extends('layout')

	@section('content')
		@foreach($users as $user)
			<p>{{ $user->name }}</p>
		@endforeach
	@stop

Anda mungkin bertanya-tanya, "di mana pernyataan `echo`-nya?". Bila menggunakan Blade, anda dapat meng-echo data dengan melingkupinya dengan kurung kurawal ganda. Ini gampang. Sekarang, anda seharusnya dapat melihat nama-nama pengguna ditampilkan pada *response* ketika anda menuju *route* `/users`.

Ini hanya awalnya saja. Dalam tutorial ini, anda telah melihat Laravel tingkat sangat dasar, tetapi masih banyak hal mengagumkan untuk dipelajari. Lanjutkan membaca dokumntasi ini sampai habis dan gali fitur-fitur berguna untuk anda yang ada dalam [Eloquent](/docs/eloquent) dan [Blade](/docs/templates). Atau, mungkin lebih tertarik pada [Queues](/docs/queues) dan [Unit Testing](/docs/testing). Mungkin juga anda ingin mengekarkan otot arsitektur anda dengan [IoC Container](/docs/ioc). Silakan pilih sesuka hati!