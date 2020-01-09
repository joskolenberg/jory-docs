# Quickstart

---

- [Creating the Jory API](#api)
- [Fetching data](#fetching)

<a name="api"></a>
## Creating the Jory API
The quickest (but not always recommended) way to get started is to turn all your Eloquent models into a Jory API with a single command. 
```shell script
php artisan jory:generate --all
```
This command generates a JoryResource for each of your models in the ```\App\Http\JoryResources``` namespace.

> {warning} Without further actions your complete database could now be exposed to the world. Be sure to add the [auth middleware](/{{route}}/{{version}}/authentication) and/or [scope](/{{route}}/{{version}}/scoping) your data before putting your code into production!

<a name="fetching"></a>
## Fetching data
