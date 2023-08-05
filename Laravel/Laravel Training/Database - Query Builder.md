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
### Chunking Results

### Streaming Results Lazily
### Aggregates
# Select Statements
# Raw Expressions
# Joins
# Unions
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

