1- composer require tymon/jwt-auth
_____________________________________
2-
Add the service provider to the providers array in the config/app.php config file as follows:

'providers' => [

    ...

    Tymon\JWTAuth\Providers\LaravelServiceProvider::class,
]
______________________________________
3- php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\LaravelServiceProvider"
4- php artisan jwt:secret
______________________________________
5- Firstly you need to implement the Tymon\JWTAuth\Contracts\JWTSubject contract on your User model,
 which requires that you implement the 2 methods getJWTIdentifier() and getJWTCustomClaims().
 Find User.php file
______________________________________
6- Inside the config/auth.php file you will need to make a few changes
to configure Laravel to use the jwt guard to power your application authentication.

'defaults' => [
    'guard' => 'web',
    'passwords' => 'users',
],

...

'guards' => [
    ...
	...
    ,
    'api' => [
        'driver' => 'jwt',
        'provider' => 'users',
    ],
],
________________________________________________________________-
7- Put "JwtMiddleware.php" file in dir: app\Http\Middleware
________________________________________________________________
8- Add
 'jwt.verify' => \App\Http\Middleware\JwtMiddleware::class,
in app\Http\Kernel.php  in $routeMiddleware array
_______________________________________________________________
8- User jwt.auth in your routes\web.php
Route::group(['middleware' => 'jwt.verify',], function ($router) {
    Route::post('login', 'AuthController@login');
});
_______________________________________________________________
9- Go to dir: your-project\vendor\laravel\ui\auth-backend\AuthenticatesUser.php
Add methods: loginAPI, createNewToken  to it, find these method in AuthenticatesUser.php
Modify the data in "return" in "loginAPI" method as you want



Notes:
* You may need to handle OPTIONS request, in your request handler, check if type is OPTIONS, return it without middleware

* 
