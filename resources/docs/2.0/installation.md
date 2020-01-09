# Installation

---

- [Install](#install)
- [Publish Config](#config)
- [Create Facade Alias](#alias)

<a name="install"></a>
## Install
You only need to pull the package in using composer.
```shell script
composer require joskolenberg/laravel-jory
```
> {warning} Laravel Jory 2.0 requires Laravel 5.8 or higher.

<a name="config"></a>
## Publish Config
Optionally you can publish the config file if you need to tweak some settings.
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

