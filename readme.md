# ElfStack/Router
## Summary
 This is a simple router, some feature is just like Laravel's, but it's easier to use.
 I hope this very very simple tool could help you a little bit, and i'm glad to get advice from you :)

## Usage
```
composer require elfstack/router
```
 And you can just use it in your project!

### How to create a route
 You can use simple `route` method to create a route or use `{http_verb}` method to create a route for specific HTTP requests.
 Every method that create a route recieve two arguments:
 > uri, route

 `uri` is a string that matches the uri, you can use regex exp in that string, and anything that matches the regex exp will be passed to the function you provided. Also, you can use `{anything_you_want_here}` just like using `(^[/]+)` cause we replace `({[\w ]+})` that exp. We also replace `*` to `.*`, so you can simply use `*` to match all uri.

 `route` can be a string or an array or a callable array/closure function.
 **callable**: We just use `call_user_func_array` to call it.
 **string**: The string will be parsed like this: `((file#)(namespace\)class@)method`
 **array**: Just like the string, you can give us an array which look like this:
```
[
	'file'   => 'foo.php',                 // could be ignored
	'class'  => 'Namespace\ClassName',     // could be ignored
	'method' => 'methodName'
]
```

### using HTTP Verb or not
 You can create a simple route by using `route` method, or match the specific HTTP Verb using `{verb}` method.

### Route group
 You can use `group` function to create a route group, and you can specific a namespace for each routes in the function so that Router can automatically look for the class you give in that namespace.

## Examples
 index.php
```
<?php
require 'vendor/autoload.php';

use ElfStack\Router;

Router::route('/', function () {
	echo 'Hello World!';
});

Router::group(['namespace' => 'Controllers'], function () {
	Router::get('/register', 'User@register');
	Router::get('/user/{username}', 'User@profile');
});

Router::post('/login', 'Controllers\User@login');

// You can also specific a file to load
Router::get('/about', __DIR__ . '/about.php@foo');

Router::route('*', function () {
	header('HTTP/1.1 404 Not Found');
	echo 'Sorry, the page could not be found!');
});
```

 Controllers\User.php
```
<?php
namespace Controllers;
class User
{
	public function register() {...}
	public function profile($username) {...}
	public function login() {...}
}
```
