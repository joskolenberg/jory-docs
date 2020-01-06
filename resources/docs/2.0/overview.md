# Laravel-Jory: Flexible Laravel Resources

---

- [Concept Overview](#concept-overview)
- [Supported functions](#supported-functions)

<a name="concept-overview"></a>
## Concept Overview
Laravel Jory creates a dynamic api for your Laravel application to serve the data from your Eloquent models to your frontend.
JoryResources are comparable to Laravel's Resources, but can be queried in a flexible way by passing a [Jory query](/{{route}}/{{version}}/query_introduction) from the frontend. You only write (or [generate](/{{route}}/{{version}}/generator)) a JoryResource once for each model and determine how you want to fetch it in your frontend. This approach saves you from having to update your backend every time your frontend demands something new.  
<br>
Jory is designed to be simple enough to master and setup in minutes but flexible enough to fit 95% of your daily use-cases.

<a name="supported-functions"></a>
## Supported Functions
### Querying
- Selecting fields (database fields & custom attributes)
- Filtering (including "and" and "or" clauses and custom filters)
- Sorting (including custom sorts)
- Relations
- Offset & Limit

### Fetch types
- Fetch a single record (like Laravel's ```first()```)
- Fetch a single record by id (like Laravel's ```find()```)
- Fetch multiple records (like Laravel's ```get()```)
- Fetch multiple resources at once

### Aggregates
- Count
- Exists

### Meta
- Total records (for pagination)
- Query count
