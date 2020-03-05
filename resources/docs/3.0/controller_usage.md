# Controller Usage

---

- [Controller Usage](#controller-usage)

<a name="controller-usage"></a>
## Controller Usage
It is a common use case to store data using an api and wanting to retrieve data dynamically in the same call. You can add the dynamic Jory functionality to your own controllers using the ```JosKolenberg\LaravelJory\Facades\Jory``` facade.

```php
public function store(Request $request)
{
	$user = User::create([
		'name' => $request->input('name')
	]);
	
	return Jory::on($user);
}
```
The example above will use the Jory Query from the ```jory``` parameter in the request, and return the data accordingly.

The facade can also be used on existing queries or a model's class name.
```php
return Jory::on(User::where('active', true));

return Jory::on(User::class);
```

> {info} In contrast to the regular jory endpoints, using the ```Jory::on``` method will only return data if a ```jory``` parameter is present in the request.
