# Security

---

- [Authentication](#authentication)
- [Scoping Results](#scoping)
- [Limiting Fields](#fields)

<a name="authentication"></a>
## Authentication
Once you've generated or written a Jory Resources without further actions, your Jory Resources are publicly available.
Add the ```auth``` middleware in the jory config file to require a user to be logged in to consume the api.  
<br>
jory.php
```php
    ...

    'routes' => [

        'enabled' => true,

        'path' => '/jory',

        'middleware' => [
            'api',
            'auth',
            \JosKolenberg\LaravelJory\Http\Middleware\SetJoryHandler::class
        ],

    ],
    ...
```

<a name="scoping"></a>
## Scoping Results
If you want to limit the available database records (probably based on the user being logged in), override the ```authorize()``` method in the JoryResource to filter down to what you want to be visible for the current user. This method receives a ```Builder``` object and the current user as parameters.
Alternatively you could use Laravel's built in [global scopes](https://laravel.com/docs/6.0/eloquent#global-scopes).
```php
public function authorize($builder, $user = null): void
{
    if(!$user->isSuperAdmin()){
        $builder->where('user_id', '=', $user->id);
    }
}
```  

<a name="fields"></a>
## Limiting Fields
If you don't want to expose some of the fields in your Jory api, just don't register them. Since you have to be explicit about what is available any unregistered fields will not be open for the api.
