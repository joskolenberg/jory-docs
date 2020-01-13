# Configuring Offset & Limit

---

- [Limit](#limit)
- [Offset](#offset)

<a name="limit"></a>
## Limit
Use the ```limitDefault``` option to set a default limit when no limit is defined in the Jory Query, this can prevent returning large datasets whenever an API consumer didn't set a limit. This limit can be overwritten in the Jory Query.
```php
protected function configure(): void
{
    ...
    $this->limitDefault(100);
    ...
}
```

Whenever you want to limit the maximum amount of records that possibly can be fetched use the ```limitMax``` option. A request which exceeds the ```limitMax``` in a Jory Query will result in a ```422``` error response.
```php
protected function configure(): void
{
    ...
    $this->limitMax(1000);
    ...
}
```

> {warning} Check the [issues](/{{route}}/{{version}}/known_issues#relation-limits) when setting limits on relations.

<a name="offset"></a>
## Offset
No configuration options available for offset.
