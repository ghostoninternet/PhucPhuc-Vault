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


