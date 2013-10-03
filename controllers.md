# Controllers

- [Basic Controllers](#basic-controllers)
- [Controller Filters](#controller-filters)
- [RESTful Controllers](#restful-controllers)
- [Resource Controllers](#resource-controllers)
- [Menangani Metode yang Hilang](#handling-missing-methods)

<a name="basic-controllers"></a>
## Controller Dasar

Daripada mendefinisikan semua logika tingkat-rute Anda dalam sebuah file `routes.php`, Anda mungkin ingin mengatur perilaku ini menggunakan class-class *Controller*. *Controller* dapat mengelompokkan logika rute terkait ke dalam sebuah Class, serta mengambil keuntungan dari fitur yang lebih canggih seperti [dependency injection] (/ docs / IOC) otomatis.

Controller biasanya disimpan dalam direktori `app / controllers`, dan direktori ini secara default terdaftar dalam opsi `classMap` pada file `composer.json` Anda.

Berikut adalah contoh sebuah class *controller* dasar

	class UserController extends BaseController {

		/**
		 * Menampilkan profil dari user yang diberikan.
		 */
		public function showProfile($id)
		{
			$user = User::find($id);

			return View::make('user.profile', array('user' => $user));
		}

	}

Semua kontroler harus meng-*extend* class `BaseController`. Class `BaseController` juga disimpan dalam direktori `app/controllers`, dan dapat digunakan sebagai tempat untuk meletakkan shared controller logic (logika yang sama pada beberapa controller). Class `BaseController` meng-*extend* class `Controller` dari framework. Sekarang, Kita dapat membuat rute ke controller ini dengan cara:

	Route::get('user/{id}', 'UserController@showProfile');

Jika Anda memilih untuk membuat controller bertingkat atau mengatur controller menggunakan *namespace* PHP, cukup menggunakan nama kelas yang memenuhi syarat ketika menentukan rute:

	Route::get('foo', 'Namespace\FooController@method');

Anda juga dapat memberi nama pada rute controller:

	Route::get('foo', array('uses' => 'FooController@method', 'as' => 'name'));

Untuk menghasilkan URL ke *action* dari sebuah controller, Anda dapat menggunakan metode `URL::action`:

	$url = URL::action('FooController@method');

Anda dapat mengakses nama *action* dari *controller* yang dijalankan menggunakan metode `currentRouteAction`:

	$action = Route::currentRouteAction();

<a name="controller-filters"></a>
## Controller Filters

[Filters](/docs/routing#route-filters) dapat ditentukan pada rute *controller* yang mirip dengan rute "biasa":

	Route::get('profile', array('before' => 'auth',
				'uses' => 'UserController@showProfile'));

Namun, anda juga dapat menentukan *filters* dari dalam *controller*:

	class UserController extends BaseController {

		/**
		 * Instantiate a new UserController instance.
		 */
		public function __construct()
		{
			$this->beforeFilter('auth');

			$this->beforeFilter('csrf', array('on' => 'post'));

			$this->afterFilter('log', array('only' =>
								array('fooAction', 'barAction')));
		}

	}

Anda juga dapat menentukan filter *controller* secara inline menggunakan sebuah *Closure*:

	class UserController extends BaseController {

		/**
		 * Instantiate a new UserController instance.
		 */
		public function __construct()
		{
			$this->beforeFilter(function()
			{
				//
			});
		}

	}

<a name="restful-controllers"></a>
## RESTful Controllers

Laravel memungkinkan Anda untuk dengan mudah mendefinisikan sebuah rute tunggal untuk menangani setiap *action* dalam *controller* menggunakan konvensi penamaan *REST* sederhana. Pertama, tentukan rute menggunakan metode `Route::controller`:

**Mendefinisikan sebuah Controller RESTful**

	Route::controller('users', 'UserController');

Metode `controller` menerima dua argumen. Yang pertama adalah basis URI yang ditangani oleh controller, sedangkan yang kedua adalah nama class dari *controller*. Selanjutnya, tambahkan saja metode untuk controller, diawali dengan kata kerja HTTP yang mereka tanggapi:

	class UserController extends BaseController {

		public function getIndex()
		{
			//
		}

		public function postProfile()
		{
			//
		}

	}

Metode `index` akan merespon *root* URI yang ditangani oleh *controller*, yang, dalam hal ini, adalah `users`. 

Jika *action* controller Anda berisi beberapa kata, Anda dapat mengakses *action* menggunakan sintaks "dash" di URI. Misalnya, *action* controller berikut pada `UserController` akan merespon URI `users/admin-profile`:

	public function getAdminProfile() {}

<a name="resource-controllers"></a>
## Controller Resource

*Controller Resource* mempermudah untuk membangun *controller RESTful* di sekitar sumber daya. Sebagai contoh, Anda mungkin ingin membuat controller yang mengelola "Foto" yang disimpan oleh aplikasi Anda. Menggunakan perintah `controller:create` melalui CLI Artisan dan metode `Route::resource`, kita dapat dengan cepat membuat *controller* tersebut.

Untuk membuat controller melalui *command line*, eksekusi perintah berikut:

	php artisan controller:make PhotoController

Sekarang kita dapat mendaftarkan rute *resourceful* ke *controller* tersebut:

	Route::resource('photo', 'PhotoController');

Deklarasi rute tunggal ini menciptakan beberapa rute untuk menangani berbagai *action-action REST* pada *resource* `photo`. Demikian juga, *controller* yang dihasilkan sudah akan memiliki metode-metode terkait untuk masing-masing *action* ini dengan catatan yang memberitahukan URI dan kata kerja apa yang mereka menangani.

**Action-action yang Ditangai oleh Resource Controller**

Verb      | Path                  | Action       | Route Name
----------|-----------------------|--------------|---------------------
GET       | /resource             | index        | resource.index
GET       | /resource/create      | create       | resource.create
POST      | /resource             | store        | resource.store
GET       | /resource/{id}        | show         | resource.show
GET       | /resource/{id}/edit   | edit         | resource.edit
PUT/PATCH | /resource/{id}        | update       | resource.update
DELETE    | /resource/{id}        | destroy      | resource.destroy

Terkadang Anda mungkin hanya perlu untuk menangani subset *action* dari resource:

	php artisan controller:make PhotoController --only=index,show

	php artisan controller:make PhotoController --except=index

Dan, Anda juga dapat menentukan subset dari *action* untuk menangani rute:

	Route::resource('photo', 'PhotoController',
					array('only' => array('index', 'show')));

	Route::resource('photo', 'PhotoController',
					array('except' => array('create', 'store', 'update', 'delete')));

<a name="handling-missing-methods"></a>
## Menangani metode-metode yang Hilang

Sebuah metode *catch-all* dapat didefinisikan yang nantinya akan dipanggil ketika tidak ada metode yang cocok lain yang ditemukan pada *controller* yang diberikan. Metode ini harus dinamai `missingMethod`, dan menerima parameter bertipe array sebagai satu-satunya argumen:

**Defining A Catch-All Method**

	public function missingMethod($parameters)
	{
		//
	}