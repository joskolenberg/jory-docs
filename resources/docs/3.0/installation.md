# Installation

---

- [Install](#install)
- [Publish Config](#config)
- [Create Facade Alias](#alias)

<a name="install"></a>
## Install
Pull the package in using composer.
```shell script
composer require joskolenberg/laravel-jory
```
> {warning} Laravel Jory 3.x requires Laravel 7.x or higher. Use the 2.x version for Laravel 5.8 to 6.x.

<a name="config"></a>
## Publish Config
Optionally you can publish the config file to tweak some settings.
```shell script
php artisan vendor:publish --provider="JosKolenberg\LaravelJory\JoryServiceProvider"
```

<a name="alias"></a>
## Create Facade Alias
And if you want to use the facade with an alias you can add it to your aliases array in the ```app.php``` config file.
```php
'aliases' => [
    ...
    'Jory' => JosKolenberg\LaravelJory\Facades\Jory::class,

],
```

