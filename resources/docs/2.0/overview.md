# Laravel-Jory: Flexible Eloquent API Resources

---

- [Concept Overview](#concept-overview)
- [Supported functions](#supported-functions)

<a name="concept-overview"></a>
## Concept Overview
Laravel Jory creates a dynamic API for your Laravel application to serve the data from your Eloquent models.
JoryResources are comparable to Laravel's built-in Resource classes but you only write (or [generate](/{{route}}/{{version}}/generator)) a JoryResource once for each model. Next, your data can be queried in a flexible way by passing a [Jory Query](/{{route}}/{{version}}/query_introduction) to the [Jory Endpoints](/{{route}}/{{version}}/endpoints).

<br>
Jory is designed to be simple enough to master within minutes but flexible enough to fit 95% of your data-fetching use-cases. It brings Eloquent Query Builder's most-used features directly to your frontend.

> {info} All fetching examples in this documentation assume the use of Axios but you can use whatever tool you like.

<a name="supported-functions"></a>
## Supported Functions
### Querying
- [Selecting fields](/{{route}}/{{version}}/query_fields) (database fields & custom attributes)
- [Filtering](/{{route}}/{{version}}/query_filters) (including nested ```and``` and ```or``` clauses and custom filters)
- [Sorting](/{{route}}/{{version}}/query_sorts) (including custom sorts)
- [Relations](/{{route}}/{{version}}/query_relations)
- [Offset & Limit](/{{route}}/{{version}}/query_offset_and_limit)

### Endpoints
- Fetch a [single record](/{{route}}/{{version}}/endpoints#first) (like Laravel's ```first()```)
- Fetch a [single record by id](/{{route}}/{{version}}/endpoints#find) (like Laravel's ```find()```)
- Fetch [multiple records](/{{route}}/{{version}}/endpoints#get) (like Laravel's ```get()```)
- Fetch [multiple resources at once](/{{route}}/{{version}}/endpoints#multiple)

### Aggregates
- [Count](/{{route}}/{{version}}/endpoints#aggregates)
- [Exists](/{{route}}/{{version}}/endpoints#aggregates)

### Metadata
- [Total records](/{{route}}/{{version}}/metadata#total) (for pagination)
- [Query count](/{{route}}/{{version}}/metadata#query-count)
