# Generator

---

- [Using the Generator](#usage)
- [Options](#options)

<a name="usage"></a>
## Using the Generator
Laravel Jory comes with a convenient generator command which pre-configures your Jory Resources by using reflection on the model class.
```shell script
php artisan jory:generate --model="\App\User"
```
Alternatively you can skip the model option and pick your model from a list.
```shell script
php artisan jory:generate
```
```shell script
For which model would you like to generate?:
  [0 ] All models
  [1 ] App\Contact
  [2 ] App\Employee
  [3 ] App\User
```

<a name="options"></a>
## Options
Use the ```force``` option to overwrite any existing Jory Resoures
```shell script
php artisan jory:generate --model="\App\User" --force
```
Use the ```all``` option to generate a Jory Resoures for each of your models
```shell script
php artisan jory:generate --all
```
