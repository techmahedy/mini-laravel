# Zuno

Zuno is a cutting-edge web application framework built to revolutionize the way developers create robust, scalable, and efficient web applications. Designed with both simplicity and power in mind, Zuno streamlines the development process, enabling developers to focus on innovation rather than boilerplate code.

At its core, Zuno is a developer's best friend, offering a rich suite of tools and features that cater to the demands of modern web development. From seamless configuration management to advanced encryption and caching mechanisms, Zuno ensures that your applications are not only secure but also performant. Its modular architecture allows for effortless integration with third-party libraries and technologies, making it a versatile choice for projects of any scale.

What sets Zuno apart is its commitment to productivity. With built-in support for configuration caching, encrypted data storage, and intuitive dot-notation access, Zuno eliminates common pain points, allowing developers to work smarter and faster. Whether you're building a small-scale application or a large enterprise solution, Zuno provides the flexibility and reliability you need to succeed.

Zuno also shines in its ability to integrate seamlessly with Adobe technologies, offering a cohesive development experience for teams working in creative and technical environments. This unique synergy empowers developers to build visually stunning and functionally rich applications that stand out in today's competitive digital landscape.

In a world where speed, security, and scalability are paramount, Zuno stands as a beacon of innovation. It’s more than just a framework—it’s a partner in your development journey, helping you turn ideas into reality with ease and confidence.

- **Getting started**
  - [Installation](#section-1)
  - [Configuration](#section-2)
  - [Directory structure](#section-3)
  - [Frontend](#section-4)
  - [Starter kits](#section-5)
  - [Deployment](#section-6)
- **Architecture Concepts**
  - [Request Lifecycle](#section-7)
  - [Service Container](#section-8)
  - [Service Providers](#section-9)
- **The Basics**
  - [Routing](#section-10)
    - [Route Parameter](#section-11)
    - [Optional Route Parameter](#section-12)
    - [Naming Route](#section-13)
  - [Middleware](#section-14)
    - [Route Middleware](#section-15)
    - [Global Web Middleware](#section-16)
    - [Middleware Params](#section-17)
  - [CSRF Protectection](#section-18)
  - [Controllers](#section-19)
  - [Request](#section-20)
  - [Response](#section-21)
  - [Views](#section-22)
  - [Asset Bundling](#section-23)
  - [Session](#section-24)
  - [Validation](#section-25)
  - [Error Handling](#section-26)
    - [Error Log and Logging](#section-27)
- **Digging Deeper**
  - [Pool Console](#section-28)
  - [Model](#section-29)
  - [Encryption & Decryption](#section-30)
  - [Collections and Macros](#section-31)
- **Database**
  - [Database Connection](#section-32)
  - [Migration](#section-33)
  - [Seeder](#section-34)
- **Authentication**
  - [Hashing](#section-35)
  - [Authentication](#section-36)
- **Mail**
  - [Configuration](#section-37)
  - [Sending Mail](#section-38)
    - [Sending Mail with Attachment](#section-39)
    - [Mail Sending with CC and BCC](#section-40)

<a name="section-1"></a>

## Installation
Before creating your first Zuno application, make sure that your local machine has PHP, Composer. If you don't have `PHP` and `Composer` installed on your local machine, the following commands will install PHP, Composer on macOS, Windows, or Linux:
#### macOS
```bach
/bin/bash -c "$(curl -fsSL https://php.new/install/mac/8.4)"
```
#### Windows PowerShell
```bach
# Run as administrator...
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://php.new/install/windows/8.4'))
```
#### Linux
```bash
/bin/bash -c "$(curl -fsSL https://php.new/install/linux/8.4)"
```

After you have installed PHP, Composer, you're ready to create a new Zuno application.
#### Create an Application
```bash 
composer create-project zunoo/zunoo example-app
```

Now run a command to generate application key that will use to encrypt and decrypt sensitive data.
```bash
php pool key:generate
```

This command will set `APP_KEY` in your `.env` file.

Now you are ready to start development server, run this command:
```bash
php pool start
```

Use `--post` as a params in command to run application in specific post like
```bash
php pool start --port=8081
```

Now your development server is ready.

<a name="section-2"></a>

## Configuration
All of the configuration files for the Zuno framework are located in the `config` directory. Each option is documented, so feel free to look through the files and get familiar with the options available to you.
> **⚠️ Warning:** If you create a new config file, Zuno do not know about it, to let it know to Zuno, need to run
> 
> ```bash
> php pool cache:clear
> 
> ```
> 
> Proceed on now

If you run system, Zuno cached your newly created config file again. You can cache config file manyally by running this command
```php pool config:cache```

Zuno's config cache file located `storage/framework/cache/config.php` path


### Accessing Configuration Values
You may easily access your configuration values using the Config facade or global config function from anywhere in your application. The configuration values may be accessed using "`dot`" syntax, which includes the name of the file and option you wish to access. A default value may also be specified and will be returned if the configuration option does not exist:
```php
<?php

use Zuno\Config\Config;

$value = Config::get('app.name');

$value = config('app.timezone');

// Retrieve a default value if the configuration value does not exist...
$value = config('app.name', 'Zuno');
```

### Environment variable
Let's know about every variable
```
APP_NAME="Zuno"
APP_KEY=base64:+cVvutS1oVZxZUZeVGVS4evpiF6M7VaKAjEkiZi7lIM= // App Key that will use to generate encryption and decryption
APP_ENV=local
APP_DEBUG=true  // If false, system will do not show any error message to browser
APP_URL=http://localhost:8000
APP_LOCALE=en

APP_TIMEZOME=UTC
LOG_CHANNEL=stack  // for details see config/log.php
FILESYSTEM_DISK=local  // for details see config/filesystem.php

SESSION_DRIVER=file // for details see config/session.php
SESSION_LIFETIME=120
SESSION_PATH=/
SESSION_DOMAIN=

DB_CONNECTION=mysql // for details see config/database.php
DB_HOST=localhost
DB_PORT=3306
DB_DATABASE=zuno
DB_USERNAME=mahedi
DB_PASSWORD=123456
```

#### Accessing Environment Variable
To get the environment varibale data, you can use global `env()` method like
```php
env('APP_KEY', 'default value');
```

<a name="section-3"></a>

## Directory structure
```
└── 📁zunoo-framework
    └── 📁app
        └── 📁Http
            └── 📁Controllers // Controllers directory
            └── Kernel.php  // Register Application global and route specific middleware
            └── 📁Middleware // Middleware located here
        └── 📁Models // Application model directory
        └── 📁Providers // Application Service Provider
    └── 📁bootstrap // Bootstrap application dependency from here
    └── 📁config // Config files located here
    └── 📁database 
        └── 📁migrations // Database migration files
        └── 📁seeds  // Database seeder files
    └── 📁public // Public directory for assets
    └── 📁resources
        └── 📁views // All the views will be located here
    └── 📁routes // Application routes directory
    └── 📁storage
        └── 📁app
            └── 📁public // Public directory symlinked to /public/storage directory to storage/app/public directory
        └── 📁cache // All the views cache and config files cache located here
        └── 📁logs // System error log and user defined log will be printed here
        └── 📁sessions // file session driver will be cache here
    └── .env
    └── .env.example
    └── .gitignore
    └── composer.json
    └── composer.lock
    └── config.example
    └── config.php // Database seeder and migration command setup file
    └── pool // Pool command
    └── README.md
```

<a name="section-4"></a>

## Frontend
Zuno uses its own template engine blade. See some avaiable blade syntax.
#### Conditionals and Loops

### @if
Checks if a condition is true.
```blade
@if ($condition)
    // Code to execute if the condition is true
@endif
```
### @elseif
Checks an additional condition if the previous `@if` or `@elseif` condition is false.
#### Syntax:
```blade
@if ($condition1)
    // Code for condition1
@elseif ($condition2)
    // Code for condition2
@endif
```
### @else
Executes code if all previous `@if` and `@elseif` conditions are false.

#### Syntax:
```
@if ($condition)
    // Code for condition
@else
    // Code if condition is false
@endif
```
### @unless
Executes code if the condition is false.
#### Syntax:
@unless ($condition)
    // Code to execute if the condition is false
@endunless

### @isset
Checks if a variable is set and not null.

#### Syntax:
```
@isset ($variable)
    // Code to execute if the variable is set
@endisset
```

### @unset
Unsets a variable.

#### Syntax:
```
@unset ($variable)
```

### @for
Executes a loop for a specified number of iterations.

#### Syntax:
```
@for ($i = 0; $i < 10; $i++)
    // Code to execute in each iteration
@endfor
```
### @foreach
Iterates over an array or collection.

#### Syntax:
```
@foreach ($items as $item)
    // Code to execute for each item
@endforeach
```

### @forelse
Iterates over an array or collection, with a fallback if the array is empty.

#### Syntax:
```
@forelse ($items as $item)
    // Code to execute for each item
@empty
    // Code to execute if the array is empty
@endforelse
```

### @while
Executes a loop while a condition is true.

#### Syntax:
```
@while ($condition)
    // Code to execute while the condition is true
@endwhile
```

### @switch
Executes a switch-case block.

#### Syntax:
```
@switch ($variable)
    @case ($value1)
        // Code for value1
        @break

    @case ($value2)
        // Code for value2
        @break

    @default
        // Default code
@endswitch
```
### @break
Breaks out of a loop or switch-case block.

#### Syntax:
```
@break
```
Example
```
@foreach ($users as $user)
    @if ($user->isBanned)
        @break
    @endif
    <p>{{ $user->name }}</p>
@endforeach
```
### @continue
Skips the current iteration of a loop.

#### Syntax:
```
@continue
```
Example:
```
@foreach ($users as $user)
    @if ($user->isBanned)
        @continue
    @endif
    <p>{{ $user->name }}</p>
@endforeach
```
### @php
Executes raw PHP code.

#### Syntax:
```
@php
    // PHP code
@endphp
```

### @json
Encodes data as JSON.

#### Syntax:
```
@json ($data)
```
Example
```
<script>
    var users = @json($users);
</script>
```

### @csrf
Generates a CSRF token for forms.

#### Syntax:
```
@csrf
```
Example
```
<form method="POST">
    @csrf
    <button type="submit">Submit</button>
</form>
```
### @exit
Usage
```
@foreach ($users as $user)
    @if ($user->isAdmin())
        @exit
    @endif
@endforeach
```
### @empty
Usage
```
@empty ($users)
    //  User is empty
@endempty
```
## Section Directives
### @extends
```
@extends('layouts.app')
```
Extends a Blade layout.

### @include
```
@include('partials.header')
```
Includes another Blade view
### @yield
```
@yield('content')
```
Outputs the content of a section.

### @section
Example:
```
@section('content')
    <p>This is the content section</p>
@endsection
```
Defines a section to be yielded later
 
### @stop
```
@stop
```
Stops the current section output.

### @overwrite
```
@overwrite
```
Overwrites the current section content.

### Authentication Directives

### @auth
Checks if a user is authenticated.

#### Syntax:
```
@auth
    // Code to execute if the user is authenticated
@endauth
```
### @guest
Checks if a user is not authenticated (guest).

#### Syntax:
```
@guest
    // Code to execute if the user is not authenticated
@endguest
```
### Flash Message Directives
### @hasflash
Checks if there are any flash messages.

#### Syntax:
```
@hasflash
    // Code to execute if flash messages exist
@endhasflash
```
Example
```
@hasflash
    <div class="alert">
        {!! flash()->getMessages() ||}
    </div>
@endhasflash
```

### @error
Checks if a specific error message exists in the flash messages.

#### Syntax:
```
@error('key')
    // Code to execute if the error message exists
@enderror
```
Example
```
@error('email')
    <p class="error">{{ $message }}</p>
@enderror
```

<a name="section-5"></a>

## Starter kits
To give you a head start building your new Zuno application, we are happy to offer application starter kits. These starter kits give you a head start on building your next Zuno application, and include the routes, controllers, and views you need to register and authenticate your application's users.
```
layout/app.blade.php
html
    <!DOCTYPE html>
    <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>@yield('title')</title>
            <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css">
        </head>
        <body>
        <div class="container mt-4">
            <div class="row justify-content-center">
                <div class="col-md-8">
                    @yield('content')
                </div>
            </div>
        </div>
        <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
    </body>
</html>
```
Extented main file 
```html
@extends('layouts.app')
@section('title','Home')
@section('content')
    // Content goes here
@endsection
```

<a name="section-7"></a>

## Request Lifecycle
Coming within next week

<a name="section-8"></a>

## Service Container
The Zuno Service Container is a robust and versatile tool designed to streamline dependency management and facilitate dependency injection within application. At its core, dependency injection is a sophisticated concept that simplifies how class dependencies are handled: instead of a class creating or managing its own dependencies, they are "injected" into the class—typically through the constructor or, in certain scenarios, via setter methods. This approach promotes cleaner, more modular, and testable code, making the Zuno framework an ideal choice for modern, scalable web development.

Let's look at a simple example:
```php
<?php

namespace App\Http\Controllers;

use Zuno\Http\Request;
use App\Service\UserService;
use App\Http\Controllers\Controller;

class UserController extends Controller
{
    public function __construct(protected User $user) {}

    public function index(UserService $userService, Request $request)
    {
        $userServiceData = $userInterface->getUser();
        $users = $this->user->all();

        return view('welcome',compact('userServiceData','users'));
    }
}
```

In this example, the UserInterface from controller `index` method and User class from constructor will automatically injected in Zuno's request life cycle and will be automatically instantiated. 


## Binding
### bind()
Binds a service to the container. You can bind a class name or a callable (closure) that returns an instance of the service.
```php
bind(string $abstract, callable|string $concrete, bool $singleton = false): void
```
Parameters:
1. $abstract: The service name or interface.
2. $concrete: The class name or a callable that returns an instance.
3. $singleton: Whether the binding should be a singleton (optional, default: false

Example
```php
<?php

namespace App\Providers;

use Zuno\DI\Container;
use App\Service\ProductInterface;
use App\Service\ProductClass;

class AppServiceProvider extends Container
{
  /**
   * Register any application services.
   *
   * This method is used to bind services into the container.
   * It is typically used to register service providers or other
   * application-specific services that are needed throughout the app.
   *
   * @return void
   */
  public function register(): void
  { 
    // You can choose any of the below declaration
    $this->bind(ProductInterface::class, fn() => new ProductClass );

    $this->bind(ProductInterface::class, ProductClass::class);
  }
}
```

## Singleton Bindings
### singleton()
Binds a service as a singleton. The container will always return the same instance when resolving the service.
Example
```php
$this->singleton(ProductInterface::class,fn() => new ProductClass);
```

## Conditional Bindings
#### when()
Conditionally binds a service based on a condition. If the condition is true, the binding is applied.

#### Syntax:
```php
when(callable|bool $condition): ?self
```
Example
```php
 $this->when(fn() => rand(0, 1) === 1)?->singleton(ProductInterface::class,fn() => new ProductClass);
```

The Zuno Service Container simplifies dependency management by providing a clean and intuitive API for binding and resolving services. Whether you're working with regular bindings, singletons, or conditional logic, the container ensures your application remains modular, testable, and scalable.


<a name="section-8"></a>

<a name="section-9"></a>

## Service Providers
Zuno supports a single service provider that loads the application custom service like
```php
<?php

namespace App\Providers;

use Zuno\DI\Container;
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
Now use this macro like
```php
<?php

use Zuno\Route;

Route::get('/', function () {

    $collection = collect(['first', 'second']);
    $upper = $collection->toUpper();

    return $upper; //output ["FIRST","SECOND"]
});
```

<a name="section-10"></a>

## Routing
Zuno now supports only GET and POST request route. All the routes will be initialized from `route/web.php`. To define a route
```php

<?php

use Zuno\Route;
use App\Http\Controllers\ExampleController;

Route::get('/', [ExampleController::class, 'index']);
Route::get('/about', [ExampleController::class, 'about']);
```
<a name="section-11"></a>

## Route Parameter
You can pass single or multiple parameter with route as like below
```php
<?php

use Zuno\Route;
use App\Http\Controllers\ProfileController;

Route::get('/user/{id}', [ProfileController::class, 'index']);
```
Now accept this param in your controller like:
```php
<?php

namespace App\Http\Controllers;

class ProfileController extends Controller
{
    public function index(int $id)
    {
        return $id;
    }
}
```
### Multiple Route Parameters
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
    public function index(int $id, string $username)
    {
        return $id;

        return $username;
    }
}
```

<a name="section-12"></a>

## Optional Route Parameter
Zuno accept optional parameter and for this, you have nothing to do.
> **⚠️ Warning:** By default, the Zuno controller callback takes parameters as optional. So, if you pass parameters in your route but do not accept them in the controller, it will not throw an error.
>
Example
```php
Route::get('/user/{id}/{username}', [ProfileController::class, 'index']);
```
if you visit `http://localhost:8000/user/1/mahedi-hasan`

```php
<?php

namespace App\Http\Controllers;

class ProfileController extends Controller
{
    public function index()
    {
        return "welcome"; // Still works and you will get the response
    }
}
```


<a name="section-13"></a>

## Naming Route

Zuno support convenient naming route structure. To create a naming route, you can do

```php

use Zuno\Route;
use App\Http\Controllers\UserController;

Route::get('/user/{id}/{name}', [UserController::class, 'profile'])->name('profile');
```
Now use this naming route any where using route() global method.
```php
 <form action="{{ route('profile', ['id' => 2, 'name' => 'mahedy']) }}" method="post">
    @csrf
    <button type="submit" class="btn btn-primary">Submit</button>
</form>
```
If there is single param in your route, just use
```php
Route::get('/user/{id}', [UserController::class, 'profile'])->name('profile');
```
Now call the route
```php
{{ route('profile', 2) }}
```


<a name="section-14"></a>

## Middleware
Middleware acts as a bridge between a request and a response, allowing you to filter or modify incoming requests before they reach the controller. It is useful for authentication, logging, and request modification.

Zuno supports two types of middleware
* Global Middleware
* Route Middleware

##### Global Middleware
Global middleware applies to all routes automatically. It is executed on every request, ensuring consistent behavior across the application.

##### Route Middleware
Route middleware is applied to specific routes, giving you more control over which requests are affected. You can assign middleware to a route or a group of routes as needed.

<a name="section-15"></a>

## Route Middleware
We can define multiple route middleware. To define route middleware, just update the `App\Http\Kernel.php` file's `$routeMiddleware` array as like below

#### Create New Middleware
Zuno has command line interface to create a new middleware. Zuno has `make:middleware` command to create a new middleware.
```
php pool create:middleware Authenticate
```
Then this command will create a new `Authenticate` for you located inside `App\Http\Authenticate` directory

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

use Zuno\Route;
use App\Http\Controllers\ProfileController;

Route::get('/', [ProfileController::class,'index'])->middleware('auth');
```

The `ProfileController` `index` method is now protected by the auth middleware. Update your middleware configuration to ensure authentication is required before accessing this method.

```php
<?php

namespace App\Http\Middleware;

use Zuno\Middleware\Contracts\Middleware;
use Zuno\Http\Response;
use Zuno\Http\Request;
use Zuno\Auth\Security\Auth;
use Closure;

class Authenticate implements Middleware
{
    /**
     * Handle an incoming request
     *
     * @param Request $request
     * @param \Closure(\Zuno\Http\Request) $next
     * @return Zuno\Http\Response
     */
    public function __invoke(Request $request, Closure $next): Response
    {
        if (Auth::check()) {
            return $next($request);
        }

        return redirect()->to('/login');
    }
}
```

<a name="section-16"></a>

## Global Middleware
We can register multiple global middleware. To register global middleware, just update the `App\Http\Kernel.php` file's `$middleware` array.

#### Create New Middleware
Zuno has command line interface to create a new middleware. Zuno has `make:middleware` command to create a new middleware.
```
php pool create:middleware CorsMiddlware
```
Then this command will create a new `CorsMiddleware` for you located inside `App\Http\Middleware` directory

```php
<?php

/**
 * Application global middleware
 */
public $middleware = [
    \App\Http\Middleware\CorsMiddleware::class,
];
```
Now update your middleware like
```php
<?php

namespace App\Http\Middleware;

use Closure;
use Zuno\Request
use Zuno\Http\Response;
use Zuno\Middleware\Contracts\Middleware;

class CorsMiddleware implements Middleware
{   
   /**
     * Handle an incoming request
     *
     * @param Request $request
     * @param \Closure(\Zuno\Http\Request) $next
     * @return Zuno\Http\Response
     */
    public function __invoke(Request $request, Closure $next): Response
    {
        /**
         * Code goes here
         */
        return $next($request);
    }
}
```

<a name="section-17"></a>

## Middleware Params
We can define multiple route middleware parameters. To define route middleware, add a `:` after the middleware name. If there are multiple parameters, separate them with a `,` comma. See the example

```php
<?php

use Zuno\Route;
use App\Http\Controllers\ExampleController;

Route::get('/', [ExampleController::class, 'index'])->middleware(['auth:admin,editor,publisher', 'is_subscribed:premium']);
```

* In this example:
  * The auth middleware receives three parameters: `admin`, `editor`, and `publisher`.
  * The is_subscribed middleware receives one parameter: `premium`.

#### Accept Parameters in Middleware
In the middleware class, define the handle method and accept the parameters as function arguments:
```php
<?php

/**
 * Handle an incoming request
 *
 * @param Request $request
 * @param \Closure(\Zuno\Http\Request) $next
 * @return Zuno\Http\Response
 */
public function __invoke(Request $request, Closure $next, $admin, $editor, $publisher): Response
{
    // Parameters received:
    // $admin = 'admin'
    // $editor = 'editor'
    // $publisher = 'publisher'

    // Middleware logic goes here

    return $next($request);
}
```


<a name="section-18"></a>

## CSRF Protectection

Cross-site request forgeries are a type of malicious exploit whereby unauthorized commands are performed on behalf of an authenticated user. Thankfully, Laravel makes it easy to protect your application from cross-site request forgery (CSRF) attacks.

Zuno automatically generates a CSRF "token" for each active user session managed by the application. This token is used to verify that the authenticated user is the person actually making the requests to the application. Since this token is stored in the user's session and changes each time the session is regenerated, a malicious application is unable to access it.

The current session's CSRF token can be accessed via the request's session or via the csrf_token helper function:
```php
use Zuno\Http\Request;

Route::get('/token', function (Request $request) {
    $token = $request->session()->token();

    $token = csrf_token();

    // ...
});
```
Anytime you define a "POST" HTML form in your application, you should include a hidden CSRF _token field in the form so that the CSRF protection middleware can validate the request. For convenience, you may use the @csrf Blade directive to generate the hidden token input field:
```php
<form method="POST" action="/profile">
    @csrf

    <!-- Equivalent to... -->
    <input type="hidden" name="_token" value="{{ csrf_token() }}" />
</form>
```


<a name="section-19"></a>

## Controllers
Rather than defining all request-handling logic as closures in route files, you can use controller classes to organize related functionality. Controllers centralize request handling, making your code more structured and maintainable. For example, a UserController can manage user-related actions like displaying, creating, updating, and deleting users. By default, controllers are stored in the `app/Http/Controllers` directory.

To quickly generate a new controller, you may run the `make:controller` Pool command. By default, all of the controllers for your application are stored in the `app/Http/Controllers` directory

```
php pool make:controller UserController

php pool make:controller User/UserController // create controller inside User directory
```
Let's take a look at an example of a basic controller. A controller may have any number of public methods which will respond to incoming HTTP requests:
```php
<?php

namespace App\Http\Controllers;

use App\Models\User;

class UserController extends Controller
{
    /**
     * Show the profile for a given user.
     */
    public function show(string $id)
    {
        return view('user.profile', [
            'user' => User::findOrFail($id)
        ]);
    }
}
```
Once you have written a controller class and method, you may define a route to the controller method like so:
```php
use App\Http\Controllers\UserController;

Route::get('/user/{id}', [UserController::class, 'show']);
```


<a name="section-20"></a>

## Request
Zuno's Zuno\Http\Request class provides an object-oriented way to interact with the current HTTP request being handled by your application as well as retrieve the input, cookies, and files that were submitted with the request.
#### Accessing the Request Data
To access request data, Zuno has some default built in method.
Zuno provides a powerful `Request` class to handle incoming HTTP requests. This class offers a wide range of methods to access and manipulate request data, making it easy to build robust web applications.

#### HTML Form Request Data

These methods are used to access data submitted through HTML forms (e.g., POST, GET).

* **Accessing All Data:**
    * `$request->all()`: Returns an array of all request data.
* **Accessing Specific Field Values:**
    * `$request->name`: Retrieves the value of the "name" field.
    * `$request->input('name')`: Equivalent to `$request->name`, using the `input()` method.
* **Retrieving Data with Exclusions:**
    * `$request->except('name')`: Retrieves all data except the "name" field.
    * `$request->except(['name', 'age'])`: Retrieves data excluding "name" and "age".
* **Retrieving Specific Fields Only:**
    * `$request->only('name')`: Retrieves only the "name" field.
    * `$request->only(['name', 'age'])`: Retrieves only "name" and "age" fields.
* **Checking Field Existence:**
    * `$request->has('name')`: Returns `true` if the "name" field exists, `false` otherwise.
* **Accessing Validation Results:**
    * `$request->passed()`: Retrieves data that passed validation (if applied).
    * `$request->failed()`: Retrieves data that failed validation (if applied).

#### Server Request Information

These methods provide access to server-related request information.

* **Client Information:**
    * `$request->ip()`: Retrieves the client's IP address.
    * `$request->userAgent()`: Retrieves the user agent string (browser/device info).
    * `$request->referer()`: Retrieves the referer URL (where the request came from).
* **Headers:**
    * `$request->headers()`: Retrieves all request headers as an array.
    * `$request->header('key')`: Retrieves the value of a specific header by key.
* **Request Details:**
    * `$request->scheme()`: Retrieves the request scheme (e.g., "http" or "https").
    * `$request->isSecure()`: Returns `true` if the request is using HTTPS, `false` otherwise.
    * `$request->isAjax()`: Returns `true` if the request is an AJAX request, `false` otherwise.
    * `$request->isJson()`: Returns `true` if the request expects a JSON response, `false` otherwise.
    * `$request->contentType()`: Retrieves the content type of the request (e.g., "application/json").
    * `$request->contentLength()`: Retrieves the content length of the request body.
    * `$request->method()`: Retrieves the HTTP method used (e.g., GET, POST).
    * `$request->query()`: Retrieves all query parameters (GET data).
    * `$request->url()`: Retrieves the full URL of the request.
    * `$request->host()`: Retrieves the host name (e.g., "example.com").
    * `$request->server()`: Retrieves all server variables as an array.
    * `$request->uri()`: Retrieves the request URI (e.g., "/path/to/resource").
* **Cookies:**
    * `$request->cookie()`: Retrieves all cookies sent with the request.

#### Authentication

This method is used to access authenticated user data.

* `$request->auth()`: Retrieves the authenticated user data.

#### File Uploads

These methods handle file uploads from HTML forms.

* **Accessing the File Object:**
    * `$image = $request->file('file')`: `'file'` is the HTML form input name.
* **Retrieving File Information:**
    * If `$file = $request->file('file')`:
        * If `$file->isValid()`:
            * `$file->getClientOriginalName()`: Retrieves the original file name.
            * `$file->getClientOriginalPath()`: Retrieves the temporary file path.
            * `$file->getClientOriginalType()`: Retrieves the MIME type.
            * `$file->getSize()`: Retrieves the file size in bytes.
##### Example
```php
// Accessing the uploaded file object
$image = $request->file('file'); // 'file' is the HTML form input name

// Retrieving file information
if ($file = $request->file('file')) {
    if ($file->isValid()) {
        $file->getClientOriginalName(); // Retrieves the original file name
        $file->getClientOriginalPath(); // Retrieves the temporary file path
        $file->getClientOriginalType(); // Retrieves the MIME type
        $file->getSize(); // Retrieves the file size in bytes
    }
}
```

#### Global `request()` Helper

The `request()` helper function provides a global way to access the `Request` object throughout your application.

* `request()->only('name')`: Equivalent to `$request->only('name')`.

<a name="section-21"></a>

## Response
All routes and controllers should return a response to be sent back to the user's browser. Zuno provides several different ways to return responses. The most basic response is returning a string from a route or controller. The framework will automatically convert the string into a full HTTP response:
```php
Route::get('/', function () {
    return 'Hello World';
});
```

In addition to returning strings from your routes and controllers, you may also return arrays.
```php
Route::get('/', function () {
    return [1, 2, 3];
});
```
You can make this array into Laravel collection by wrapping data with the collection helper.
```php
Route::get('/', function () {
    return collect([1, 2, 3]);
});
```
### Response Objects
Typically, you won't just be returning simple strings or arrays from your route actions. Instead, you will be returning full `Zuno\Http\Response` instances.
```php
Route::get('/', function () {
    return response()->text("Hello World", 200);
});
```

### Eloquent Models and Collections
For Eloquent, Zuno usage Laravel Eloquent Model and Collection also. So you can return Eloquent collection data as `Zuno\Http\Response`
```php
use App\Models\User;

Route::get('/user', function (User $user) {
    return $user->all();
});
```
### Attaching Headers to Responses
Keep in mind that most response methods are chainable, allowing for the fluent construction of response instances. For example, you may use the `setHeader` method to add a series of headers to the response before sending it back to the user:
```php
return response()->text("Hello World", 200)
    ->setHeader('Content-Type', 'text/plain')
    ->setHeader('X-Header-One', 'Header Value')
    ->setHeader('X-Header-Two', 'Header Value');
```
Or, you may use the withHeaders method to specify an array of headers to be added to the response:
```php
return response()->text("Hello World", 200)
    ->withHeaders([
        'Content-Type' => 'text/plain',
        'X-Header-One' => 'Header Value',
        'X-Header-Two' => 'Header Value',
    ]);
```
You also use the `Response` object as the controller method params and get the same response like
```php
<?php

namespace App\Http\Controllers;

use Zuno\Http\Response;
use App\Http\Controllers\Controller;

class UserController extends Controller
{

    public function index(Response $response)
    {
        return $response->text("Hello World", 200)
            ->withHeaders([
                'Content-Type' => 'text/plain',
                'X-Header-One' => 'Header Value',
                'X-Header-Two' => 'Header Value',
            ]);
            
    }
}
```
### Redirects

Redirect responses are instances of the `Zuno\Http\Redirect` class, and contain the proper headers needed to redirect the user to another URL. There are several ways to generate a Redirect instance. The simplest method is to use the global `redirect` helper:
```php
Route::get('/dashboard', function () {
    return redirect()->to('/home/dashboard');
});
```
Sometimes you may wish to redirect the user to their previous location, such as when a submitted form is invalid. You may do so by using the global back helper function.
```php
Route::post('/user/profile', function () {
    // Validate the request...
 
    return redirect()->back()->withInput();
    return redirect()->back()->withInput()->withErrors($errors);
});
```
### Redirecting to Named Routes
When you call the redirect helper with no parameters, an instance of `Zuno\Routing\Redirect` is returned, allowing you to call any method on the `Redirect` instance. For example, to generate a `Redirect` to a named route, you may use the route method:
```php
return redirect()->route('login');
```
If your route has parameters, you may pass them as the second argument to the route method:
```php
// For a route with the following URI: /profile/{id}/{username}
return redirect()->route('profile', ['id' => 1, 'username' => 'mahedy150101']);
```
### Redirecting to External Domains
Sometimes you may need to redirect to a domain outside of your application. You may do so by calling the `away` method, which creates a RedirectResponse without any additional URL encoding, validation, or verification:
```php
return redirect()->away('https://www.google.com');
```
### Redirecting With Flashed Session Data

Redirecting to a new URL and flashing data to the session are usually done at the same time. Typically, this is done after successfully performing an action when you flash a success message to the session. For convenience, you may create a `RedirectResponse` instance and flash data to the session in a single, fluent method chain:
```php
<?php

namespace App\Http\Controllers;

use Zuno\Http\Request;
use App\Http\Controllers\Controller;

class LoginController extends Controller
{
    public function login(Request $request)
    {
       flash()->message('success', 'You are logged in');
       return redirect()->to('/home');

    }
}

```
You can show this in many ways. This ways will automatically check is there is any session flash message then display it
```php
@hasflash
    {!! flash()->display() !!} // this will render html bootstrap 5 alert message by type
@endhasflash
```
Or you can show it like this ways
```php
@if (flash->has('success'))
    <div class="alert alert-success">
        {{ flash->get('success') }}
    </div>
@endif
```
### View Responses
If you need control over the response's status and headers but also need to return a view as the response's content, you should use the view method:
```php
return response()->view('welcome')->setHeader('Content-Type', 'text/html');
```
### JSON Responses
The json method will automatically set the `Content-Type header to application/json`, as well as convert the given array to JSON using the `json_encode` PHP function:
```php
 return response()->json([
            'name' => 'Mahedi Hasan',
            'age' => 33
        ], 200);
```
### File Downloads
Coming

<a name="section-22"></a>

## Views
In Zuno, it's not practical to return entire HTML document strings directly from your routes and controllers. Fortunately, views offer a clean and structured way to manage your application's UI by keeping HTML in separate files.

Views help separate your application's logic from its presentation layer, improving maintainability and readability. In Zuno, views are typically stored in the resources/views directory. The templating system in Zuno allows you to create dynamic and reusable UI components efficiently. A basic view file might look like this:
```
<!-- View stored in resources/views/greeting.blade.php -->

<html>
    <body>
        <h1>Hello, {{ $name }}</h1>
    </body>
</html>
```
Since this view is stored at resources/views/greeting.blade.php, we may return it using the global view helper like so:
```
Route::get('/', function () {
    return view('greeting', ['name' => 'James']);
});
```
### Passing Data to Views
As you saw in the previous examples, you may pass an array of data to views to make that data available to the view:
```php
return view('greetings', ['name' => 'Victoria']);
```
### Optimizing Views
In Zuno Framework, Blade template views are compiled on demand. When a request is made to render a view, Zuno checks if a compiled version exists. If the compiled view is missing or outdated compared to its source, Zuno will recompile it dynamically.

However, compiling views at runtime introduces a minor performance overhead. To optimize performance, Zuno provides the view:cache command, which precompiles all views ahead of time. This can be particularly useful during deployment.
```php
php pool view:cache
```
You may use the `view:clear` command to clear the view cache:
```
php artisan view:clear
```


<a name="section-23"></a>

## Asset Bundling
In Zuno, you can easily load CSS, JavaScript, and other asset files from the public folder using the enqueue() functio
```html
<link rel="stylesheet" href="{{ enqueue('style.css') }}">
<link rel="stylesheet" href="{{ enqueue('assets/style.css') }}"> // Here assets is a folder name
```
This ensures that your asset paths remain dynamic and easily manageable across the project.

<a name="section-24"></a>

## Session
Zuno's `session()` helper method provides a simple and intuitive way to manage session data in your application. Below is a detailed explanation of the available methods and their usage. Like get, put, has, forget, flush, and more, you can easily handle session operations.

### Session Console Command
To remove all the session data, you can run
```bash
php pool session:clear
php pool session:clear --force
```

### Put and Get Session Data
To store data in the session and retrieve it later, use the put and get methods.
#### Syntax
```php
session()->put($key, $value);
```
Example
```php
session()->put('name', 'Mahedi Hasan');
return session()->get('name');
```
You can check a key exists or not in a session data by doing this
```php
if(session()->has('name')){
    // Session key data exists
}
```

You can destroy specific key session data by doing this
```php
session()->forget('key')
```
You can clear all session data by calling `flush()` function
```php
session()->flush();
```

Even you can destroy session using `destroy` function
```php
session()->destroy()
```
### Set and Get Session ID
You can set seesion id and get is also. 
```php
session()->setId($id);
session()->getId()
```

If you want to show all the session data from your application, call `all()` method like
```php
session()->all()
```

<a name="section-25"></a>

## Validation
Zuno provides a very simple approaches to validate your application's incoming data. It is most common to use the validate method available on all incoming HTTP requests. However, we will discuss other approaches to validation as well. We can validate from and can show error message in blade file very easily. To validate from , just assume we have two routes.
```php
<?php

use Zuno\Route;
use App\Http\Controllers\ExampleController;

Route::get('/register', [ExampleController::class, 'index']);
Route::post('/register', [ExampleController::class, 'store']);
```
And now we can update `App\Http\Controllers\ExampleController.php` like
```php
<?php

namespace App\Http\Controllers;

use Zuno\Request;
use App\Http\Controllers\Controller;

class ExampleController extends Controller
{
    public function store(Request $request)
    {
        $data = $request->sanitize([
            'email' => 'required|email|unique:users|min:2|max:100',
            'password' => 'required|min:2|max:20',
            'username' => 'required|unique:users|min:2|max:100',
            'name' => 'required|min:2|max:20'
        

        // If validation passes, proceed to store the user data.
    }
}
```
## User Creation: `store()` Method

The `store()` method in the `UserController` is designed to handle user registration form submissions. It ensures that user input is validated and sanitized before being stored in the database.

**Functionality:**

1.  **Request Handling:**
    * The method receives an instance of the `Request` class (`$request`), which encapsulates all incoming form data.

2.  **Data Sanitization and Validation:**
    * The `$request->sanitize()` method is employed to validate and filter the incoming data according to the following rules:
        * `email`:
            * Must be present (`required`).
            * Must adhere to a valid email format.
            * Must be unique within the `users` table.
            * Must be between 2 and 100 characters in length.
        * `password`:
            * Must be present (`required`).
            * Must be between 2 and 20 characters in length.
        * `username`:
            * Must be present (`required`).
            * Must be unique within the `users` table.
            * Must be between 2 and 100 characters in length.
        * `name`:
            * Must be present (`required`).
            * Must be between 2 and 20 characters in length.

3.  **Data Storage:**
    * Upon successful validation, the sanitized and validated data is stored in the `$data` variable.
    * The method then proceeds to store this data in the database, typically by creating a new record in the `users` table.

You can also get `failed` and `passed` data from the request. 
```php
<?php

public function register(Request $request)
{
    $data = $request->sanitize([
        'email' => 'required|email|unique:users|min:2|max:100',
        'password' => 'required|min:2|max:20',
        'username' => 'required|unique:users|min:2|max:100',
        'name' => 'required|min:2|max:20'
    ]);

    if ($data instanceof \Zuno\Http\Response) {
        // Validation failed
        dd($request->failed()); 
    } else {
        dd($request->passed());
    }
    
    // Safely create the user now
    User::create($request->passed());
}
```

### Show Validation Message
To show validation error message in your blade file, zuno has a very elegent syntax. Showing validation error message specific key wise
```html
<input .... class="form-control @error('email') is-invalid @enderror value="{{ old('email') }}">
@error('email')
    <div class="alert alert-danger mt-1 p-1">{{ $message }}</div>
@enderror
```
But if you want to show all error message in a single call, you can use below way
```html
@hasflash
    {!! flash()->display() !!}
@endhasflash
```

Zuno will automatically trace the error message and display here.

### Image validation
In Zuno, you can use the $request->sanitize() method to validate and sanitize incoming request data. This ensures that the submitted data meets specific criteria before being processed.
To validate an uploaded file, use the following code:
```php
$request->sanitize([
    'file' => 'required|image|mimes:jpg,png,jpeg|dimensions:min_width=100,min_height=100,max_width=1000,max_height=1000|max:2048'
]);
```

The following validation rules are applied to image uploads to ensure that only properly formatted and sized images are processed.

**Rules:**

* **`required`**:
    * Ensures that a file is provided in the upload request. If no file is present, validation will fail.
* **`image`**:
    * Verifies that the uploaded file is indeed an image. This rule checks the file's MIME type to confirm it's an image format.
* **`mimes:jpg,png,jpeg`**:
    * Restricts the acceptable image file types to JPEG (`jpg`, `jpeg`) and PNG (`png`). Any other image formats or file types will be rejected.
* **`dimensions:min_width=100,min_height=100,max_width=1000,max_height=1000`**:
    * Enforces specific size constraints on the image.
        * `min_width=100` and `min_height=100`: The image must have a minimum width and height of 100 pixels.
        * `max_width=1000` and `max_height=1000`: The image must not exceed a maximum width and height of 1000 pixels.
* **`max:2048`**:
    * Limits the maximum file size to 2048 kilobytes (2 megabytes). Files exceeding this size will be rejected.

**Validation Failure:**

If the uploaded file fails to meet any of these criteria, the validation process will fail. An error response will be generated, indicating the specific validation failures.

**Purpose:**

These validation rules are crucial for maintaining data integrity and ensuring that your application only processes images that comply with the defined specifications. This helps prevent issues such as:

* Corrupted or invalid image files.
* Excessively large images that can impact performance.
* Images with inappropriate dimensions that may distort layouts.
* Security Risks.

<a name="section-26"></a>

## Error Handling
Zuno has HttpException class to throw an exception. You can use it like
```php
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use Zuno\Http\Exceptions\HttpException;

class UserController extends Controller
{

    public function index()
    {
        try {
           // code
        } catch (HttpException $th) {
            return $th->getMessage();
        }
    }
}

```

<a name="section-27"></a>

## Error Logging and System Monitoring

Zuno provides a comprehensive logging system to facilitate application debugging and monitoring. By leveraging the powerful `Monolog` library, Zuno offers flexible logging capabilities, allowing you to record various events and messages to different channels.

**Logging Channels:**

Zuno supports three primary logging channels, configurable via the `config/log.php` file and your `.env` environment variables:

* **`stack`**: Allows you to group multiple log handlers into a single channel.
* **`daily`**: Creates a new log file each day, useful for managing large log volumes.
* **`single`**: Writes all log messages to a single file (`storage/logs/zuno.log`).

**Configuration:**

You can customize the logging behavior by modifying the `config/log.php` file and the `.env` file. This allows you to select your preferred logging channel, adjust log levels, and configure other logging options.

**Basic Logging with the `logger()` Helper:**

Zuno provides a convenient `logger()` helper function to simplify the logging process. To log a message, simply use the following syntax:

```php
logger()->info('Informational message.');
logger()->error('An error occurred.');
logger()->warning('Warning message.');
logger()->debug('Debugging information.');
```
#### Passing array as params
If you need to log array data, follow as like below
```php
logger()->info('message', ['name' => 'Mahedi Hasan', 'spouse' => 'Nure Yesmin']);
```

#### Automatic System Error Logging
Zuno includes a robust, built-in system for automatically capturing and recording system errors. This crucial feature ensures that critical issues are promptly identified, allowing developers to maintain application stability and effectively troubleshoot problems. Zuno automatically logged system error message in `storage/log/zuno.log` file.

**How It Works:**

* **Automatic Capture:**
    * When an uncaught exception or error occurs within your Zuno application, the framework's error handling mechanism automatically intercepts the event.
* **Log Destination:**
    * These captured system error messages are then written to the `storage/logs/zuno.log` file. This central location acts as a comprehensive repository for all system-level errors.
* **Purpose:**
    * This automatic logging empowers developers to:
        * Identify and diagnose the root cause of errors.
        * Monitor application health and stability in real-time.
        * Proactively address potential issues before they escalate.

**Benefits:**

* **Simplified Debugging:**
    * A centralized log of system errors significantly streamlines the debugging process. Developers can quickly review the log to pinpoint the source of problems, saving valuable time and effort.
* **Enhanced Monitoring:**
    * Regularly reviewing the error log enables developers to identify recurring issues, potential vulnerabilities, and performance bottlenecks, leading to a more stable application.
* **Improved Reliability:**
    * By promptly addressing logged errors, developers can enhance the overall reliability and stability of their Zuno application, ensuring a consistent and dependable user experience.

**In Essence:**

Zuno's automatic system error logging acts as a vital safety net, ensuring that critical errors are meticulously recorded for thorough analysis and swift resolution. This proactive approach significantly contributes to the development of a more robust, dependable, and maintainable application.

<a name="section-28"></a>

## Pool Console
Pool is the command line interface included with Zuno. Pool exists at the root of your application as the pool script and provides a number of helpful commands that can assist you while you build your application. To view a list of all available Pool commands, you may use the list command:
```bash
php pool list
```
It will shows all the application commands
**General Commands:**

* `completion`: Dump the shell completion script.
* `help`: Display help for a command.
* `list`: List available commands.

**Database Migration Commands:**

* `migrate`: Runs new database migrations.
* `migrate:fresh`: Rolls back all migrations and re-runs them.

**Cache Management Commands:**

* `cache:clear`: Clear all cache files from the `storage/framework` folder.
* `config:cache`: Cache the configuration files into a single file for faster access.
* `view:cache`: Precompile all views and store them in the `storage/framework/views` folder.
* `view:clear`: Clear all compiled view files from the `storage/framework/views` folder.

**Database Seeding Commands:**

* `db:seed`: Run database seeds.

**Key Generation Commands:**

* `key:generate`: Generate a new application key and set it in the `.env` file.

**Code Generation (Make) Commands:**

* `make:controller`: Creates a new controller class.
* `make:middleware`: Creates a new middleware class.
* `make:model`: Creates a new model class.
* `create:migration`: Creates a new Phinx migration class.
* `create:seed`: Creates a new Phinx seed class.

**Session Management Commands:**

* `session:clear`: Clear all session files from the `storage/framework/sessions` folder.

**Storage Management Commands:**

* `storage:link`: Create a symbolic link from `public/storage` to `storage/app/public`.

**Development Server Command:**

* `start`: Start the Zuno development server.

<a name="section-29"></a>

## Model
Zuno's Model class extented [Laravel Model](https://laravel.com/docs/12.x/eloquent#generating-model-classes). So you can use most the eloquent features in Zuno's model class. Zuno's model located in `App\Models` directory and you can create as many model as you using `create:model` command.

### Create New Model
To create a new model, run this command
```bash
php pool make:model Product
```

This command will generate a new model inside `App\Models` directory. Models generated by the make:model command will be placed in the app/Models directory. Let's see a basic model class.
```php
<?php

namespace App\Models;

use Zuno\Model\Model;

class Product extends Model
{
    //
}
```

<a name="section-30"></a>

## Encryption
Zuno's encryption services provide a simple, convenient interface for encrypting and decrypting text via PHP's OpenSSL using `AES-256` and `AES-128` encryption. All of Zuno's encrypted values are signed using a message authentication code (MAC) so that their underlying value cannot be modified or tampered with once encrypted.

Before using Zuno's encrypter, you must set the key configuration option in your `config/app.php` configuration file. This configuration value is driven by the `APP_KEY` environment variable. You should use the `php pool key:generate` command to generate this variable's value since the `key:generate` command will use PHP's secure random bytes generator to build a cryptographically secure key for your application. Typically, the value of the `APP_KEY` environment variable will be generated for you during Zuno's installation.

#### Encrypting a Value
You may `encrypt` a value using the encryptString method provided `Zuno\Support\Encryption` class. 
```php
<?php

namespace App\Http\Controllers;

use Zuno\Http\Request;
use Zuno\Support\Encryption;

class UserController extends Controller
{
    public function store(Request $request)
    {
        $encrypted = Encryption::encrypt("Hello World");
        
        $encrypted // This is now encrypted
    }
}
```

#### Decrypting a Value
You may `decrypt` a value using the encryptString method provided `Zuno\Support\Encryption` class. 
```php
<?php

namespace App\Http\Controllers;

use Zuno\Http\Request;
use Zuno\Support\Encryption;

class UserController extends Controller
{
    public function store(Request $request)
    {
        $decrypted = Encryption::decrypt($encrypted);
        
        $encrypted // This is now decrypted
    }
}
```

<a name="section-31"></a>
## Collections and Macros
Zuno leverages the power of Laravel's Collections and Macros to enhance data manipulation and code reusability. See [Collection](https://laravel.com/docs/12.x/collections#introduction) and [Macro](https://laravel.com/docs/12.x/http-client#macro) example that how you can use them in Zuno Framework.

To create a custom macro, just update service provider `App\Providers\AppServiceProvider.php` like:
```php
<?php

namespace App\Providers;

use Zuno\DI\Container;
use Illuminate\Support\Collection;

class AppServiceProvider extends Container
{
    /**
     * register.
     *
     * Register any application services.
     * @return	void
     */
    public function register(): void
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

use Zuno\Route;

Route::get('/', function () {

    $collection = collect(['first', 'second']);
    $upper = $collection->toUpper();

    return $upper; //output ["FIRST","SECOND"]
});
```

<a name="section-32"></a>

## Database Connection
To connect the database you have to update `.env` configuration file and database related config file located in `config/database.php`. Zuno currently support only MYSQL database.
```
DB_CONNECTION=mysql
DB_HOST=localhost
DB_PORT=3306
DB_DATABASE=
DB_USERNAME=
DB_PASSWORD=
```
<a name="section-33"></a>

## Database Migration
Zuno allow you to create migration. To create migration, Zuno uses `CakePHP's phinx`. So to create a migration file first you need to update the configuration file environments array that is located in your root project path `config.php`
```php
<?php

return [
    'paths' => [
        'migrations' => './database/migrations',
        'seeds' => './database/seeds',
    ],
    'migration_base_class' => 'Zuno\Migration\Migration',
    'environments' => [
        'default_migration_table' => 'phinxlog',
        'zuno' => [
            'adapter' => 'mysql',
            'host' => 'localhost',
            'name' => '',
            'user' => '',
            'pass' => '',
            'port' => ''
        ]
    ]
];
```
Now once you have configured this file, now migration pool console command will be avaiable for you.

### Create Migration
To create a new migration file, run this command
```bash
php pool create:migration PostMigration
```
Now this command will create `PostMigration` file inside your `database/migrations` folder. Now to know avaiable fields type, see this [Phinx](https://book.cakephp.org/phinx/0/en/migrations.html#working-with-columns) documentation page.

### Create Migrate
To migrate your all file or newly created migration files, run this `migrate` command
```bash
php pool migrate
```
<a name="section-34"></a>

### Database Seeder
No you know that seeder is the most important thing when you develop a web application. Zuno support Database Seeding. Zuno includes the ability to seed your database with data using seed classes. All seed classes are stored in the `database/seeds` directory. To create a Seeder class, run this command
```php
php pool create:seed UserSeeder
```
Now it will generate a file like
```php
<?php

declare(strict_types=1);

use Phinx\Seed\AbstractSeed;

class UserSeeder extends AbstractSeed
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

    }
}
```

Now you can seed data from this class by updating run method. You can update run method like
```php
<?php

declare(strict_types=1);

use Phinx\Seed\AbstractSeed;
use Zuno\Auth\Security\Hash;

class UserSeeder extends AbstractSeed
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
        $data = [
            [
                'name' => $name = fake()->name(),
                'username' => strtolower(str_replace(' ', '_', $name)),
                'email' => fake()->email(),
                'password' => Hash::make('password')
            ]
        ];

        $user = $this->table('users');
        $user->insert($data)->saveData();
    }
}
```

Remember that `fake()` is a global helper function, so you can use it in your entire application where you want.

### Run The Seed
To run the database seeder, there is also pool console command. Run this command to seed database
```bash
php pool db:seed // it will seed all seeder class
php pool db:seed UserSeeder // It will only seed UserSeed class
```

### Fresh Database
To drop all the table and want to rebuild your database schema? You have to run `migrate:fresh` coomand
```bash
php pool migrate:fresh
```

<a name="section-35"></a>

## Hashing Configuration
The `config/hashing.php` file in Zuno allows you to configure password hashing for your application. This configuration determines the algorithm used to securely store user passwords.

**Key Configuration Options:**

* **`driver`**:
    * This option specifies the default hashing algorithm to be used.
    * Zuno supports the following drivers:
        * `bcrypt`: A widely used and secure hashing algorithm.
        * `argon`: A modern and secure hashing algorithm.
        * `argon2id`: The most modern and recommended argon variant.
    * **Default:** `argon2id` (recommended).

* **`bcrypt`**:
    * This section configures options for the `bcrypt` hashing algorithm.
    * **`rounds`**:
        * Controls the number of rounds used in the bcrypt hashing process.
        * Higher rounds increase the time taken for hashing, making it more secure but also slower.
        * The value is read from the `BCRYPT_ROUNDS` environment variable, with a default of 10.
        * Configure this value in your `.env` file.

* **`argon`**:
    * This section configures options for the `argon` hashing algorithm.
    * **`memory`**:
        * Specifies the amount of memory (in kilobytes) used by the argon hashing algorithm.
        * Increasing this value enhances security but increases memory usage.
        * **Default:** `65536`
    * **`threads`**:
        * Specifies the number of threads used by the argon hashing algorithm.
        * Increasing this value can speed up the hashing process on multi-core systems.
        * **Default:** `1`
    * **`time`**:
        * Specifies the number of iterations for the argon hashing algorithm.
        * Increasing this value increases the time taken for hashing, making it more secure.
        * **Default:** `4`

**Usage:**

* To change the default hashing driver, modify the `driver` option in the `config/hashing.php` file.
* To customize the bcrypt rounds, set the `BCRYPT_ROUNDS` environment variable in your `.env` file.
* To change the argon configuration, modify the 'argon' array in the `config/hashing.php` file.

Now we you create hash password using `bcrypt()` or direct calling `Hash::make()` method.
```php
$hashedValue = bcrypt('bcrypt');
$hashedValue // "$2y$10$gtr.qSIRWTDh7uh9ubj5duC0/KwQJcwZ0.KpFPOPzeRClpwo2FRSa"
```
Or you can use 
```php
use Zuno\Auth\Security\Hash;

$hashedValue = Hash::make('password');
$hashedValue // "$2y$10$qcxCuljWvI7e1A5ah6axl.qgNsVoNw3ad8HSDFRmnVxyzIoj5/x8m"
```

### Checking Hash
If you want to check hash data, you need to use `check` method
```php
use Zuno\Auth\Security\Hash;

if (Hash::check('plainText', 'hashedValue')) {
    // Password matched
}
```

### Determining if a Password Needs to be Rehashed
Check if a password hash needs rehashing (security upgrade).
```php
use Zuno\Auth\Security\Hash;

$rehashed = Hash::needsRehash('hashedValue');
```
The needsRehash method provided by the Hash class allows you to determine if the work factor used by the hasher has changed since the password was hashed. Some applications choose to perform this check during the application's authentication process:
```php
if (Hash::needsRehash($hashed)) {
    $hashed = Hash::make('plain-text');
}
```

<a name="section-36"></a>

## Authentication
Zuno provides built-in authentication features, simplifying user login and logout processes. This section details how to use these features within your application.

### Login Functionality

Zuno's authentication system allows you to easily verify user credentials and establish a login session.

**Implementation:**

1.  **Controller Setup:**
    * Use the `Zuno\Auth\Security\Auth` trait within your controller to access authentication methods.

2.  **Login Method:**
    * Utilize the `sanitize` method to validate and sanitize user input (e.g., email and password).
    * Call the `Auth::establishSession()` method, passing the validated credentials (`$request->passed()`).
    * If `Auth::establishSession()` returns `true`, the user is successfully logged in.
y you will get login and logout features. To do login
```php
<?php

namespace App\Http\Controllers\Auth;

use Zuno\Http\Request;
use Zuno\Auth\Security\Auth;
use App\Http\Controllers\Controller;

class LoginController extends Controller
{
    use Auth;

    public function login(Request $request)
    {
        $request->sanitize([
            'email' => 'required|email|min:2|max:100', // you can also pass 'username' rather than 'email'
            'password' => 'required|min:2|max:20',
        ]);

        if (Auth::establishSession($request->passed())) {
            // User is logged in
        }
    }
}

```

### Logout
To destroy user session, simply call `logout` function.
```php
<?php

namespace App\Http\Controllers\Auth;

use Zuno\Http\Request;
use Zuno\Auth\Security\Auth;
use App\Http\Controllers\Controller;

class LoginController extends Controller
{
    use Auth;

    public function logout()
    {
        Auth::logout(); // User login session is now destroyed
    }
}

```
<a name="section-37"></a>

## Mail Configuration
Zuno provides a convenient way to setup your mail configuration. Zuno currently support only `smtp` driver for mail configuration. Need to update `.env`'s mail configuration before starting with mail features. Zuno usage `PHPMailer` tp send mail.
```
MAIL_MAILER=smtp
MAIL_HOST=
MAIL_PORT=2525
MAIL_USERNAME=
MAIL_PASSWORD=
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS="hello@example.com"
MAIL_FROM_NAME="${APP_NAME}"
```

Now you can check also mail related configuration from `config/mail.php` file. Remember, if you update, `config/mail.php` file, don't forget to clear your system cache.
```bash
php pool cache:clear // to clear the cache
php pool config:cache // to cache again
```

Now if you setup with your `smtp` mail credentials, now you are ready to go.

<a name="section-38"></a>

## Sending Mail
When building Zuno applications, each type of email sent by your application is represented as a "mailable" class. These classes are stored in the `app/Mail` directory. Don't worry if you don't see this directory in your application, since it will be generated for you when you create your first mailable class using the `make:mail` Pool command:
```bash
php pool make:mail InvoicMail
```

### Configuring the Sender
You specify a global "`from`" address in your `config/mail.php` configuration file. This address will be used to send mail.
```
'from' => [
    'address' => env('MAIL_FROM_ADDRESS', 'hello@example.com'),
    'name' => env('MAIL_FROM_NAME', 'Example'),
],
```

### Configuring the Subject
By time to time, every Mail has a subject. Zuno allows you to define a Mail subject in a very convenient way. To define Mail subject, just need to update the subject method from your mailable class.
```php
/**
 * Define mail subject
 * @return Zuno\Support\Mail\Mailable\Subject
 */
public function subject(): Subject
{
    return new Subject(
        subject: 'New Mail'
    );
}
```

### Configuring the View
Within a mailable class's content method, you may define the view, or which template should be used when rendering the email's contents. Since each email typically uses a Blade template to render its contents, you have the full power and convenience of the Blade templating engine when building your email's HTML:
```php
/**
 * Set the message body and data
 * @return Zuno\Support\Mail\Mailable\Content
 */
public function content(): Content
{
    return new Content(
        view: 'Optional view.name'
    );
}
```

If you want to pass the data without views, you can pass string or array data.
```php
public function content(): Content
{
    return new Content(
        data: [
           'order_status' => true
        ]
    );
}
```

Even you can send mail without passing any data to Content. Suppose you just want to pass attachment only. you can do in this case, just make content empty.
```php
public function content(): Content
{
    return new Content();
}
```

### Complete Example of Sending Mail
Zuno provides `to` and `send` method primaritly to send a basic mail.
```php
<?php

namespace App\Http\Controllers;

use Zuno\Support\Mail\Mail;
use App\Models\User;
use App\Http\Controllers\Controller;
use App\Mail\InvoiceMail;

class OrderController extends Controller
{
    public function index()
    {
        $user = User::find(1);

        $data = [
            'order_status' => 'success',
            'invoice_no' => '123-123'
        ];

        Mail::to($user)->send(new InvoiceMail($data));
    }
}
```

Now update your `InvoiceMail` mailable class like
```php
<?php

namespace App\Mail;

use Zuno\Support\Mail\Mailable\Subject;
use Zuno\Support\Mail\Mailable\Content;
use Zuno\Support\Mail\Mailable;

class TestMail extends Mailable
{
    public function __construct(protected $data) {}

    public function subject(): Subject
    {
        return new Subject(
            subject: "Order Shipped Confirmation"
        );
    }

    public function content(): Content
    {
        return new Content(
            view: 'emails.order.invoice', // 'resources/views/emails/order/invoice.blade.php'
            data: $this->data // Passing data will be available in invoice.blade.php, access it via {{ $data }}
        );
    }

    public function attachment(): array
    {
        return [];
    }
}
```

<a name="section-39"></a>

## Sending Mail with Attachment
To send mail with attachment, you have to pass data attachment path using `attachment` method.
```php
public function attachment(): array
{
    return [
        storage_path('invoice.pdf') => [
            'as' => 'rename_invoice.pdf', // The file will be sent using this name
            'mime' => 'application/pdf',  // file mime types
        ]
    ];
}
```

### Multiple Attachments with Mime Types
You can also send mail with multiple attachment. Just pass your file arrays in the attachment method like
```php
public function attachment(): array
{
    return [
        storage_path('invoice_1.pdf') => [
            'as' => 'invoice_3.pdf',
            'mime' => 'application/pdf',
        ],
        storage_path('invoice_2.pdf') => [
            'as' => 'invoice_3.pdf',
            'mime' => 'application/pdf',
        ],
        storage_path('invoice_3.pdf') => [
            'as' => 'invoice_3.pdf',
            'mime' => 'application/pdf',
        ],
    ];
}
```

Here both `as` and `mime` is optional, you can simply send email attachment like
```php
public function attachment(): array
{
    return [
        storage_path('invoice_1.pdf'),
        storage_path('invoice_2.pdf'),
        storage_path('invoice_3.pdf')
    ];
}
```
<a name="section-40"></a>
## Sending Mail with CC and BCC
You are not limited to just specifying the "to" recipients when sending a message. You are free to set "to", "cc", and "bcc" recipients by chaining their respective methods together:
```php
use Zuno\Support\Mail\Mail;

Mail::to($request->user())
    ->cc($moreUsers)
    ->bcc($evenMoreUsers)
    ->send(new OrderShipped($order));
```