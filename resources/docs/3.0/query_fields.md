# Selecting Fields

---

- [Selecting Fields](#selecting)
- [Default Fields](#default)

<a name="selecting"></a>
## Selecting Fields
Use the ```fields``` or minified ```fld``` key to define which fields you want to be returned.  
<br>
Get the first and last name from all musicians.
```javascript
axios.get('jory/musician', {
    params: {
        jory: {
            fields: ['first_name', 'last_name']
        }
    }
});
```

> {info} When the ```fields``` and ```fld``` are omitted the [default](/{{route}}/{{version}}/fields#default) fields set in the Jory Resource will be returned.

When only selecting a single field it may be passed as a string without the surrounding array.  
<br>
Get only the first name from all musicians.
```javascript
axios.get('jory/musician', {
    params: {
        jory: {
            fields: 'first_name',
        }
    }
});
```

