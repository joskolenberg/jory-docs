# Using offset & limit

---

- [Applying a Limit](#limit)
- [Applying an Offset](#offset)

<a name="limit"></a>
## Applying a Limit
Use the ```limit``` or minified ```lmt``` key to set a limit on your query.  
<br>
Get the first 10 musicians sorted by ```last_name```.
```javascript
axios.get('jory/musician', {
    params: {
        jory: {
            sorts: ['last_name'],
            limit: 10
        }
    }
});
```

<a name="offset"></a>
## Applying an Offset
Use the ```offset``` or minified ```ofs``` key to set an offset on your query.  
<br>
Get musicians 11 to 20 sorted by ```last_name```.
```javascript
axios.get('jory/musician', {
    params: {
        jory: {
            sorts: ['last_name'],
            offset: 10,
            limit: 10
        }
    }
});
```

> {warning} An offset cannot be set without a limit.
