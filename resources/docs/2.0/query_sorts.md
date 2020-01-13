# Using Sorts

---

- [Applying Sorts](#applying)

<a name="applying"></a>
## Applying Sorts
Use the ```sorts``` or minified ```srt``` key to add sorting to your Jory Query. Prefix a sort with a dash (```-```) to apply a descending sort.  
<br>
Get all musicians sorted by ```last_name``` descending first, and ```first_name``` ascending second.
```javascript
axios.get('jory/musician', {
    params: {
        jory: {
            sorts: ['-last_name', 'first_name']
        }
    }
});
```

> {info} Sort options in the request will always take precedence over the default ones defined in the Jory Resource.

When only applying a single sort it may be passed as a string without the surrounding array.  
<br>
Get all musicians sorted by ```last_name``` descending.
```javascript
axios.get('jory/musician', {
    params: {
        jory: {
            sorts: '-last_name'
        }
    }
});
```

