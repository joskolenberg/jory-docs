# Configuring Filters

---

- [Registering Filters](#registering)
- [Custom Filters](#custom-filters)
- [Filtering on Relations](#relations)
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
The filter will be performed by applying a ```where``` on a column with the same name.
> {info} A request which tries to apply a filter which is not configured results in a ```422``` error response.

<a name="custom-filters"></a>
## Custom Filters
When you want to add a custom filter to your Jory API you can add it as a second parameter. This class must implement the ```JosKolenberg\LaravelJory\Scopes\FilterScope``` interface.    
FullNameFilter.php
```php
use JosKolenberg\LaravelJory\Scopes\FilterScope;

class FullNameFilter implements FilterScope
{
    public function apply($builder, string $operator = null, $data = null): void
    {
        $builder->where('first_name', $operator, $data);
        $builder->orWhere('last_name', $operator, $data);
    }
}
```
MusicianJoryResource.php
```php
protected function configure(): void
{
    ...
    // Search for musicians with a first or last name matching the search criteria
    $this->filter('full_name', new FullNameFilter);
    ...
}
```
> {info} All filters are scoped, so any ```orWhere``` in a custom filter won't affect the rest of the query.

<a name="relations"></a>
## Filtering on Relations
Filtering on a relation's field doesn't require a custom ```FilterScope``` class, all registered filters in dot notation will automatically be translated into a filter on the relation using Laravel's ```whereHas```.  
MusicianJoryResource.php
```php
protected function configure(): void
{
    ...
    // Search for musicians playing in a band which has an album with a name matching the search criteria
    $this->filter('band.albums.name');
    ...
}
```
> {info} All filters are scoped, so any ```orWhere``` in a custom filter won't affect the rest of the query.

<a name="operators"></a>
## Available Operators
The following operators are available for any filter: ```=```, ```!=```, ```<>```, ```>```, ```>=```, ```<```, ```<=```, ```<=>```, ```like```, ```not_like```, ```is_null```, ```not_null```, ```in``` and ```not_in```.  
Use the ```operators``` method if you want to change the operators for a filter.
```php
protected function configure(): void
{
    ...
    $this->filter('number_of_albums_in_year', new NumberOfAlbumsInYearFilter)->operators(['=', '>', '<']);
    ...
}
```
> {info} A request which tries to apply an operator which is not available results in a ```422``` error response.

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
    $this->field('name')->filterable(function ($filter){
            $filter->scope(new CustomNameFilter);
            $filter->operators(['=', 'like']);
        });
    ...
}
```
