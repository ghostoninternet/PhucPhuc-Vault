# Introduction
Migrations are like version control for your database, allowing your team to define and share the application's database schema definition. If you have ever had to tell a teammate to manually add a column to their local database schema after pulling in your changes from source control, you've faced the problem that database migrations solve.

The Laravel `Schema` [facade](https://laravel.com/docs/10.x/facades) provides database agnostic support for creating and manipulating tables across all of Laravel's supported database systems. Typically, migrations will use this facade to create and modify database tables and columns.
# Generating Migrations
You may use the `make:migration` [Artisan command](https://laravel.com/docs/10.x/artisan) to generate a database migration. The new migration will be placed in your `database/migrations` directory. Each migration filename contains a timestamp that allows Laravel to determine the order of the migrations:
```PHP
php artisan make:migration create_flights_table
```
Laravel will use the name of the migration to attempt to guess the name of the table and whether or not the migration will be creating a new table. If Laravel is able to determine the table name from the migration name, Laravel will pre-fill the generated migration file with the specified table. Otherwise, you may simply specify the table in the migration file manually.

If you would like to specify a custom path for the generated migration, you may use the `--path` option when executing the `make:migration` command. The given path should be relative to your application's base path.
## Squashing Migrations
```PHP
php artisan schema:dump

# Dump the current database schema and prune all existing migrations...

php artisan schema:dump --prune
```
When you execute this command, Laravel will write a "schema" file to your application's `database/schema` directory. The schema file's name will correspond to the database connection. Now, when you attempt to migrate your database and no other migrations have been executed, Laravel will first execute the SQL statements in the schema file of the database connection you are using. After executing the schema file's SQL statements, Laravel will execute any remaining migrations that were not part of the schema dump.

If your application's tests use a different database connection than the one you typically use during local development, you should ensure you have dumped a schema file using that database connection so that your tests are able to build your database. You may wish to do this after dumping the database connection you typically use during local development:
```PHP
php artisan schema:dump

php artisan schema:dump --database=testing --prune
```
You should commit your database schema file to source control so that other new developers on your team may quickly create your application's initial database structure.

>Migration squashing is only available for the MySQL, PostgreSQL, and SQLite databases and utilizes the database's command-line client. Schema dumps may not be restored to in-memory SQLite databases.
# Migration Structure
A migration class contains two methods: `up` and `down`. The `up` method is used to add new tables, columns, or indexes to your database, while the `down` method should reverse the operations performed by the `up` method.
```PHP
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    /**
     * Run the migrations.
     */

    public function up(): void
    {
        Schema::create('categories', function (Blueprint $table) {
            $table->increments('category_id');
            $table->string('category_name', 255);
        });
  
        Schema::create('brands', function (Blueprint $table) {
            $table->increments('brand_id');
            $table->string('brand_name', 255);
        });
    }

    /**
     * Reverse the migrations.
     */

    public function down(): void
    {
        Schema::dropIfExists('bike_store');
    }
};
```
### Setting The Migration Connection
If your migration will be interacting with a database connection other than your application's default database connection, you should set the `$connection` property of your migration
```PHP
/**
* The database connection that should be used by the migration.
*
* @var string
*/
protected $connection = 'pgsql';
/**
* Run the migrations.
*/

public function up(): void
{
	// ...
}
```
# Running Migrations
To run all of your outstanding migrations, execute the `migrate` Artisan command
```PHP
php artisan migrate
```
If you would like to see which migrations have run thus far, you may use the `migrate:status` Artisan command

If you would like to see the SQL statements that will be executed by the migrations without actually running them, you may provide the `--pretend` flag to the `migrate` command
### Isolating Migration Execution
If you are deploying your application across multiple servers and running migrations as part of your deployment process, you likely do not want two servers attempting to migrate the database at the same time. To avoid this, you may use the `isolated` option when invoking the `migrate` command.

When the `isolated` option is provided, Laravel will acquire an atomic lock using your application's cache driver before attempting to run your migrations. All other attempts to run the `migrate` command while that lock is held will not execute; however, the command will still exit with a successful exit status code:
```PHP
php artisan migrate --isolated
```

>To utilize this feature, your application must be using the `memcached`, `redis`, `dynamodb`, `database`, `file`, or `array` cache driver as your application's default cache driver. In addition, all servers must be communicating with the same central cache server.
### Forcing Migrations To Run In Production
Some migration operations are destructive, which means they may cause you to lose data. In order to protect you from running these commands against your production database, you will be prompted for confirmation before the commands are executed. To force the commands to run without a prompt, use the `--force` flag:
```PHP
php artisan migrate --force
```
## Rolling back migrations
To roll back the latest migration operation, you may use the `rollback` Artisan command. This command rolls back the last "batch" of migrations, which may include multiple migration files
```PHP
php artisan migrate:rollback
```

You may roll back a limited number of migrations by providing the `step` option to the `rollback` command
```PHP
php artisan migrate:rollback --step=5
```

The `migrate:reset` command will roll back all of your application's migrations
```PHP
php artisan migrate:reset
```

You may roll back a specific "batch" of migrations by providing the `batch` option to the `rollback` command, where the `batch` option corresponds to a batch value within your application's `migrations` database table.
```PHP
php artisan migrate:rollback --batch=3
```

If you would like to see the SQL statements that will be executed by the migrations without actually running them, you may provide the `--pretend` flag to the `migrate:rollback` command
```PHP
php artisan migrate:rollback --pretend
```
### Rollback & Migrate Using A Single Command
The `migrate:refresh` command will roll back all of your migrations and then execute the `migrate` command. This command effectively re-creates your entire database

You may roll back and re-migrate a limited number of migrations by providing the `step` option to the `refresh` command
### Drop All Tables & Migrate
The `migrate:fresh` command will drop all tables from the database and then execute the `migrate` command
>The `migrate:fresh` command will drop all database tables regardless of their prefix. This command should be used with caution when developing on a database that is shared with other applications.
# Tables
## Creating Tables
To create a new database table, use the `create` method on the `Schema` facade. The `create` method accepts two arguments: the first is the name of the table, while the second is a closure which receives a `Blueprint` object that may be used to define the new table:
```PHP
Schema::create('categories', function (Blueprint $table) {
       $table->increments('category_id');
       $table->string('category_name', 255);
 });
```
When creating the table, you may use any of the schema builder's [column methods](https://laravel.com/docs/10.x/migrations#creating-columns) to define the table's columns.
### Checking For Table / Column Existence
You may check for the existence of a table or column using the `hasTable` and `hasColumn` methods:
```PHP
if (Schema::hasTable('users')) {
	// The "users" table exists...
}

if (Schema::hasColumn('users', 'email')) {
	// The "users" table exists and has an "email" column...
}
```

### Database Connection & Table Options
If you want to perform a schema operation on a database connection that is not your application's default connection, use the `connection` method
```PHP
Schema::connection('sqlite')->create('users', function (Blueprint $table) {
	$table->id();
});
```

The `engine` property may be used to specify the table's storage engine when using **MySQL**
```PHP
Schema::create('users', function (Blueprint $table) {
	$table->engine = 'InnoDB';
	// ...
});
```

The `charset` and `collation` properties may be used to specify the character set and collation for the created table when using **MySQL**
```PHP
Schema::create('users', function (Blueprint $table) {
	$table->charset = 'utf8mb4';
	$table->collation = 'utf8mb4_unicode_ci';
	// ...
});
```

The `temporary` method may be used to indicate that the table should be "temporary". Temporary tables are only visible to the current connection's database session and are dropped automatically when the connection is closed:
```PHP
Schema::create('calculations', function (Blueprint $table) {
	$table->temporary();
	// ...
});
```

If you would like to add a "comment" to a database table, you may invoke the `comment` method on the table instance. Table comments are currently only supported by **MySQL and Postgres**:
```PHP
Schema::create('calculations', function (Blueprint $table) {
	$table->comment('Business calculations');
	// ...
});
```
## Updating Tables
The `table` method on the `Schema` facade may be used to update existing tables. Like the `create` method, the `table` method accepts two arguments: the name of the table and a closure that receives a `Blueprint` instance you may use to add columns or indexes to the table
```PHP
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

Schema::table('users', function (Blueprint $table) {
	$table->integer('votes');
});
```
## Renaming / Dropping Tables
To rename an existing database table, use the `rename` method:
```PHP
use Illuminate\Support\Facades\Schema;

Schema::rename($from, $to);
```
To drop an existing table, you may use the `drop` or `dropIfExists` methods:
```PHP
Schema::drop('users');

Schema::dropIfExists('users');
```

# Columns
## Creating Columns
The `table` method on the `Schema` facade may be used to update existing tables. Like the `create` method, the `table` method accepts two arguments: the name of the table and a closure that receives an `Illuminate\Database\Schema\Blueprint` instance you may use to add columns to the table:
```PHP
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

Schema::table('users', function (Blueprint $table) {
	$table->integer('votes');
});
```
## Available Column Types
The schema builder blueprint offers a variety of methods that correspond to the different types of columns you can add to your database tables. Each of the available methods are listed in the table below:
[Click here to see all column types](https://laravel.com/docs/10.x/migrations#available-column-types)
## Column Modifiers
In addition to the column types listed above, there are several column "modifiers" you may use when adding a column to a database table.
```PHP
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

Schema::table('users', function (Blueprint $table) {
	$table->string('email')->nullable();
});
```

![[Pasted image 20230809112131.png]]
### Default Expression
The `default` modifier accepts a value or an `Illuminate\Database\Query\Expression` instance. Using an `Expression` instance will prevent Laravel from wrapping the value in quotes and allow you to use database specific functions.
### Column Order
When using the **MySQL** database, the `after` method may be used to add columns after an existing column in the schema:
```PHP
$table->after('password', function (Blueprint $table) {
	$table->string('address_line1');
	$table->string('address_line2');
	$table->string('city');
});
```
## Modifying Columns
The `change` method allows you to modify the type and attributes of existing columns. For example, you may wish to increase the size of a `string` column. To see the `change` method in action, let's increase the size of the `name` column from 25 to 50. To accomplish this, we simply define the new state of the column and then call the `change` method:
```PHP
Schema::table('users', function (Blueprint $table) {
	$table->string('name', 50)->change();
});
```

When modifying a column, you must explicitly include all of the modifiers you want to keep on the column definition - any missing attribute will be dropped. For example, to retain the `unsigned`, `default`, and `comment` attributes, you must call each modifier explicitly when changing the column:
```PHP
Schema::table('users', function (Blueprint $table) {
	$table->integer('votes')->unsigned()->default(1)->comment('my comment')->change();
});
```
## Renaming Columns
To rename a column, you may use the `renameColumn` method provided by the schema builder:
```PHP
Schema::table('users', function (Blueprint $table) {
	$table->renameColumn('from', 'to');
});
```
## Dropping Columns
To drop a column, you may use the `dropColumn` method on the schema builder:
```PHP
Schema::table('users', function (Blueprint $table) {
	$table->dropColumn('votes');
});
```

```PHP
Schema::table('users', function (Blueprint $table) {
	$table->dropColumn(['votes', 'avatar', 'location']);
});
```

### Available Command Aliases
Laravel provides several convenient methods related to dropping common types of columns. Each of these methods is described in the table below:
![[Pasted image 20230809150308.png]]
# Indexes
## Creating Indexes
The Laravel schema builder supports several types of indexes. The following example creates a new `email` column and specifies that its values should be unique. To create the index, we can chain the `unique` method onto the column definition:
```PHP
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

Schema::table('users', function (Blueprint $table) {
	$table->string('email')->unique();
});
```

Alternatively, you may create the index after defining the column. To do so, you should call the `unique` method on the schema builder blueprint. This method accepts the name of the column that should receive a unique index:
```PHP
$table->unique('email');
```

```PHP
$table->index(['account_id', 'created_at']);
```

When creating an index, Laravel will automatically generate an index name based on the table, column names, and the index type, but you may pass a second argument to the method to specify the index name yourself:
```PHP
$table->unique('email', 'unique_email');
```

### Available Index Types
Laravel's schema builder blueprint class provides methods for creating each type of index supported by Laravel. Each index method accepts an optional second argument to specify the name of the index. If omitted, the name will be derived from the names of the table and column(s) used for the index, as well as the index type.
![[Pasted image 20230809150756.png]]
## Renaming Indexes
To rename an index, you may use the `renameIndex` method provided by the schema builder blueprint. This method accepts the current index name as its first argument and the desired name as its second argument:
```PHP
$table->renameIndex('from', 'to')
```
## Dropping Indexes
To drop an index, you must specify the index's name. By default, Laravel automatically assigns an index name based on the table name, the name of the indexed column, and the index type. Here are some examples:
![[Pasted image 20230809151054.png]]
```PHP
Schema::table('geo', function (Blueprint $table) {

$table->dropIndex(['state']); // Drops index 'geo_state_index'

});
```
## Foreign Key Constraints
Laravel also provides support for creating foreign key constraints, which are used to force referential integrity at the database level. For example, let's define a `user_id` column on the `posts` table that references the `id` column on a `users` table:
```PHP
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

Schema::table('posts', function (Blueprint $table) {
	$table->unsignedBigInteger('user_id');
	$table->foreign('user_id')->references('id')->on('users');
});
```

When using the `foreignId` method to create your column, the example above can be rewritten like so:
```PHP
Schema::table('posts', function (Blueprint $table) {
	$table->foreignId('user_id')->constrained();
});
```

The `foreignId` method creates an `UNSIGNED BIGINT` equivalent column, while the `constrained` method will use conventions to determine the table and column being referenced. If your table name does not match Laravel's conventions, you may manually provide it to the `constrained` method. In addition, the name that should be assigned to the generated index may be specified as well:
```PHP
Schema::table('posts', function (Blueprint $table) {
	$table->foreignId('user_id')->constrained(
		table: 'users', indexName: 'posts_user_id'
	);
});
```

You may also specify the desired action for the "on delete" and "on update" properties of the constraint:
```PHP
$table->foreignId('user_id')
	  ->constrained()
	  ->onUpdate('cascade')
	  ->onDelete('cascade');
```
![[Pasted image 20230809151735.png]]
Any additional [column modifiers](https://laravel.com/docs/10.x/migrations#column-modifiers) must be called before the `constrained` method:

```PHP
$table->foreignId('user_id')
	  ->nullable()
	  ->constrained();
```
### Dropping Foreign Keys
To drop a foreign key, you may use the `dropForeign` method, passing the name of the foreign key constraint to be deleted as an argument. Foreign key constraints use the same naming convention as indexes.
```PHP
$table->dropForeign('posts_user_id_foreign');
```

Alternatively, you may pass an array containing the column name that holds the foreign key to the `dropForeign` method. The array will be converted to a foreign key constraint name using Laravel's constraint naming conventions:
```PHP
$table->dropForeign(['user_id']);
```
### Toggling Foreign Key Constraints
You may enable or disable foreign key constraints within your migrations by using the following methods:
```PHP
Schema::enableForeignKeyConstraints();

Schema::disableForeignKeyConstraints();

Schema::withoutForeignKeyConstraints(function () {

	// Constraints disabled within this closure...
});
```


