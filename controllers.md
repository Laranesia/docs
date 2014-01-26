# Controllers

- [Controller Dasar](#basic-controllers)
- [Controller Filters](#controller-filters)
- [RESTful Controllers](#restful-controllers)
- [Resource Controllers](#resource-controllers)
- [Menangani Metode-Metode yang Hilang](#handling-missing-methods)

<a name="basic-controllers"></a>
## Controller Dasar

<!-- Instead of defining all of your route-level logic in a single `routes.php` file, you may wish to organize this behavior using Controller classes. Controllers can group related route logic into a class, as well as take advantage of more advanced framework features such as automatic [dependency injection](/docs/ioc). -->
Daripada mendefinisikan semua logika tingkat-rute Anda dalam sebuah file `routes.php`, Anda mungkin ingin mengatur perilaku ini menggunakan class-class *Controller*. *Controller* dapat mengelompokkan logika rute terkait ke dalam sebuah Class, serta mengambil keuntungan dari fitur yang lebih canggih seperti [dependency injection](/docs/IOC) otomatis.

<!--Controllers are typically stored in the `app/controllers` directory, and this directory is registered in the `classmap` option of your `composer.json` file by default. -->
*Controller* biasanya disimpan dalam direktori `app/controllers`, dan direktori ini secara asali terdaftar dalam opsi `classMap` pada file `composer.json` Anda.

<!--Here is an example of a basic controller class:-->
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

<!--All controllers should extend the `BaseController` class. The `BaseController` is also stored in the `app/controllers` directory, and may be used as a place to put shared controller logic. The `BaseController` extends the framework's `Controller` class. Now, We can route to this controller action like so:-->
Semua *controller* harus meng-*extend* class `BaseController`. Class `BaseController` juga disimpan dalam direktori `app/controllers`, dan dapat digunakan sebagai tempat untuk meletakkan *shared controller logic* (logika yang sama yang digunakan pada beberapa controller). Class `BaseController` meng-*extend* class `Controller` dari framework. Sekarang, Kita dapat membuat rute ke controller ini dengan cara:

	Route::get('user/{id}', 'UserController@showProfile');

<!--If you choose to nest or organize your controller using PHP namespaces, simply use the fully qualified class name when defining the route:-->
Jika Anda memilih untuk membuat controller bertingkat atau mengatur controller menggunakan *namespace* PHP, cukup menggunakan nama kelas yang memenuhi syarat ketika menentukan rute:

	Route::get('foo', 'Namespace\FooController@method');

<!--You may also specify names on controller routes:-->
Anda juga dapat memberi nama pada rute *controller*:

	Route::get('foo', array('uses' => 'FooController@method', 'as' => 'name'));

<!--To generate a URL to a controller action, you may use the `URL::action` method:-->
Untuk menghasilkan URL ke *action* dari sebuah controller, Anda dapat menggunakan metode `URL::action`:

	$url = URL::action('FooController@method');

<!--You may access the name of the controller action being run using the `currentRouteAction` method:-->
Anda dapat mengakses nama *action* dari *controller* yang dijalankan menggunakan metode `currentRouteAction`:

	$action = Route::currentRouteAction();

<a name="controller-filters"></a>
## Controller Filters

<!--[Filters](/docs/routing#route-filters) may be specified on controller routes similar to "regular" routes:-->
[Filters](/docs/routing#route-filters) dapat ditentukan pada rute *controller* yang mirip dengan rute "biasa":

	Route::get('profile', array('before' => 'auth',
				'uses' => 'UserController@showProfile'));

<!--However, you may also specify filters from within your controller:-->
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

<!--You may also specify controller filters inline using a Closure:-->
Anda juga dapat menentukan filter *controller* dalam satu baris menggunakan sebuah *Closure*:

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
## Controller RESTful

<!--Laravel allows you to easily define a single route to handle every action in a controller using simple, REST naming conventions. First, define the route using the `Route::controller` method:-->
Laravel memungkinkan Anda untuk dengan mudah mendefinisikan sebuah rute tunggal untuk menangani setiap *action* dalam *controller* menggunakan konvensi penamaan *REST* sederhana. Pertama, tentukan rute menggunakan metode `Route::controller`:

<!--**Defining A RESTful Controller**-->
**Mendefinisikan sebuah *Controller* RESTful**

	Route::controller('users', 'UserController');

<!--The `controller` method accepts two arguments. The first is the base URI the controller handles, while the second is the class name of the controller. Next, just add methods to your controller, prefixed with the HTTP verb they respond to:-->
Metode `controller` menerima dua argumen. Yang pertama adalah basis URI yang ditangani oleh controller, sedangkan yang kedua adalah nama class dari *controller*. Selanjutnya, tambahkan saja metode untuk *controller*, diawali dengan kata kerja HTTP yang mereka tanggapi:

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

<!--The `index` methods will respond to the root URI handled by the controller, which, in this case, is `users`.-->
Metode `index` akan merespon *root* URI yang ditangani oleh *controller*, yang, dalam hal ini, adalah `users`. 

<!--If your controller action contains multiple words, you may access the action using "dash" syntax in the URI. For example, the following controller action on our `UserController` would respond to the `users/admin-profile` URI:-->
Jika *action* controller Anda berisi beberapa kata, Anda dapat mengakses *action* menggunakan sintaks "dash" di URI. Misalnya, *action* controller berikut pada `UserController` akan merespon URI `users/admin-profile`:

	public function getAdminProfile() {}

<a name="resource-controllers"></a>
## Controller Resource

<!--Resource controllers make it easier to build RESTful controllers around resources. For example, you may wish to create a controller that manages "photos" stored by your application. Using the `controller:make` command via the Artisan CLI and the `Route::resource` method, we can quickly create such a controller.-->
*Controller Resource* mempermudah untuk membangun *controller RESTful* di sekitar sumber daya. Sebagai contoh, Anda mungkin ingin membuat *controller* yang mengelola "Foto" yang disimpan oleh aplikasi Anda. Menggunakan perintah `controller:create` melalui CLI Artisan dan metode `Route::resource`, kita dapat dengan cepat membuat *controller* tersebut.

<!--To create the controller via the command line, execute the following command:-->
Untuk membuat *controller* melalui *command line*, eksekusi perintah berikut:

	php artisan controller:make PhotoController

<!--Now we can register a resourceful route to the controller:-->
Sekarang kita dapat mendaftarkan rute *resourceful* ke *controller* tersebut:

	Route::resource('photo', 'PhotoController');

<!--This single route declaration creates multiple routes to handle a variety of RESTful actions on the photo resource. Likewise, the generated controller will already have stubbed methods for each of these actions with notes informing you which URIs and verbs they handle.-->
Deklarasi rute tunggal ini menciptakan beberapa rute untuk menangani berbagai *action-action REST* pada *resource* `photo`. Demikian juga, *controller* yang dihasilkan sudah akan memiliki metode-metode terkait untuk masing-masing *action* ini dengan catatan yang memberitahukan Anda tentang URI dan kata kerja apa saja yang mereka menangani.

<!--**Actions Handled By Resource Controller**-->
**Action-action yang Ditangani oleh Resource Controller**

Verb      | Path                  | Action       | Route Name
----------|-----------------------|--------------|---------------------
GET       | /resource             | index        | resource.index
GET       | /resource/create      | create       | resource.create
POST      | /resource             | store        | resource.store
GET       | /resource/{id}        | show         | resource.show
GET       | /resource/{id}/edit   | edit         | resource.edit
PUT/PATCH | /resource/{id}        | update       | resource.update
DELETE    | /resource/{id}        | destroy      | resource.destroy

<!--Sometimes you may only need to handle a subset of the resource actions:-->
Terkadang Anda mungkin hanya perlu untuk menangani subset *action* dari *resource*:

	php artisan controller:make PhotoController --only=index,show

	php artisan controller:make PhotoController --except=index

<!--And, you may also specify a subset of actions to handle on the route:-->
Dan, Anda juga dapat menentukan subset dari *action* untuk menangani rute:

	Route::resource('photo', 'PhotoController',
					array('only' => array('index', 'show')));

	Route::resource('photo', 'PhotoController',
					array('except' => array('create', 'store', 'update', 'delete')));

<a name="handling-missing-methods"></a>
## Menangani Metode-Metode yang Hilang

<!--A catch-all method may be defined which will be called when no other matching method is found on a given controller. The method should be named `missingMethod`, and receives the parameter array for the request as its only argument:-->
Sebuah metode *catch-all* dapat didefinisikan yang nantinya akan dipanggil ketika tidak ada metode lain yang cocok ditemukan pada *controller* yang diberikan. Metode ini harus dinamai `missingMethod`, dan menerima parameter bertipe array sebagai satu-satunya argumen:

**Defining A Catch-All Method**

	public function missingMethod($parameters)
	{
		//
	}