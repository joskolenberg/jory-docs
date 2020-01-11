# Laravel-Jory: Flexible Eloquent API Resources

---

- [Concept Overview](#concept-overview)
- [Supported functions](#supported-functions)

<a name="concept-overview"></a>
## Concept Overview
Laravel Jory creates a dynamic API for your Laravel application to serve the data from your Eloquent models.
JoryResources are comparable to Laravel's built-in Resource classes but you only write (or [generate](/{{route}}/{{version}}/generator)) a JoryResource once for each model. Next, your data can be queried in a flexible way by passing a [Jory Query](/{{route}}/{{version}}/query_introduction) to the [Jory Endpoints](/{{route}}/{{version}}/endpoints).

<br>
Jory is designed to be simple enough to master within minutes but flexible enough to fit 95% of your daily use-cases. It's like some lightweight Graphql around your Eloquent models. <sup>*The Jory syntax has nothing to do with Graphql</sup>

> {info} All fetching examples in this documentation assume the use of Axios but you can use whatever tool you like.

<a name="supported-functions"></a>
## Supported Functions
### Querying
- [Selecting fields](/{{route}}/{{version}}/query_fields) (database fields & custom attributes)
- [Filtering](/{{route}}/{{version}}/query_filters) (including nested ```and``` and ```or``` clauses and custom filters)
- [Sorting](/{{route}}/{{version}}/query_sorts) (including custom sorts)
- [Relations](/{{route}}/{{version}}/query_relations)
- [Offset & Limit](/{{route}}/{{version}}/query_offset)

### Endpoints
- Fetch a [single record](/{{route}}/{{version}}/endpoint_first) (like Laravel's ```first()```)
- Fetch a [single record by id](/{{route}}/{{version}}/endpoint_find) (like Laravel's ```find()```)
- Fetch [multiple records](/{{route}}/{{version}}/endpoint_get) (like Laravel's ```get()```)
- Fetch [multiple resources at once](/{{route}}/{{version}}/endpoint_multiple)

### Aggregates
- [Count](/{{route}}/{{version}}/endpoint_aggregates#count)
- [Exists](/{{route}}/{{version}}/endpoint_aggregates#exists)

### Metadata
- [Total records](/{{route}}/{{version}}/metadata#total) (for pagination)
- [Query count](/{{route}}/{{version}}/metadata#query-count)
