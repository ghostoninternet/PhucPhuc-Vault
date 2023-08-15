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
### Mass Assignment
A mass assignment vulnerability occurs when a user passes an unexpected HTTP request field and that field changes a column in your database that you did not expect. For example, a malicious user might send an `is_admin` parameter through an HTTP request, which is then passed to your model's `create` method, allowing the user to escalate themselves to an administrator.
#### Mass Assignment & JSON Columns
When assigning JSON columns, each column's mass assignable key must be specified in your model's `$fillable` array
#### Allowing Mass Assignment
If you would like to make all of your attributes mass assignable, you may define your model's `$guarded` property as an empty array
#### Mass Assignment Exceptions
If you wish, you may instruct Laravel to throw an exception when attempting to fill an unfillable attribute by invoking the `preventSilentlyDiscardingAttributes` method. Typically, this method should be invoked within the `boot` method of one of your application's service providers.
### Upsert
Occasionally, you may need to update an existing model or create a new model if no matching model exists. Like the `firstOrCreate` method, the `updateOrCreate` method persists the model, so there's no need to manually call the `save` method.

```PHP
$flight = Flight::updateOrCreate(

['departure' => 'Oakland', 'destination' => 'San Diego'],

['price' => 99, 'discounted' => 1]

);
```

If you would like to perform multiple "upserts" in a single query, then you should use the `upsert` method instead. The method's first argument consists of the values to insert or update, while the second argument lists the column(s) that uniquely identify records within the associated table. The method's third and final argument is an array of the columns that should be updated if a matching record already exists in the database. The `upsert` method will automatically set the `created_at` and `updated_at` timestamps if timestamps are enabled on the model.

```PHP
Flight::upsert([

['departure' => 'Oakland', 'destination' => 'San Diego', 'price' => 99],

['departure' => 'Chicago', 'destination' => 'New York', 'price' => 150]

], ['departure', 'destination'], ['price']);
```

>All databases except SQL Server require the columns in the second argument of the `upsert` method to have a "primary" or "unique" index. In addition, the MySQL database driver ignores the second argument of the `upsert` method and always uses the "primary" and "unique" indexes of the table to detect existing records.
# Deleting Models
To delete a model, you may call the `delete` method on the model instance.

You may call the `truncate` method to delete all of the model's associated database records. The `truncate` operation will also reset any auto-incrementing IDs on the model's associated table
#### Deleting An Existing Model By Its Primary Key
if you know the primary key of the model, you may delete the model without explicitly retrieving it by calling the `destroy` method. In addition to accepting the single primary key, the `destroy` method will accept multiple primary keys, an array of primary keys, or a [collection](https://laravel.com/docs/10.x/collections) of primary keys

>The `destroy` method loads each model individually and calls the `delete` method so that the `deleting` and `deleted` events are properly dispatched for each model

#### Deleting Models Using Queries
In this example, we will delete all flights that are marked as inactive. Like mass updates, mass deletes will not dispatch model events for the models that are deleted:
```PHP
$deleted = Flight::where('active', 0)->delete();
```
>When executing a mass delete statement via Eloquent, the `deleting` and `deleted` model events will not be dispatched for the deleted models. This is because the models are never actually retrieved when executing the delete statement.
### Soft Deleting
When models are soft deleted, they are not actually removed from your database. Instead, a `deleted_at` attribute is set on the model indicating the date and time at which the model was "deleted". To enable soft deletes for a model, add the `Illuminate\Database\Eloquent\SoftDeletes` trait to the model
```PHP
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

use Illuminate\Database\Eloquent\SoftDeletes;

class Flight extends Model

{

use SoftDeletes;

}
```

You should also add the `deleted_at` column to your database table. The Laravel [schema builder](https://laravel.com/docs/10.x/migrations) contains a helper method to create this column:
```PHP
use Illuminate\Database\Schema\Blueprint;

use Illuminate\Support\Facades\Schema;

Schema::table('flights', function (Blueprint $table) {

	$table->softDeletes();

});

Schema::table('flights', function (Blueprint $table) {

	$table->dropSoftDeletes();

});
```

To determine if a given model instance has been soft deleted, you may use the `trashed` method:
```PHP
if ($flight->trashed()) {
	// ...

}
```

#### Restore Soft Deleted Models
To restore a soft deleted model, you may call the `restore` method on a model instance.

You may also use the `restore` method in a query to restore multiple models. Again, like other "mass" operations, this will not dispatch any model events for the models that are restored:
```PHP
Flight::withTrashed()
		->where('airline_id', 1)
		->restore();
```

#### Permanently Deleting Models
ou may use the `forceDelete` method to permanently remove a soft deleted model from the database table:
```PHP
$flight->forceDelete();
```
### Querying Soft Deleted Models
You may force soft deleted models to be included in a query's results by calling the `withTrashed` method on the query
```PHP
use App\Models\Flight;

$flights = Flight::withTrashed()
					->where('account_id', 1)
					->get();
```
The `onlyTrashed` method will retrieve **only** soft deleted models:
```PHP
$flights = Flight::onlyTrashed()
					->where('airline_id', 1)
					->get();
```
# Pruning Models
Sometimes you may want to **periodically delete models that are no longer needed**. To accomplish this, you may add the `Illuminate\Database\Eloquent\Prunable` or `Illuminate\Database\Eloquent\MassPrunable` trait to the models you would like to periodically prune. After adding one of the traits to the model, implement a `prunable` method which returns an Eloquent query builder that resolves the models that are no longer needed.

`pruning` method: This method will be called before the model is deleted. This method can be useful for deleting any additional resources associated with the model, such as stored files, before the model is permanently removed from the database

After configuring your prunable model, you should schedule the `model:prune` Artisan command in your application's `App\Console\Kernel` class. You are free to choose the appropriate interval at which this command should be run:
```PHP
/**
 * Define the application's command schedule.
 */
protected function schedule(Schedule $schedule): void
{
	$schedule->command('model:prune')->daily();
}
```

If your models are in a different location, you may use the `--model` option to specify the model class names:
```PHP
$schedule->command('model:prune', [
	'--model' => [Address::class, Flight::class],
])->daily();
```

If you wish to exclude certain models from being pruned while pruning all other detected models, you may use the `--except` option:
```PHP
$schedule->command('model:prune', [
	'--except' => [Address::class, Flight::class],
])->daily();
```
#### Mass Pruning
When models are marked with the `Illuminate\Database\Eloquent\MassPrunable` trait, models are deleted from the database using mass-deletion queries. Therefore, the `pruning` method will not be invoked, nor will the `deleting` and `deleted` model events be dispatched. This is because the models are never actually retrieved before deletion, thus making the pruning process much more efficient.
# Replicating Models
You may create an unsaved copy of an existing model instance using the `replicate` method. This method is particularly useful when you have model instances that share many of the same attributes.
```PHP
use App\Models\Address;

$shipping = Address::create([
	'type' => 'shipping',
	'line_1' => '123 Example Street',
	'city' => 'Victorville',
	'state' => 'CA',
	'postcode' => '90001',
]);

$billing = $shipping->replicate()->fill([
	'type' => 'billing'
]);

$billing->save();
```

To exclude one or more attributes from being replicated to the new model, you may pass an array to the `replicate` method:
```PHP
$flight = Flight::create([
	'destination' => 'LAX',
	'origin' => 'LHR',
	'last_flown' => '2020-03-04 11:00:00',
	'last_pilot_id' => 747,
]);

$flight = $flight->replicate([
	'last_flown',
	'last_pilot_id'
]);
```
# Query Scopes
### Global Scopes
Global scopes allow you to add constraints to all queries for a given model. Writing your own global scopes can provide a convenient, easy way to make sure every query for a given model receives certain constraints.
#### Generating Scopes
To generate a new global scope, you may invoke the `make:scope` Artisan command, which will place the generated scope in your application's `app/Models/Scopes` directory:
```PHP
php artisan make:scope AncientScope
```
#### Writing Global Scopes
Writing a global scope is simple. First, use the `make:scope` command to generate a class that implements the `Illuminate\Database\Eloquent\Scope` interface. The `Scope` interface requires you to implement one method: `apply`. The `apply` method may add `where` constraints or other types of clauses to the query as needed:
```PHP
<?php
namespace App\Models\Scopes;

use Illuminate\Database\Eloquent\Builder;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Scope;

class AncientScope implements Scope
{
/**
 * Apply the scope to a given Eloquent query builder.
 */
	public function apply(Builder $builder, Model $model): void
	{
		$builder->where('created_at', '<', now()->subYears(2000));
	}
}
```
#### Applying Global Scopes
To assign a global scope to a model, you should override the model's `booted` method and invoke the model's `addGlobalScope` method. The `addGlobalScope` method accepts an instance of your scope as its only argument.
```PHP
<?php

namespace App\Models;

use App\Models\Scopes\AncientScope;
use Illuminate\Database\Eloquent\Model;

class User extends Model
{
	/**
	 * The "booted" method of the model.
	 */
	protected static function booted(): void
	{
		static::addGlobalScope(new AncientScope);
	}
}
```
#### Anonymous Global Scope
When defining a global scope using a closure, you should provide a scope name of your own choosing as the first argument to the `addGlobalScope` method:
```PHP
<?php
namespace App\Models;

use Illuminate\Database\Eloquent\Builder;
use Illuminate\Database\Eloquent\Model;

class User extends Model
{
	/**
	 * The "booted" method of the model.
	 */

	protected static function booted(): void
	{
		static::addGlobalScope('ancient', function (Builder $builder) {
			$builder->where('created_at', '<', now()->subYears(2000));
		});
	}
}
```
#### Removing Global Scopes
If you would like to remove a global scope for a given query, you may use the `withoutGlobalScope` method. This method accepts the class name of the global scope as its only argument

Or, if you defined the global scope using a closure, you should pass the string name that you assigned to the global scope

If you would like to remove several or even all of the query's global scopes, you may use the `withoutGlobalScopes` method:
```PHP
// Remove all of the global scopes...
User::withoutGlobalScopes()->get();

// Remove some of the global scopes...
User::withoutGlobalScopes([
	FirstScope::class, SecondScope::class
])->get();
```
### Local Scopes
Local scopes allow you to define common sets of query constraints that you may easily re-use throughout your application. To define a scope, prefix an Eloquent model method with `scope`
Scopes should always return the same query builder instance or `void`:

```PHP
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Builder;
use Illuminate\Database\Eloquent\Model;

class User extends Model
{
	/**
	 * Scope a query to only include popular users.
	 */

	public function scopePopular(Builder $query): void
	{
		$query->where('votes', '>', 100);
	}
	
	/**
 	 * Scope a query to only include active users.
	 */
	
	public function scopeActive(Builder $query): void
	{
		$query->where('active', 1);
	}
}
```

#### Utilizing A Local Scope
Once the scope has been defined, you may call the scope methods when querying the model. However, you should not include the `scope` prefix when calling the method. You can even chain calls to various scopes:
```PHP
use App\Models\User;

$users = User::popular()->active()->orderBy('created_at')->get();
```
#### Dynamic Scope
Sometimes you may wish to define a scope that accepts parameters. To get started, just add your additional parameters to your scope method's signature. Scope parameters should be defined after the `$query` parameter:
```PHP
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Builder;
use Illuminate\Database\Eloquent\Model;

class User extends Model
{
	/**
	 * Scope a query to only include users of a given type.
	 */
	public function scopeOfType(Builder $query, string $type): void
	{
		$query->where('type', $type);
	}
}
```

```PHP
$users = User::ofType('admin')->get();
```
# Comparing Models
Sometimes you may need to determine if two models are the "same" or not. The `is` and `isNot` methods may be used to quickly verify two models have the same primary key, table, and database connection or not:
```PHP
if ($post->is($anotherPost)) {
	// ...
}

if ($post->isNot($anotherPost)) {
	// ...
}
```
The `is` and `isNot` methods are also available when using the `belongsTo`, `hasOne`, `morphTo`, and `morphOne` [relationships](https://laravel.com/docs/10.x/eloquent-relationships). This method is particularly helpful when you would like to compare a related model without issuing a query to retrieve that model:
```PHP
if ($post->author()->is($user)) {
	// ...
}
```
# Events
### Using Closures
### Observers
### Mutating Events

