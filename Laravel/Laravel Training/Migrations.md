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
When using the MySQL database, the `after` method may be used to add columns after an existing column in the schema:
```PHP
$table->after('password', function (Blueprint $table) {
	$table->string('address_line1');
	$table->string('address_line2');
	$table->string('city');
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

