# Quickstart

---

- [Creating the Jory API](#api)
- [Fetching data](#fetching)

<a name="api"></a>
## Creating the Jory API
The quickest (but not always recommended) way to get started is to turn all your Eloquent models into a Jory API with a single command. 
```shell script
php artisan jory:generate --all
```
This command generates a JoryResource in the ```\App\Http\JoryResources``` namespace for each of your models.

> {warning} Without further actions your Jory Resources are publicly available. Be sure to add the [auth middleware](/{{route}}/{{version}}/security#authentication) and/or [scope](/{{route}}/{{version}}/security#scoping) your data before putting your code into production!

<a name="fetching"></a>
## Fetching data
Let's assume we've got the following Eloquent models.
```text
Musician
    - id
    - first_name
    - last_name
    - date_of_birth
    - band_id

Band
    - id
    - name
    - year_start

Album
    - id
    - release_date
    - band_id
```
```Musician``` has a ```band``` relation and ```Band``` has an ```albums``` relation defined.  
### Fetch all musicians
```javascript
axios.get('jory/musician');
```
Result:
```json
{
    "data": [
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
        {
            "id": 3,
            "first_name": "Paul",
            "last_name": "McCartney",
            "date_of_birth": "1942-06-18",
            "band_id": 2
        },
        {
            "id": 4,
            "first_name": "Robbert",
            "last_name": "Plant",
            "date_of_birth": "1948-08-20",
            "band_id": 3
        }
    ]
}
```
### Fetch a musician by id
```javascript
axios.get('jory/musician/2');
```
Result:
```json
{
    "data": {
        "id": 2,
        "first_name": "John",
        "last_name": "Lennon",
        "date_of_birth": "1940-10-09",
        "band_id": 2
    }
}
```
### Fetch a musician's first name by id
```javascript
axios.get('jory/musician/2', {
    params: {
        jory: {
            fields: ["first_name"]
        }
    }
});
```
Result:
```json
{
    "data": {
        "first_name": "John"
    }
}
```
### Fetch all the musician's first names filtered by band
```javascript
axios.get('jory/musician', {
    params: {
        jory: {
            fields: ["first_name"],
            filter: {
                field: "band_id",  
                operator: "=", // "=" is the default operator. Could be omitted in this case but included for clarity.          
                data: 2
            }
        }
    }
});
```
Result:
```json
{
    "data": [
        {
            "first_name": "John"
        },
        {
            "first_name": "Paul"
        }
    ]
}
```
### Fetch all the musician's first and last names sorted by first name
```javascript
axios.get('jory/musician', {
    params: {
        jory: {
            fields: ["first_name", "last_name"],
            sorts: ["first_name"],
        }
    }
});
```
Result:
```json
{
    "data": [
        {
            "first_name": "John",
            "last_name": "Lennon"
        },
        {
            "first_name": "Mick",
            "last_name": "Jagger"
        },
        {
            "first_name": "Paul",
            "last_name": "McCartney"
        },
        {
            "first_name": "Robbert",
            "last_name": "Plant"
        }
    ]
}
```
### Fetch the first two musician's first names
```javascript
axios.get('jory/musician', {
    params: {
        jory: {
            fields: ["first_name"],
            limit: 2
        }
    }
});
```
Result:
```json
{
    "data": [
        {
            "first_name": "Mick"
        },
        {
            "first_name": "John"
        }
    ]
}
```
### Fetch a musician's first name and related band.
```javascript
axios.get('jory/musician/2', {
    params: {
        jory: {
            fields: ["first_name"],
            relations: {
                band: {}
            }
        }
    }
});
```
Result:
```json
{
    "data": {
        "first_name": "John",
        "band": {
            "id": 2,
            "name": "Beatles",
            "year_start": 1960
        }
    }
}
```
### Fetch a musician's first name and related band's name.
```javascript
axios.get('jory/musician/2', {
    params: {
        jory: {
            fields: ["first_name"],
            relations: {
                band: {
                    fields: ["name"],
                }
            }
        }
    }
});
```
Result:
```json
{
    "data": {
        "first_name": "John",
        "band": {
            "name": "Beatles",
        }
    }
}
```
### Putting it all together
- Filter musicians having a last name containing an "a"
- Fetch the first two musicians sorted by last name descending
- Return only musician's first and last name
- Include the related band's name
- Include the band's albums
- Return only the album's names and release dates
- Sort the related albums by release date ascending

```javascript
axios.get('jory/musician', {
    params: {
        jory: {
            filter: {
                field: "last_name",
                operator: "like",
                data: "%a%"
            },
            limit: 2,
            sorts: ["-last_name"],
            fields: ["first_name", "last_name"],
            relations: {
                band: {
                    fields: ["name"],
                    relations: {
                        albums: {
                            fields: ["name", "release_date"],
                            sorts: ["release_date"],
                        }
                    }
                }
            }
        }
    }
});
```
Result:
```json
{
    "data": [
        {
            "first_name": "Robbert",
            "last_name": "Plant",
            "band": {
                "name": "Led Zeppelin",
                "albums": [
                    {
                        "name": "Led Zeppelin I",
                        "release_date": "1969-01-12"
                    },
                    {
                        "name": "Led Zeppelin II",
                        "release_date": "1969-10-22"
                    },
                    {
                        "name": "Led Zeppelin III",
                        "release_date": "1970-10-05"
                    }
                ]
            }
        },
        {
            "first_name": "Paul",
            "last_name": "McCartney",
             "band": {
                 "name": "Beatles",
                 "albums": [
                     {
                         "name": "Sgt. Peppers lonely hearts club band",
                         "release_date": "1967-06-01"
                     },
                     {
                         "name": "Abbey road",
                         "release_date": "1969-09-26"
                     },
                     {
                         "name": "Let it be",
                         "release_date": "1970-05-08"
                     }
                 ]
             }
       }
    ]
}
```
