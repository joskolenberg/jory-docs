# Configuring Fields

---

- [Registering Fields](#registering)
- [Custom Attributes](#custom-attributes)
- [Eager Loading](#eager)
- [Filtering and Sorting](#filtering-sorting)

<a name="registering"></a>
## Registering Fields
Only attributes which are explicitly configured in the Jory Resource can be fetched using the API. A model's database attribute can be registered using the ```field()``` method.

```php
protected function configure(): void
{
    $this->field('id');
    $this->field('name');
    ...
}
```
> {info} A request which requests a field which is not configured results in a ```422``` error response.

<a name="custom-attributes"></a>
## Custom Attributes
A model's [custom attribute](https://laravel.com/docs/6.x/eloquent-mutators#accessors-and-mutators) can be registered using the same ```field()``` method.

```php
protected function configure(): void
{
    $this->field('total_amount');
}
```

When you want to add a custom attribute to your Jory API without defining it in your model you can add it as a second parameter. The referenced class must implement the ```JosKolenberg\LaravelJory\Attributes\Attribute``` interface.  
OrderTotalAmount.php
```php
use Illuminate\Database\Eloquent\Model;
use JosKolenberg\LaravelJory\Attributes\Attribute;

class OrderTotalAmount implements Attribute
{

    /**
     * Get the attribute for the model.
     *
     * @param Model $order
     * @return mixed
     */
    public function get(Model $order)
    {
        return $order->orderlines->sum('amount');
    }
}
```
OrderJoryResource.php
```php
protected function configure(): void
{
    $this->field('total_amount', new OrderTotalAmount);
}
```

<a name="eager"></a>
## Eager Loading
When a custom attribute uses relations, fetching multiple models with this attribute could lead to N+1 problems. Use the ```load``` method to eager load any relations whenever the particular field is requested.

```php
protected function configure(): void
{
    $this->field('total_amount', new OrderTotalAmount)->load(['orderlines']);
}
```

<a name="filtering-sorting"></a>
## Filtering and Sorting
When registering a database attribute you probably want to be able to filter and sort by this column as well. To prevent registering this all separately the ```filterable``` and ```sortable``` methods are available when configuring a field. 
```php
protected function configure(): void
{
    $this->field('name')->filterable()->sortable();
}
```
A callback can be added to set additional options.
```php
protected function configure(): void
{
    $this->field('name')->filterable(function (Filter $filter){
            $filter->scope(new CustomNameFilter);
            $filter->operators(['=', 'like']);
        })->sortable(function(Sort $sort){
            $sort->scope(new CustomNameSort);
            $sort->default();
        });
}
```

> {info} Jory is totally independent of Eloquent's ```visible``` and ```hidden``` properties, so any ```visible``` or ```hidden``` settings on the model DON'T affect the result using Jory.

