# Explicit Select

---

- [Explicit Select](#explicit-select)

<a name="explicit-select"></a>
## Explicit Select
Even when only a few fields are requested, all database queries will by default always be executed selecting all database fields. E.g. ```SELECT users.*```
With large datasets this could however lead to large memory usage even if only the ```id``` field is requested for a model.
Call the ```explicitSelect()``` method to select only the requested fields in the database queries.

This could however lead to unexpected results when fetching custom attributes which rely on other database fields since they could now not always be present on the model.
So, in addition; call the ```select()``` method when registering a field which relies on other fields than it's own. Or use ```noSelect()``` for custom attributes which don't use any fields from the current model to prevent the name of the custum attribute to be added to the select clause in the query.   
<br>
Example:
```php
    protected function configure(): void
    {
        $this->explicitSelect();

        // Fields
        $this->field('id')->filterable()->sortable(); // Standard database column, no problem
        $this->field('first_name')->filterable()->sortable(); // Standard database column, no problem
        $this->field('last_name')->filterable()->sortable(); // Standard database column, no problem
        $this->field('full_name')->select(['first_name', 'last_name']); // Full name is custom attribute which relies on first and last name
        $this->field('total_revenue')->noSelect(); // Total revenue doesn't use any fields from the current model
    }
```

> {info} The ```explicitSelect``` option is smart enough to detect any primary or foreign keys which need to be added to the query for any requested relations to be loaded.

