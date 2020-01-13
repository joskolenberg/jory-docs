# Endpoints

---

- [Single Records](#first)
- [By id](#find)
- [Collections](#get)
- [Aggregates](#aggregates)
- [Multiple Resources](#multiple)

<a name="first"></a>
## Single Records
To fetch a single record use the ```first``` endpoint.  
<br>
Fetch the first musician.
```javascript
axios.get('jory/musician/first');
```
<br>
Fetch the first musician ordered by ```last_name```.
```javascript
axios.get('jory/musician/first', {
    params: {
        jory: {
            sorts: ['last_name']
        }
    }
});
```

<a name="find"></a>
## By id
To fetch a single record by it's primary key use the ```find``` endpoint by adding the id to the url.  
<br>
Fetch the musician having id 4.
```javascript
axios.get('jory/musician/4');
```
<br>
Fetch the the ```last_name``` of the musician having id 4.
```javascript
axios.get('jory/musician/4', {
    params: {
        jory: {
            fields: ['last_name']
        }
    }
});
```

<a name="get"></a>
## Collections
Use the ```get``` endpoint to fetch multiple records matching the criteria in your Jory Query.  
<br>
Fetch all musicians.
```javascript
axios.get('jory/musician');
```
<br>
Get the ```id``` and ```last_name``` of the first 10 musicians with ```first_name``` like ```John``` ordered by ```last_name```.
```javascript
axios.get('jory/musician/4', {
    params: {
        jory: {
            fields: ['id', 'last_name'],
            filter: {
                field: 'first_name',
                operator: 'like',
                data: '%John%'
            },
            sorts: ['last_name'],
            limit: 10,
        }
    }
});
```

<a name="aggregates"></a>
## Aggregates
Simple ```count``` and ```exists``` endpoints are available to do a record count or check if any record matches your criteria.  
<br>
Count the musicians having a ```first_name``` like ```John```.
```javascript
axios.get('jory/musician/count', {
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
<br>
Check if there is a musician having a ```first_name``` like ```John```.
```javascript
axios.get('jory/musician/exists', {
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

<a name="multiple"></a>
## Multiple Resources
Use the root endpoint to fetch multiple unrelated resources at once. When using this endpoint the ```jory``` parameter must hold key/value pairs, the key being the ```resource``` and the value being the Jory Query.  
<br>
Fetch all musicians and bands at once.
```javascript
axios.get('jory', {
    params: {
        jory: {
            musician: {},
            band: {},
        }
    }
});
```
The data will be returned matching the same keys.
```json
{
    "data": {
        "musician": [
            {
                "id": 1,
                "first_name": "Mick",
                "last_name": "Jagger",
                "date_of_birth": "1943-07-26",
                "band_id": 1
            },
            {
                "id": 2,
                "first_name": "John",
                "last_name": "Lennon",
                "date_of_birth": "1940-10-09",
                "band_id": 2
            },
            ...
        ],
        "band": [
            {
                "id": 1,
                "name": "Beatles",
            },
            {
                "id": 2,
                "name": "Rolling Stones",
            },
            ...
        ]
    }
}
```
<br>
Fetch the first 10 musicians and all bands ordered by name.
```javascript
axios.get('jory', {
    params: {
        jory: {
            musician: {
                limit: 10
            },
            band: {
                sorts: ['name']
            },
        }
    }
});
```
### Other functions
The ```first```, ```find```, ```count``` and ```exists``` functions are available when fetching multiple resources by using a colon.  
<br>
Fetch the ```first_name``` of musician having id 4 and the total number of bands.
```javascript
axios.get('jory', {
    params: {
        jory: {
            "musician:4": {
                fields: ['first_name']
            },
            "band:count": {},
        }
    }
});
```
### Aliases
The keys may be post-fixed with ``` as {alias}``` to return the data on a custom key, which allows to fetch a single resource multiple times.  
<br>
Fetch the ```first_name``` of musicians being born after the sixties, all musicians with a ```first_name``` like ```John``` and a total musician count.
```javascript
axios.get('jory', {
    params: {
        jory: {
            "musician as young_musicians": {
                fields: ['first_name'],
                filter: {
                    field: 'date_of_birth',
                    operator: '>=',
                    data: '1970-01-01'
                }
            },
            "musician as johns": {
                filter: {
                    field: 'first_name',
                    operator: 'like',
                    data: '%John%'
                }
            },
            "musician:count as number_of_musicians": {},
        }
    }
});
```
