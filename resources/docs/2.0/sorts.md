# Configuring Sorts

---

- [Registering Sorts](#registering)
- [Custom Sorts](#custom-sorts)
- [Available Operators](#operators)
- [Registering on a Field](#field)

<a name="registering"></a>
## Registering Sorts
To register a sort option use the ```sort``` method.

```php
protected function configure(): void
{
    ...
    $this->sort('name');
    ...
}
```
The sort will be performed by applying an ```orderBy``` on a column with the same name.
> {info} A request which tries to apply a sort which is not configured results in a ```422``` error response.

<a name="custom-sorts"></a>
## Custom Sorts
When you want to add a custom sort option to your Jory API you can add it as a second parameter. This class must implement the ```JosKolenberg\LaravelJory\Scopes\SortScope``` interface.  
BandNameSort.php
```php
use JosKolenberg\LaravelJory\Scopes\SortScope;

class BandNameSort implements SortScope
{
    public function apply($builder, string $order = 'asc'): void
    {
        $builder->join('bands', 'band_id', 'bands.id')->orderBy('bands.name', $order);
    }
}
```
MusicianJoryResource.php
```php
protected function configure(): void
{
    ...
        $this->sort('band_name', new BandNameSort);
    ...
}
```
<a name="default-sorts"></a>
## Default Sorts
Use the ```default``` option to apply a sort on every query. When using multiple default sorts, use the ```index``` parameter to define in which order the sorts must be applied.
Any sort options in the request will always take precedence over the default ones.
```php
protected function configure(): void
{
    ...
    $this->sort('first_name')->default(2);
    $this->sort('last_name')->default(1, 'desc');
    $this->sort('title');
    ...
}
```
The following request will fetch musicians sorted by 1. ```title asc```, 2. ```last_name desc``` and 3. ```first_name asc```.
```javascript
axios.get('jory/musician', {
    params: {
        jory: {
            sorts: ['title']
        }
    }
});
```
<a name="field"></a>
## Registering on a Field
If you want to register a field and a sort option on the same column in one go use the ```sortable``` helper method when configuring a field.
```php
protected function configure(): void
{
    ...
    $this->field('name')->sortable();
    ...
}
```
A callback can be added to set additional options.
```php
protected function configure(): void
{
    ...
    $this->field('name')->sortable(function ($sort){
            $sort->scope(new CustomNameSort);
            $sort->default();
        });
    ...
}
```
