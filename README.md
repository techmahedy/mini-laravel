## About MII

MII is a robust and versatile web application framework designed to streamline the development process and enhance productivity. It provides a comprehensive set of tools and features that cater to modern web development needs, integrating seamlessly with Adobe technologies to offer a cohesive development experience.

### Key Features

- **Route Management**: MII simplifies route definition and management, allowing developers to easily set up routes, handle parameters, and manage requests with minimal configuration. It supports both single and multiple route parameters to cater to complex routing needs.

- **Database Integration**: The framework offers powerful database tools, including support for migrations, seeding, and query building. MII’s database layer ensures efficient data management and interaction, with built-in support for models and database connections.

- **View Handling**: With MII, creating and managing views is straightforward. The framework supports custom Blade directives, allowing developers to extend view functionalities as needed.

- **Middleware Support**: MII includes support for both global and route-specific middleware, giving developers the flexibility to implement application-wide and route-specific logic easily.

- **Validation and Security**: The framework provides robust validation mechanisms and security features such as CSRF token management, ensuring that data handling and form submissions are both secure and reliable.

- **Dependency Injection**: MII promotes clean and maintainable code with its dependency injection system, supporting both constructor and method injection, and allowing for easy binding of interfaces to service classes.

- **Session and Logging**: MII offers comprehensive session management and logging capabilities, enabling developers to manage user sessions and track application logs effectively.

- **Collections and Macros**: The framework includes support for collections and custom macros, providing developers with additional tools to manipulate data and extend functionality as required.

MII is designed to be intuitive and developer-friendly, with a focus on providing the essential features needed for modern web applications while integrating seamlessly with Adobe technologies for enhanced capabilities.

<a name="section-1"></a>
# How to use

- [About](#section-1)
- [How to Install](#section-14)
- **Routes**
  - [Define Route](#section-2)
  - [Route Parameter](#section-8)
  - [Multiple Route Parameters](#section-9)
  - [Request](#section-15)
- **Database**
  - [Database and Migration](#section-21)
  - [Database Seeder](#section-22)
  - [Database Query Builder](#section-23)
  - [Model](#section-5)
  - [Database Connection](#section-6)
- **Views**
  - [Views](#section-7)
  - [Custom Blade Directives](#section-11)
- **Middleware**
  - [Global Middleware](#section-10)
  - [Route Middleware](#section-17)
- **Validation**
  - [Form Validation](#section-12)
  - [CSRF Token](#section-16)
- **Collections**
  - [Collection And Macro](#section-18)
- **Session and Logging**
  - [Session And Flash Message](#section-19)
  - [Log](#section-20)
- **Dependency Injection**
  - [Binding Interface to Service Class](#section-3)
  - [Controller Method Dependency Injection](#section-4)
  - [Constructor Dependency Injection](#section-13)

<a name="section-1"></a>

## About
MII, A basic PHP MVC framework designed in a way that you feel like you are working in a Laravel application. In this framework, you will get all the basic features of a web application like routing, middleware, dependency injection, eloquent relationship, model, blade template engine interface injection, and many more. Test it and if you like, please give it a star.
<a name="section-14"></a>

## How to Install
We can easily set up and install this application with a few steps. Before using this application, a minimum `PHP 8.3` version is needed.
- Step 1: To get project skeleton, run  => `composer create-project mii/mii example-app`
- Step 2: Go to the project directory with this command `cd example-app`
- Step 3: Run `composer update`
- Step 4: Start the development server by running this command `php -S localhost:8000`

<a name="section-2"></a>

## Define Route
To define the route, navigate to this file and update
### `routes/web.php`
```php

<?php

use Mii\Route;
use App\Http\Controllers\ExampleController;

Route::get('/', [ExampleController::class, 'index']);
Route::get('/about', [ExampleController::class, 'about']);
```

<a name="section-3"></a>

## Binding Interface to Service Class
To bind the interface with your service class, just update `App\Providers\AppServiceProvider.php`.

```php
<?php

namespace App\Providers;

use Mii\Container;
use App\Services\StripePaymentService;
use App\Contracts\PaymentServiceContract;

class AppServiceProvider extends Container
{
    public function register()
    {  
       //Remember, the global request() helper is available here. You can get input value here like

       //request()->input('payment_type')
       
       $this->bind(PaymentServiceContract::class, StripePaymentService::class);
    }
}
```

<a name="section-4"></a>

## Dependency Injection
Now look at that, how you can use dependency injection.
```php

<?php

namespace App\Http\Controllers;

use App\Models\User;
use App\Models\Post;
use Mii\Request;
use App\Contracts\PaymentServiceContract;

class ExampleController extends Controller
{   
    /**
     * You can pass as many class as you want as parameter
     */
    public function index(
        Request $request, //class dependency injection
        User $user, //class dependency injection
        Post $post, //class dependency injection
        PaymentServiceContract $payment //interface dependency injection
    ) {
        
       //Use any eloquent query of Laravel
    }

    public function about()
    {
        return view('about.index');
    }
}
```
<a name="section-13"></a>

## Constructor Dependency Injection
Now look at that, how you can use dependency injection using constructor.
```php

<?php

namespace App\Http\Controllers;

use App\Models\User;
use App\Models\Post;
use Mii\Request;
use App\Contracts\PaymentServiceContract;

class ExampleController extends Controller
{   
    /**
     * Look at that, we are passing interface, models. How cool it is
     */
    public function __construct(
        public PaymentServiceContract $payment, 
        public User $user, 
        public Post $post,
    ) {}
}
```

<a name="section-5"></a>

## Model
Now look at Model, how you can use it
```php

<?php

use Mii\Model;

/**
 * User Model
 *
 * This class represents the User model in the Mii framework. It extends the base Model class provided
 * by the Mii framework, allowing you to interact with the 'users' table in the database using Eloquent-like
 * features.
 *
 * You can define your model-specific methods and properties here. This model can use various features
 * provided by the Mii framework, similar to how Laravel models work.
 *
 * For example, you can define relationships, accessors, mutators, and more.
 *
 * @package App\Models
 */
class User extends Model
{
    /**
     * Model Table Name
     *
     * The name of the database table associated with this model. By default, Mii will assume the table
     * name is the plural form of the model name. You can override this property if your table name does
     * not follow this convention.
     *
     * @var string
     */
    protected $table = 'users';

    /**
     * Mass Assignable Attributes
     *
     * Specify which attributes should be mass-assignable. This helps protect against mass assignment
     * vulnerabilities. Define the attributes you want to be mass-assignable in this property.
     *
     * @var array
     */
    protected $fillable = ['name', 'email', 'password'];

    /**
     * Hidden Attributes
     *
     * Specify which attributes should be hidden from arrays. For example, you might want to hide sensitive
     * information such as passwords from the model's array or JSON representation.
     *
     * @var array
     */
    protected $hidden = ['password', 'remember_token'];

    /**
     * Date Casting
     *
     * Specify attributes that should be cast to native types. For example, if you have a 'created_at'
     * attribute in your table, you can cast it to a DateTime object for easier manipulation.
     *
     * @var array
     */
    protected $casts = [
        'email_verified_at' => 'datetime',
    ];

    /**
     * Relationships
     *
     * Define any relationships this model has with other models. For example, if a user has many posts,
     * you can define a relationship method here.
     *
     * Example:
     * public function posts()
     * {
     *     return $this->hasMany(Post::class);
     * }
     */
}
```

<a name="section-6"></a>

## Database Connection
Connect your database like that, just pass your credentials to `.env`
```
DB_CONNECTION=mysql
DB_HOST=localhost
DB_PORT=3306
DB_DATABASE=
DB_USERNAME=
DB_PASSWORD=
```

And you can print configuration value like `$_ENV['DB_CONNECTION']` or you can use `env('DB_CONNECTION')`

<a name="section-7"></a>

## Views
To work with views, default view file path inside `resources/views`. Now passing data with views like
```php
<?php

use Mii\Route;
use Mii\Application;

Route::get('/', function () {
    $version = Application::VERSION;
    return view('welcome', compact('version'));  //for nested folder file view: home.index
});
```

This will load `welcome.blade.php` file. We can print this value like

```HTML
<h1>{{ $version }}</h1>
```
### Avaiable blade systex
```BLADE
@section('looping-test')
  <p>Let's print odd numbers under 50:</p>
  <p>
    @foreach($numbers as $number)
      @if($number % 2 !== 0)
        {{ $number }} 
      @endif
    @endforeach
  </p>
@endsection
```
For mastering template
```BLADE
@include('shared.header')
<body>
  <div id="container">
    <h3>Welcome to <span class="reddish">{{ $title }}</span></h3>
    <p>{{ $content }}</p>
    
    <p>Master file</p>
    
    @yield('looping-test')
  </div>
  @include('shared.footer')
</body>
```
`You can use any blade systex as you want like laravel framework`

<a name="section-8"></a>

## Route Parameters
You can pass single or multiple parameter with route as like below
```php
<?php

use Mii\Route;
use App\Http\Controllers\ProfileController;

Route::get('/user/{id}', [ProfileController::class, 'index']);
```

Now accept this param in your controller like:

```php
<?php

namespace App\Http\Controllers;

class ProfileController extends Controller
{
    public function index($id)
    {
        return $id;
    }
}
```

<a name="section-9"></a>

## Multiple Route Parameters
You can pass multiple parameter with route as like below
```php
<?php

use App\Http\Controllers\ProfileController;

Route::get('/user/{id}/{username}', [ProfileController::class, 'index']);
```

Now accept this multiple param in your controller like:

```php
<?php

namespace App\Http\Controllers;

class ProfileController extends Controller
{
    public function index($id, $username)
    {
        return $id;

        return $username;
    }
}
```
<a name="section-15"></a>

## Request
Request is most important thing when we work in a web application. We can use Request in this application like
```php
<?php

namespace App\Http\Controllers;

use Mii\Request;

class ExampleController extends Controller
{
    public function store(Request $request)
    {   
        //asume we have a url like http://www.example.com/?name=mahedi. Now we can check.
        if($request->has('name')){
            
        }

        //We can also check form request data like
        if($request->has('name') && $request->has('email')){
            
        }

        //Now get the value from request like:
        $name = $request->input('name');
        $email = $request->input('email');

        //You can also use global request() helper like:
        $name = request()->input('name');

        //or
        if(request()->has('name')){
            
        }

        //get all the input as an array
        $input = $request->all();
        dd($input);
    }
}

```

<a name="section-10"></a>

## Global Middleware
We can define multiple global middleware. To define global middleware, just update the `App\Http\Kernel.php` file's `$middleware` array as like below 
```php
<?php

/**
 * Application global middleware
 */
public $middleware = [
    \App\Http\Middleware\ExampleMiddleware::class,
];
```

Now update your middleware like

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Mii\Request;
use Mii\Middleware\Contracts\Middleware;

class ExampleMiddleware implements Middleware
{
    public function __invoke(Request $request, Closure $next)
    {   
        /**
         * Code goes here
         */
        return $next($request);
    }
}
```

<a name="section-17"></a>

## Route Middleware
We can define multiple route middleware. To define route middleware, just update the `App\Http\Kernel.php` file's `$routeMiddleware` array as like below 

```php
<?php

/**
 * The application's route middleware.
 *
 * These middleware may be assigned to groups or used individually.
 *
 * @var array<string, class-string|string>
 */
protected $routeMiddleware = [
    'auth' => \App\Http\Middleware\Authenticate::class,
];
```

And update your route like:

```php
<?php

use Mii\Route;
use App\Http\Controllers\ProfileController;

Route::get('/', [ProfileController::class,'index'])->middleware('auth');
```

Now update your middleware like

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Mii\Request;
use Mii\Middleware\Contracts\Middleware;

class Authenticate implements Middleware
{
    /**
     * handle.
     *
     * @param	Request	$request	
     * @param	Closure	$next   	
     * @return	mixed
     */
    public function handle(Request $request, Closure $next)
    {
        /**
         * code
         */
        return $next($request);
    }
}
```
<a name="section-23"></a>

## Database Query Builder
Mii has its own custom query builder for fetching database query. See the very simple example

```php
<?php

namespace App\Http\Controllers;

use Mii\Database\DB;
use Mii\Controllers\Controller;

class ExampleController extends Controller
{
    public function index(Request $request)
    {   
        $data = DB::table('users')
            ->select(['id', 'name', 'email'])
            ->where('status', '=', 'active')
            ->orderBy('name')
            ->limit(10)
            ->offset(0)
            ->get();

        return $data; // this is laravel collection. you can use any collection wrapper in this data.
    }
}

```

<a name="section-11"></a>

## Custom Blade Directive
We can define custom blade directive. To define it, update `App\Providers\AppServiceProvider.php` as like below 
```php

<?php

namespace App\Providers;

use Mii\Container;

class AppServiceProvider extends Container
{
    public function register()
    {
        $this->directive('capitalize', function ($text) {
            return "<?php echo strtoupper($text) ?>";
        });
    }
}
```

And now we can call it in a blade file like
```HTML
{{ capitalize('hello') }}
```

<a name="section-12"></a>

## From Validation
We can validate from and can show error message in blade file very easily. To validate from , just assume we have two routes
```php
<?php

use Mii\Route;
use App\Http\Controllers\ExampleController;

Route::get('/register', [ExampleController::class, 'index']);
Route::post('/register', [ExampleController::class, 'store']);
```

And now we can update `App\Http\Controllers\ExampleController.php` like
```php
<?php

namespace App\Http\Controllers;

use Mii\Request;
use Mii\Controllers\Controller;

class ExampleController extends Controller
{
    public function index()
    {   
        return view('user.index');
    }

    public function store(Request $request)
    {   
        $request->validate([
            'email' => 'required|email|unique:user|min:2|max:100', //unique:user -> here [user] is model
            'password' => 'required|min:2|max:100',
        ]);

        //save the data

        return redirect()->url('/test'); //or redirect()->back()
    }
}

```

Now update the `resources/user/index.blade.php` like
```HTML

<!-- Showing All Error Messages -->
@if (session()->has('errors'))
    @foreach (session()->get('errors') as $error)
        @foreach ($error as $item)
            <li>{{ $item }}</li>
        @endforeach
    @endforeach
@endif

<form action="/register" method="post">
    @csrf
    <div class="mb-3">
        <label for="exampleInputEmail1" class="form-label">Email address</label>
        <input type="email" class="form-control" name="email">
        <!-- Show Specific Error Message -->
        @if (session()->has('email'))
            {{ session()->get('email') }}
        @endif
    </div>
    <div class="mb-3">
        <label for="exampleInputPassword1" class="form-label">Password</label>
        <input type="password" class="form-control" name="password">
        <!-- Show Specific Error Message -->
        @if (session()->has('password'))
            {{ session()->get('password') }}
        @endif
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
</form>
```

<a name="section-16"></a>

## CSRF Token
If you submit a post request form, then you must be provide `csrf_token` with your request like below, otherwise it will throw an exception error.

```HTML
<form action="/" method="post">
    @csrf
    <input type="submit" value="submit">
</form>
```

<a name="section-18"></a>

## Collection & Macro
Like Laravel framework, in this MII framework, you can also work with Laravel collection and you can create your own custom macro. To create a custom macro, just update service provider `App\Providers\AppServiceProvider.php` like: 
```php
<?php

namespace App\Providers;

use Mii\Container;
use Illuminate\Support\Collection;

class AppServiceProvider extends Container
{
    /**
     * register.
     *
     * Register any application services.
     * @return	void
     */
    public function register()
    {
        Collection::macro('toUpper', function () {
            return $this->map(function ($value) {
                return strtoupper($value);
            });
        });
    }
}
```

And now we can use it like:

```php
<?php

use Mii\Route;

Route::get('/', function () {

    $collection = collect(['first', 'second']);
    $upper = $collection->toUpper();

    return $upper; //output ["FIRST","SECOND"]
});
```

<a name="section-19"></a>

## Session Flash Message
When we work with form submit then we need to show validation error message or success message. We can show session flash message very easily like
```php
<?php

//Set the session flash value like
session->flash('key', 'value to be printed');

//Now you can print this value lie
if(session()->has('key')){
    echo session()->get('key');
}

```

<a name="section-20"></a>

## Log
We can easily print important messages in a log file which is located inside `storage\logs\mii.log`. To print a message, mii provide `logger()` helper function, you just need to follow this
```php
<?php

//logger() is a global helper function
logger()->info('Hello');

```

<a name="section-21"></a>

## Database and Migration
MII allow you to create migration. To create migration, MII uses `CakePHP`'s `phinx`. So to create a migration file first you need to update the configuration file `environments` array like:
### `config.php` 

```php
<?php

return [
    'environments' => [
        'default_migration_table' => 'phinxlog',
        'your_database_name' => [
            'adapter' => 'mysql',
            'host' => 'localhost',
            'name' => 'your_database_name',
            'user' => 'your_database_username',
            'pass' => 'your_database_password',
            'port' => '3306'
        ]
    ]
];
```
Now run the below command in your project terminal like:
`php vendor/bin/phinx create Post -c config.php`

Here `Post` is the model name.

Now this command will generate a migration file in the following path with the empty `change()` method.
### `app\database\migration\20231111144423_post.php`

```php
<?php

declare(strict_types=1);

use Mii\Migration\Migration;

final class Post extends Migration
{
    /**
     * Change Method.
     *
     * Write your reversible migrations using this method.
     *
     * More information on writing migrations is available here:
     * https://book.cakephp.org/phinx/0/en/migrations.html#the-change-method
     *
     * Remember to call "create()" or "update()" and NOT "save()" when working
     * with the Table class.
     */
    public function change(): void
    {
        $table = $this->table('posts');

        $table->addColumn('title', 'string', ['limit' => 20])
            ->addColumn('body', 'text')
            ->addColumn('cover_image', 'string')
            ->addTimestamps()
            ->addIndex(['title'], ['unique' => true]);

        $table->create();
    }
}
```

Now run the migration command:

`php vendor/bin/phinx migrate -c config.php`.

Now see the documentation of `phinx` [Documentation](https://book.cakephp.org/phinx/0/en/migrations.html) to learn more.

<a name="section-22"></a>

## Database Seeder
MII allow you to create database seeder file to generate fake date. To create seeder, MII uses `CakePHP`'s `phinx`. So to create a seeder file first you need run below command. Assume we are going to create `PostSeeder`:

Now run the below command in your project terminal like:
`php vendor/bin/phinx seed:create PostSeeder -c config.php`

Here `PostSeeder` is the seeder class name.

Now this command will generate a seeder file in the following path with the empty `run()` method.
### `app\database\seeds\PostSeeder.php`

```php
<?php

declare(strict_types=1);

use Phinx\Seed\AbstractSeed;

class PostSeeder extends AbstractSeed
{
    /**
     * Run Method.
     *
     * Write your database seeder using this method.
     *
     * More information on writing seeders is available here:
     * https://book.cakephp.org/phinx/0/en/seeding.html
     */
    public function run(): void
    {   
        //you can use fake() helper here as well as your entire application 

        $data = [
            [
                'title'   => fake()->title(),
                'body'    => fake()->title(),
                'created_at' => date('Y-m-d H:i:s'),
            ]
        ];

        $posts = $this->table('posts');
        $posts->insert($data)
            ->saveData();

        // empty the table
        // $posts->truncate();
    }
}

```

Now run the seeder command:

`php vendor/bin/phinx seed:run -c config.php`. 

Or you can run specific seeder class file lie

`php vendor/bin/phinx seed:run -s PostSeeder -c config.php`

Now see the documentation from `phinx` [Documentation](https://book.cakephp.org/phinx/0/en/seeding.html) to learn more.

## Author Blog link:
[Blog](https://www.laravelia.com)
