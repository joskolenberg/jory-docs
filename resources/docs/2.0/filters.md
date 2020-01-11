# Filters

---

- [Registering Filters](#registering)
- [Custom Filters](#custom-filters)
- [Available Operators](#operators)
- [Registering on a Field](#field)

<a name="registering"></a>
## Registering Filters
To register a filter option use the ```filter``` method.

```php
protected function configure(): void
{
    ...
    $this->filter('name');
    ...
}
```
By default the filter will be performed by applying a ```where``` on a column with the same name.
> {info} A request which applies a filter which is not configured results in a ```422``` error response.

<a name="custom-filters"></a>
## Custom Filters
When you want to add a custom filter to your Jory API you can add a second parameter. This class must implement the ```JosKolenberg\LaravelJory\Scopes\FilterScope``` interface.  
NumberOfAlbumsInYearFilter.php
```php
use JosKolenberg\LaravelJory\Scopes\FilterScope;

class NumberOfAlbumsInYearFilter implements FilterScope
{
    public function apply($builder, string $operator = null, $data = null): void
    {
        // The $data parameter can be an array
        $year = $data['year'];
        $value = $data['value'];

        $builder->whereHas('albums', function ($builder) use ($year) {
            $builder->where('release_date', '>=', $year.'-01-01');
            $builder->where('release_date', '<=', $year.'-12-31');
        }, $operator, $value);
    }
}
```
BandJoryResource.php
```php
protected function configure(): void
{
    ...
    $this->filter('number_of_albums_in_year', new NumberOfAlbumsInYearFilter);
    ...
}
```
<a name="operators"></a>
## Available Operators
By default the following operators are available for any filter: ```=```, ```!=```, ```<>```, ```>```, ```>=```, ```<```, ```<=```, ```<=>```, ```like```, ```not_like```, ```is_null```, ```not_null```, ```in``` and ```not_in```.  
Use the ```operators``` method if you want to change the operators for a filter.
```php
protected function configure(): void
{
    ...
    $this->filter('number_of_albums_in_year', new NumberOfAlbumsInYearFilter)->operators(['=', '>', '<']);
    ...
}
```
> {info} A request which applies an operator which is not available results in a ```422``` error response.

<a name="field"></a>
## Registering on a Field
If you want to register a field and a filter on the same column in one go you could use the ```filterable``` helper method when configuring a field.
```php
protected function configure(): void
{
    ...
    $this->field('name')->filterable();
    ...
}
```
A callback can be added to set additional options.
```php
protected function configure(): void
{
    ...
    $this->field('name')->filterable(function (Filter $filter){
            $filter->scope(new CustomNameFilter);
            $filter->operators(['=', 'like']);
        });
    ...
}
```
