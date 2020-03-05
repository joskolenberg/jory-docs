# Fetching Relations

---

- [Fetching Relations](#fetching)
- [Fetch types](#fetch-types)
- [Aliases](#aliases)

<a name="fetching"></a>
## Fetching Relations
Use the ```relations``` (```rlt```) key to define which relations you want to fetch. This parameter holds an object with key/value pairs, the key being the relation name and the value being another Jory Query. This Jory Query will be applied on the loaded relation.
> {info} The Jory Query on a relation can in turn contain relations, this allows you to fetch unlimited nested relations.
  
<br>
Load all musicians first names including their related band.
```javascript
axios.get('jory/musician', {
    params: {
        jory: {
            fields: ['first_name'],
            relations: {
                band: {}
            }
        }
    }
});
```
<br>
Load all musicians first names including the related band and the title of songs written before 1980 ordered by release date descending.
```javascript
axios.get('jory/musician', {
    params: {
        jory: {
            fields: ['first_name'],
            relations: {
                band: {},
                songs_written: {
                    filter: {
                        field: 'release_date',
                        operator: '<',
                        data: '2018-01-01'
                    },
                    sorts: ['-release_date'],
                    fields: ['title']
                }
            }
        }
    }
});
```
<br>
When you want to load multiple levels of relations you can also use dot-notation.
```javascript
axios.get('jory/musician', {
    params: {
        jory: {
            relations: {
                "band.songs": {
                    fields: ["title"]
                },
                "band.albums": {
                    fields: ["name"]
                }
            }
        }
    }
});
```
equals
```javascript
axios.get('jory/musician', {
    params: {
        jory: {
            relations: {
                band: {
                    relations: {
                        songs: {
                            fields: ["title"]
                        },
                        albums: {
                            fields: ["name"]
                        }
                    }
                }
            }
        }
    }
});
```

<a name="fetch-types"></a>
## Fetch Types
The ```first```, ```count``` and ```exists``` functions are available when fetching a relation by using a colon.  
<br>
Load all musicians including the total of related songs.
```javascript
axios.get('jory/musician', {
    params: {
        jory: {
            relations: {
                "songs:count": {},
            }
        }
    }
});
```

> {warning} The ```first```, ```count``` and ```exists``` cannot be eager loaded. Using these on large collections can lead to N+1 performance issues.

<a name="aliases"></a>
## Aliases
The relation keys may be post-fixed with ``` as {alias}``` to return the data on a custom key, which allows to fetch a single relation multiple times.  
<br>
Load all musicians including the total of related songs and the number of songs containing "love".
```javascript
axios.get('jory/musician', {
    params: {
        jory: {
            fields: ['first_name'],
            relations: {
                "songs:count as song_count": {},
                "songs:count as lovesong_count": {
                    field: "title",
                    operator: "like",
                    data: "%love%",
                },
            }
        }
    }
});
```
