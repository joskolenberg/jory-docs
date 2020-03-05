# Metadata

---

- [Requesting Metadata](#requesting)
- [Total](#total)
- [Query Count](#query-count)
- [User](#user)
- [Time](#time)
- [Create Your Own](#custom)

<a name="requesting"></a>
## Requesting Metadata
Laravel Jory includes some metadata functions out of the box, metadata can be requested by adding an array to the ```meta``` key in the request.  
> {info} Metadata is not available when Laravel Jory is configured to return data in the root.

<a name="total"></a>
## Total
The ```total``` metadata returns the total number of records, convenient when paginating results.  
<br>
Fetch the first 10 active users and total active users
```javascript
axios.get('jory/user', {
    params: {
        jory: {
            filter: {
                field: "is_active",
                data: true,
            },
            limit: 10
        },
        meta: ['total']
    }
});
```

<a name="query-count"></a>
## Query Count
The ```query_count``` metadata returns the total number of executed queries during the request. This can be useful for detecting N+1 problems on relations and custom attributes during development.  
<br>
Fetch the total_revenue of all projects and check the number of queries.
```javascript
axios.get('jory/project', {
    params: {
        jory: {
            fields: ['total_revenue'],
        },
        meta: ['query_count']
    }
});
```

<a name="time"></a>
## Time
The ```time``` metadata returns the execution time for the request.

<a name="user"></a>
## Current User
The ```user``` metadata simply returns the current user as extra data.

<a name="custom"></a>
## Create Your Own
You can add your own metadata options by extending the ```JosKolenberg\LaravelJory\Meta\Metadata``` class and reference it in the config's ```metadata``` array.
