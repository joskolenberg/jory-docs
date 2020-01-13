# Using Filters

---

- [Applying Filters](#applying)
- [Single Filters](#single-filters)
- [And/Or Groups](#groups)

<a name="applying"></a>
## Applying Filters
Use the ```filter``` or minified ```flt``` key to add a filter to your Jory Query.  

<a name="single-filters"></a>
## Single Filters
A basic filter consists of a ```field``` (```f```), ```operator``` (```o```) and ```data``` (```d```) key, the ```operator``` defaults to an "equals" comparison when omitted.  
<br>
Return only the active users:
```javascript
axios.get('jory/user', {
    params: {
        jory: {
            filter: {
                field: 'is_active',
                data: true
            }
        }
    }
});
```
<br>
Return all musicians with ```first_name``` like ```John```.
```javascript
axios.get('jory/musician', {
    params: {
        jory: {
            filter: {
                field: 'first_name',
                operator: 'like',
                data: '%John%'
            }
        }
    }
});
```


<a name="groups"></a>
## And/Or Groups
Applying multiple filters can be done using the ```group_and``` (```and```) or ```group_or``` (```or```) key. This key should contain an array of filters, these filters can be [single filters](#single-filters) or another grouped AND or OR filter if you want to create nested conditions.  
<br>
Return only the users which are active AND are last modified after 6 dec 2019:
```javascript
axios.get('jory/user', {
    params: {
        jory: {
            filter: {
                group_and: [
                    {
                        field: "is_active",
                        data: true
                    },
                    {
                        field: "modified_at",
                        operator: ">",
                        data: "2019-12-06"
                    }
                ]
            }
        }
    }
});
```

<br>
Return only the users which are active AND are last modified in 2019 AND have a last_name starting with "Clap":
```javascript
axios.get('jory/user', {
    params: {
        jory: {
            filter: {
                group_and: [
                    {
                       field: "is_active",
                    },
                    {
                        group_or: [
                            {
                                field: "modified_at",
                                operator: ">=",
                                data: "2019-01-01"
                            },
                            {
                                field: "modified_at",
                                operator: "<",
                                data: "2020-01-01"
                            }
                        ]
                    },
                    {
                        field: "last_name",
                        operator: "like",
                        data: "Clap%"
                    }
                ]
            }
        }
    }
});
```

> {info} The ```data``` key must contain an array when using the ```in``` or ```not_in``` operator.

&nbsp;
> {info} The ```data``` key may contain objects with key/value pairs when using custom filters in the Jory Resource. These objects will be passed as an associative array to the custom FilterScope.
