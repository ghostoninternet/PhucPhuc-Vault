# Introduction
Laravel's database query builder provides a convenient, fluent interface to creating and running database queries. It can be used to perform most database operations in your application and works perfectly with all of Laravel's supported database systems.

The Laravel query builder uses PDO parameter binding to protect your application against SQL injection attacks. There is no need to clean or sanitize strings passed to the query builder as query bindings.
# Running Database Query
#### Retrieving All Rows From A Table
You may use the `table` method provided by the `DB` facade to begin a query. The `table` method returns a fluent query builder instance for the given table, allowing you to chain more constraints onto the query and then finally retrieve the results of the query using the `get` method:
```PHP
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use Illuminate\Support\Facades\DB;
use Illuminate\View\View;

class UserController extends Controller
{

	/**
	 * Show a list of all of the application's users.
	 */
	
	public function index(): View
	{
		$users = DB::table('users')->get();
		return view('user.index', ['users' => $users]);
	}
}
```
The `get` method returns an `Illuminate\Support\Collection` instance containing the results of the query where each result is an instance of the PHP `stdClass` object. You may access each column's value by accessing the column as a property of the object
#### Retrieving A Single Row / Column From A Table
If you just need to retrieve a single row from a database table, you may use the `DB` facade's `first` method. This method will return a single `stdClass` object
```PHP
$user = DB::table('users')->where('name', 'John')->first();

return $user->email;
```
If you don't need an entire row, you may extract a single value from a record using the `value` method. This method will return the value of the column directly.
```PHP
$email = DB::table('users')->where('name', 'John')->value('email');
```
To retrieve a single row by its `id` column value, use the `find` method:
```PHP
$user = DB::table('users')->find(3);
```
#### Retrieving A List Of Column Values
If you would like to retrieve an `Illuminate\Support\Collection` instance containing the values of a single column, you may use the `pluck` method. In this example, we'll retrieve a collection of user titles:
```PHP
use Illuminate\Support\Facades\DB;

$titles = DB::table('users')->pluck('title');

foreach ($titles as $title) {
	echo $title;
}
```
You may specify the column that the resulting collection should use as its keys by providing a second argument to the `pluck` method:

```PHP
$titles = DB::table('users')->pluck('title', 'name');

foreach ($titles as $name => $title) {
	echo $title;
}
```
### Chunking Results
If you need to work with thousands of database records, consider using the `chunk` method provided by the `DB` facade. This method retrieves a small chunk of results at a time and feeds each chunk into a closure for processing. 
```PHP
use Illuminate\Support\Collection;
use Illuminate\Support\Facades\DB;

DB::table('users')->orderBy('id')->chunk(100, function (Collection $users) {
	foreach ($users as $user) {
		// ...
	}
});
```
You may stop further chunks from being processed by returning `false` from the closure:
```PHP
DB::table('users')->orderBy('id')->chunk(100, function (Collection $users) {
	// Process the records...
	return false;
});
```
If you are updating database records while chunking results, your chunk results could change in unexpected ways. If you plan to update the retrieved records while chunking, it is always best to use the `chunkById` method instead. This method will automatically paginate the results based on the record's primary key.

>When updating or deleting records inside the chunk callback, any changes to the primary key or foreign keys could affect the chunk query. This could potentially result in records not being included in the chunked results.
### Streaming Results Lazily
The `lazy` method works similarly to [the `chunk` method](https://laravel.com/docs/10.x/queries#chunking-results) in the sense that it executes the query in chunks. However, instead of passing each chunk into a callback, the `lazy()` method returns a [`LazyCollection`](https://laravel.com/docs/10.x/collections#lazy-collections), which lets you interact with the results as a single stream:
```PHP
use Illuminate\Support\Facades\DB;

DB::table('users')->orderBy('id')->lazy()->each(function (object $user) {
	// ...
});
```
Once again, if you plan to update the retrieved records while iterating over them, it is best to use the `lazyById` or `lazyByIdDesc` methods instead. These methods will automatically paginate the results based on the record's primary key.
### Aggregates
The query builder also provides a variety of methods for retrieving aggregate values like `count`, `max`, `min`, `avg`, and `sum`. You may call any of these methods after constructing your query.
#### Determining If Records Exist
Instead of using the `count` method to determine if any records exist that match your query's constraints, you may use the `exists` and `doesntExist` methods:
```PHP
if (DB::table('orders')->where('finalized', 1)->exists()) {
	// ...
}

if (DB::table('orders')->where('finalized', 1)->doesntExist()) {
	// ...
}
```
# Select Statements
```PHP
use Illuminate\Support\Facades\DB;

$users = DB::table('users')
			->select('name', 'email as user_email')
			->get();
```

```PHP
$users = DB::table('users')->distinct()->get();
```

```PHP
$query = DB::table('users')->select('name');

$users = $query->addSelect('age')->get();
```
# Raw Expressions
Sometimes you may need to insert an arbitrary string into a query. To create a raw string expression, you may use the `raw` method provided by the `DB` facade:
```PHP
$users = DB::table('users')
			->select(DB::raw('count(*) as user_count, status'))
			->where('status', '<>', 1)
			->groupBy('status')
			->get();
```

>Raw statements will be injected into the query as strings, so you should be extremely careful to avoid creating SQL injection vulnerabilities.
# Joins
#### Inner Join Clause
```PHP
use Illuminate\Support\Facades\DB;

$users = DB::table('users')
		->join('contacts', 'users.id', '=', 'contacts.user_id')
		->join('orders', 'users.id', '=', 'orders.user_id')
		->select('users.*', 'contacts.phone', 'orders.price')
		->get();
```

#### Left Join / Right Join Clause
```PHP
$users = DB::table('users')
			->leftJoin('posts', 'users.id', '=', 'posts.user_id')
			->get();

$users = DB::table('users')
			->rightJoin('posts', 'users.id', '=', 'posts.user_id')
			->get();
```

#### Cross Join Clause
Cross joins generate a cartesian product between the first table and the joined table:
```PHP
$sizes = DB::table('sizes')
			->crossJoin('colors')
			->get();
```

#### Advanced Join Clause
You may also specify more advanced join clauses. To get started, pass a closure as the second argument to the `join` method. The closure will receive a `Illuminate\Database\Query\JoinClause` instance which allows you to specify constraints on the "join" clause:
```PHP
DB::table('users')
	->join('contacts', function (JoinClause $join) {
		$join->on('users.id', '=', 'contacts.user_id')->orOn(/* ... */);
	})
	->get();
```
If you would like to use a "where" clause on your joins, you may use the `where` and `orWhere` methods provided by the `JoinClause` instance. Instead of comparing two columns, these methods will compare the column against a value:
```PHP
DB::table('users')
	->join('contacts', function (JoinClause $join) {
		$join->on('users.id', '=', 'contacts.user_id')
				->where('contacts.user_id', '>', 5);
	})
	->get();
```

#### Subquery Joins
You may use the `joinSub`, `leftJoinSub`, and `rightJoinSub` methods to join a query to a subquery. Each of these methods receives three arguments: the subquery, its table alias, and a closure that defines the related columns
```PHP
$latestPosts = DB::table('posts')
				->select('user_id', DB::raw('MAX(created_at) as last_post_created_at'))
				->where('is_published', true)
				->groupBy('user_id');

$users = DB::table('users')
		->joinSub($latestPosts, 'latest_posts', function (JoinClause $join) {
			$join->on('users.id', '=', 'latest_posts.user_id');
		})->get();
```
# Unions
```PHP
use Illuminate\Support\Facades\DB;

$first = DB::table('users')
		->whereNull('first_name');

$users = DB::table('users')
		->whereNull('last_name')
		->union($first)
		->get();
```
In addition to the `union` method, the query builder provides a `unionAll` method. Queries that are combined using the `unionAll` method will not have their duplicate results removed. The `unionAll` method has the same method signature as the `union` method.
# Basic Where Clauses
### Where Clauses
### Or Where Clauses
### Where Not Clauses
### JSON Where Clauses
### Addtional Where Clauses
### Logical Grouping
# Advanced Where Clauses
### Where Exists Clauses
### Subquery Where Clauses
### Full Text Where Clauses
# Ordering, Grouping, Limit & Offset
### Ordering
### Grouping
### Limit & Offset
# Conditional Clauses
# Insert Statements
### Upsert
# Update Statements
### Updating JSON Columns
### Increment & Decrement
# Delete Statements
# Pessimistic Locking
# Debugging

