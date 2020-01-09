# Laravel-Jory: Flexible Eloquent API Resources

---

- [Concept Overview](#concept-overview)
- [Supported functions](#supported-functions)

<a name="concept-overview"></a>
## Concept Overview
Laravel Jory creates a dynamic API for your Laravel application to serve the data from your Eloquent models.
JoryResources are comparable to Laravel's Resources, but you only write (or [generate](/{{route}}/{{version}}/generator)) a JoryResource once for each model. Next, your data can be queried in a flexible way by passing a [Jory Query](/{{route}}/{{version}}/query_introduction) to the [Jory Endpoints](/{{route}}/{{version}}/endpoints).

<br>
Jory is designed to be simple enough to master within minutes but flexible enough to fit 95% of your daily use-cases. It's like some lightweight Graphql around your Eloquent models. <sup>*The Jory syntax has nothing to do with Graphql</sup>

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
