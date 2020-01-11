# Creating Jory Resources

---

- [What is a Jory Resource?](#what-is)
- [Registering Jory Resources](#registering)
- [Setting up the Basics](#basics)
- [Configuring](#config)
- [A Note on Relations](#relations)

<a name="what-is"></a>
## What is a Jory Resource?
A Jory Resource is much like Laravel's built-in Resource classes but can be called dynamically using [Jory Queries](/{{route}}/{{version}}/query_introduction).
Inside a Jory Resource is defined what can be fetched (and how) for a specific Eloquent Model, this includes:
- [Fields](/{{route}}/{{version}}/fields) (which database and custom attributes can be fetched)
- [Filters](/{{route}}/{{version}}/filters) (by which columns and custom filters an be filtered)
- [Sorts](/{{route}}/{{version}}/sorts) (by which columns and custom sorts an be sorted)
- [Relations](/{{route}}/{{version}}/relations) (which relations can be fetched)
- [Offset & Limit](/{{route}}/{{version}}/offset_and_limit)

Jory Resources must extend the ```JosKolenberg\LaravelJory\JoryResource``` class and can best be created using the [generator](/{{route}}/{{version}}/generator) command.

> {info} All configuration is explicit so anything that's not defined in a Jory Resource can never be fetched. Models without a Jory Resource are completely disabled for the Jory API.

<a name="registering"></a>
## Registering
All Jory Resources are autoregistered as long as their located in the ```\App\Http\JoryResources``` namespace. Registering manually is possible using the Facade.
```php
Use JosKolenberg\LaravelJory\Facades\Jory;

Jory::register(CustomJoryResource::class);
```

<a name="basics"></a>
## Setting up the Basics
### Related Model
A Jory Resource must be linked to an Eloquent model using the ```$modelClass``` attribute.
```php
namespace App\Http\JoryResources;

use JosKolenberg\LaravelJory\JoryResource;

class UserJoryResource extends JoryResource
{
    protected $modelClass = \App\User::class;
}
```
### Uri
By default the model's Jory API will be available using the kebab-cased model name. For example; a UserGroup model can be fetched at ```domain/jory/user-group```.
This uri can be changed using the ```$uri``` attribute on the Jory Resource.
```php
namespace App\Http\JoryResources;

use JosKolenberg\LaravelJory\JoryResource;

class UserGroupJoryResource extends JoryResource
{
    protected $modelClass = \App\UserGroup::class;

    protected $uri = 'usergroup';
}
```
If you want to disable the endpoint for a model completely the ```$routes``` property can be set to false. This can be convenient if you don't want the model to be fetched directly but it should be available as another model's relation (e.g. ```orderlines``` which are typically only fetched as a part of an ```order```).
```php
namespace App\Http\JoryResources;

use JosKolenberg\LaravelJory\JoryResource;

class OrderlineJoryResource extends JoryResource
{
    protected $modelClass = \App\Orderline::class;

    protected $routes = false;
}
```

<a name="config"></a>
## Configuring
All configuring (what can be fetched and how) must be done in the ```configure``` method, more details on this in the next chapters.  
Little example:
```php
namespace App\Http\JoryResources;

use JosKolenberg\LaravelJory\JoryResource;

class UserGroupJoryResource extends JoryResource
{
    protected $modelClass = \App\UserGroup::class;

    protected function configure(): void
    {
        // Fields
        $this->field('id')->filterable()->sortable();
        $this->field('name')->filterable()->sortable();

        // Custom attributes
        $this->field('user_count');

        // Custom filters
        $this->filter('popular', new \PopularUserGroupFilter);

        // Custom filters
        $this->sort('popular', new \PopularUserGroupSort);

        // Relations
        $this->relation('users');
    }
}
```

<a name="relations"></a>
## A Note on Relations
Whenever a relation is fetched the parser will look for a registered Jory Resource for the related model and use this one. This means that:
- You only write a Jory Resource for each model once, and use it everywhere.
- Models which will only be queried via another model's relation still need their own Jory Resource.
