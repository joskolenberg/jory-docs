# Configuring Relations

---

- [Registering Relations](#registering)

<a name="registering"></a>
## Registering Relations
To register a relation use the ```relation``` method, the relation's name should match the name of the relation in your model.

```php
protected function configure(): void
{
    ...
    $this->relation('albums');
    ...
}
```
> {info} A request which tries to fetch a relation which is not configured results in a ```422``` error response.

Whenever a relation is requested the parser will look for the registered Jory Resource for the related model and use it to process the Jory Query on the relation.
>> {warning} Be sure to have a Jory Resource for the related model when registering a relation.
