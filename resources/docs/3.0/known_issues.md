# Known Issues

---

- [Large request parameter](#post-routes)
- [MorphTo Relations](#morph)
- [Relations and Limits](#relation-limits)
- [Base64 Support](#base64)

<a name="post-routes"></a>
## Large Request Parameter
Jory Queries can become quite large. The ```GET``` requests for fetching this parameter might in extreme scenarios become too large.
If you would ever run into this problem a possible solution would be to skip the ```REST``` standards and create some POST routes which refer to the same controller methods.
```php
...
Route::post('/{resource}', [\JosKolenberg\LaravelJory\Http\Controllers\JoryController::class, 'get']);
...
```
> {info} This issue is considered too much edge-case to provide the ```POST``` routes out-of-the-box.

<a name="morph"></a>
## MorphTo Relations
MorphTo relations cannot be used by Jory due their dynamic nature. However, an easy workaround can be made by creating a separate belongsTo relation for each "morphable" type.

<a name="relation-limits"></a>
## Relations and Limits
When loading relations on collections Jory will automatically eager load them on the complete collection to optimize the querying as much as possible. Thereby any offsets and limits on a relation could lead to unexpected results since the offset and limit are applied to the total collection instead of per model.

<a name="base64"></a>
## Base64 Support
The Jory api may be consumed with base64 encoded json strings as well to provide prettier urls. As a bonus this can also resolve some edge-case problems when using reserved keywords in your json string combined with webserver restrictions.  
```shell script
GET /jory/band?jory=eyJmaWx0ZXIiOnsiZiI6Im51bWJlcl9vZl9hbGJ1bXNfaW5feWVhciIsIm8iOiI9IiwiZCI6eyJ5ZWFyIjoxOTcwLCJ2YWx1ZSI6MX19LCJybHQiOnsiYWxidW1zIjp7ImZsdCI6eyJmIjoiaGFzX3Nvbmdfd2l0aF90aXRsZSIsIm8iOiJsaWtlIiwiZCI6IiVsb3ZlJSJ9LCJybHQiOnsiY292ZXIiOnt9fX0sInNvbmdzIjp7ImZsdCI6eyJmIjoidGl0bGUiLCJvIjoibGlrZSIsImQiOiIlYWMlIn0sInJsdCI6eyJhbGJ1bSI6eyJybHQiOnsiYmFuZCI6e319fX0sImZsZCI6WyJpZCIsInRpdGxlIl19LCJwZW9wbGUiOnsiZmxkIjpbImlkIiwibGFzdF9uYW1lIl0sInJsdCI6eyJpbnN0cnVtZW50cyI6e319fX19
```
equals
```shell script
GET /jory/band?jory={"filter":{"f":"number_of_albums_in_year","o":"=","d":{"year":1970,"value":1}},"rlt":{"albums":{"flt":{"f":"has_song_with_title","o":"like","d":"%love%"},"rlt":{"cover":{}}},"songs":{"flt":{"f":"title","o":"like","d":"%ac%"},"rlt":{"album":{"rlt":{"band":{}}}},"fld":["id","title"]},"people":{"fld":["id","last_name"],"rlt":{"instruments":{}}}}}
```
> {warning} Adding base64 encoding does not provide any security. For real security use the other [security](/{{route}}/{{version}}/security) options instead.
