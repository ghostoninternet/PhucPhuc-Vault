# Introduction
Using Eloquent: ==Each Model== corresponds to ==A Database Table==

# Eloquent Model Conventions
### Table Names
By convention, the **"snake case"**, plural name of the class will be used as the **table name** unless another name is explicitly specified
>`Flight` --> `flights`
>`AirTrafficController` --> `air_traffic_controllers`

`table` property: specify the model's table

### Primary Keys
>Eloquent will also assume that each model's corresponding database table has a primary key column named `id`

`primaryKey` property: specify a different column that serves as your model's primary key

> In addition, Eloquent assumes that the primary key is an incrementing integer value, which means that Eloquent will automatically cast the primary key to an integer

`incrementing` property: specify which type of key you want to use: non-interger or interger and non-incrementing or incrementing

>If your model's primary key is not an integer, you should define a protected `$keyType` property on your model. This property should have a value of `string`

### "Composite" Primary Keys
"Composite" primary keys are not supported by Eloquent models

### UUID & ULID Keys
>UUIDs are universally unique alpha-numeric identifiers that are 36 characters long

To use UUID key:
1. Ensure the model has a UUID equivalent primary key column
2. Use `Illuminate\Database\Eloquent\Concerns\HasUuids` trait on the model. The `HasUuids` trait will generate **"ordered" UUIDs** for your models

`newUniqueId` method: override the UUID generation process for a given model
`uniqueIds` method: specify which columns should receive UUIDs

>ULIDs are similar to UUIDs; however, they are only 26 characters in length

To use ULID key:
1. Ensure the model has a ULID equivalent primary key column
2. Use `Illuminate\Database\Eloquent\Concerns\HasUlids` trait on the model. 

### Timestamps
>Eloquent will automatically set`created_at` and `updated_at` columns to exist on your model's corresponding database table.

`$timestamps` property: choose whether these columns to be automatically managed by Eloquent

`$dateFormat` property: determines how date attributes are stored in the database as well as their format when the model is serialized to an array or JSON

`CREATED_AT` and `UPDATED_AT` constants: customize the names of the columns used to store the timestamps.

>If you would like to perform model operations without the model having its `updated_at` timestamp modified, you may operate on the model within a closure given to the `withoutTimestamps` method

```PHP
Model::withoutTimestamps(fn () => $post->increment(['reads']));
```

### Database connections:
>By default, all Eloquent models will use the default database connection that is configured for your application

`$connection` property: specify a different connection that should be used when interacting with a particular model

### Default Attribute Values
>By default, a newly instantiated model instance will not contain any attribute values

`$attributes` property: define the default values for some of your model's attributes. Attribute values placed in the `$attributes` array should be in their raw, "storable" format as if they were just read from the database.

# Retrieving Models
`all` method: retrieve all of the records from the model's associated database table
```PHP
use App\Models\Flight;

foreach (Flight::all() as $flight) {
	echo $flight->name;
}
```

### Building Queries
>Since Eloquent models are query builders, you should review all of the methods provided by Laravel's [query builder](https://laravel.com/docs/10.x/queries). You may use any of these methods when writing your Eloquent queries.

### Refreshing Models
`fresh` method: re-retrieve the model from the database
```PHP
$flight = Flight::where('number', 'FR 900')->first();

$freshFlight = $flight->fresh();
```

`refresh` method: re-hydrate the existing model using fresh data from the database
```PHP
$flight = Flight::where('number', 'FR 900')->first();

$flight->number = 'FR 456';

$flight->refresh();

$flight->number; // "FR 900"
```

### Collection
Eloquent methods like `all` and `get` don't return a plain PHP array, instead, an instance of `Illuminate\Database\Eloquent\Collection` is returned.

### Chunking Result
`chunk` method: process large numbers of models more efficiently. The `chunk` method will retrieve a subset of Eloquent models, passing them to a closure for processing. Since only the current chunk of Eloquent models is retrieved at a time, the `chunk` method will provide significantly reduced memory usage when working with a large number of models
The first argument passed to the `chunk` method is the number of records you wish to receive per "chunk". The closure passed as the second argument will be invoked for each chunk that is retrieved from the database. A database query will be executed to retrieve each chunk of records passed to the closure.
If you are filtering the results of the `chunk` method based on a column that you will also be updating while iterating over the results, you should use the `chunkById` method. Using the `chunk` method in these scenarios could lead to unexpected and inconsistent results.
### Chunking Using Lazy Collections
`lazy` method: works similarly to the `chunk` method. However, instead of passing each chunk directly into a callback as is, the `lazy` method returns a flattened [`LazyCollection`](https://laravel.com/docs/10.x/collections#lazy-collections) of Eloquent models, which lets you interact with the results as a single stream.
If you are filtering the results of the `lazy` method based on a column that you will also be updating while iterating over the results, you should use the `lazyById` method. Internally, the `lazyById` method will always retrieve models with an `id` column greater than the last model in the previous chunk
### Cursors
`cursor` method: used to significantly reduce your application's memory consumption when iterating through tens of thousands of Eloquent model records. The `cursor` method will only execute a single database query; however, the individual Eloquent models will not be hydrated until they are actually iterated over. Therefore, only one Eloquent model is kept in memory at any given time while iterating over the cursor. Since the `cursor` method only ever holds a single Eloquent model in memory at a time, it cannot eager load relationships. The `cursor` returns an `Illuminate\Support\LazyCollection` instance.

### Advanced Subqueries
#### Subquery Selects
Eloquent also offers advanced subquery support, which allows you to pull information from related tables in a single query
#### Subquery Ordering

# Retrieving Single Models / Aggregates
`find`, `first`, or `firstWhere` methods: Instead of returning a collection of models, these methods return a single model instance.

`findOr` and `firstOr` methods: return a single model instance or, if no results are found, execute the given closure

`findOrFail` and `firstOrFail` methods: retrieve the first result of the query; however, if no result is found, an `Illuminate\Database\Eloquent\ModelNotFoundException` will be thrown

### Retrieving Or Creating Models
`firstOrCreate` method: locate a database record using the given column / value pairs. If the model can not be found in the database, a record will be inserted with the attributes resulting from merging the first array argument with the optional second array argument.

`firstOrNew` method: attempt to locate a record in the database matching the given attributes. However, if a model is not found, a new model instance will be returned. Note that the model returned by `firstOrNew` has not yet been persisted to the database. You will need to manually call the `save` method to persist it.

### Retrieving Aggregates
When interacting with Eloquent models, you may also use the `count`, `sum`, `max`, and other [aggregate methods](https://laravel.com/docs/10.x/queries#aggregates) provided by the Laravel [query builder](https://laravel.com/docs/10.x/queries)

# Inserting and Updating Models
### Insert
To insert a new record into the database, you should instantiate a new model instance and set attributes on the model. Then, call the `save` method on the model instance

Alternatively, you may use the `create` method to "save" a new model using a single PHP statement. The inserted model instance will be returned to you by the `create` method. However, before using the `create` method, you will need to specify either a `fillable` or `guarded` property on your model class.
### Updates
To update a model, you should retrieve it and set any attributes you wish to update. Then, you should call the model's `save` method. Again, the `updated_at`
timestamp will automatically be updated, so there is no need to manually set its value
#### Mass updates
```PHP
Flight::where('active', 1)

->where('destination', 'San Diego')

->update(['delayed' => 1]);
```
The `update` method expects an array of column and value pairs representing the columns that should be updated. The `update` method returns the number of affected rows.
#### Examining Attribute Changes
`isDirty`, `isClean`, and `wasChanged` methods: examine the internal state of your model and determine how its attributes have changed from when the model was originally retrieved

>`isDirty` method: determines if any of the model's attributes have been changed since the model was retrieved. ou may pass a specific attribute name or an array of attributes to the `isDirty` method to determine if any of the attributes are "dirty". 
>
>`isClean` method: determine if an attribute has remained unchanged since the model was retrieved. This method also accepts an optional attribute argument
>
>`wasChanged` method: determines if any attributes were changed when the model was last saved within the current request cycle. If needed, you may pass an attribute name to see if a particular attribute was changed.

`getOriginal` method: returns an array containing the original attributes of the model regardless of any changes to the model since it was retrieved.  If needed, you may pass a specific attribute name to get the original value of a particular attribute.

