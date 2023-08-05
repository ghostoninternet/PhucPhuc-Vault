# Introduction
The `Illuminate\Support\Collection` class provides a fluent, convenient **wrapper** for working with arrays of data.
For example, check out the following code. We'll use the `collect` helper to create a new collection instance from the array, run the `strtoupper` function on each element, and then remove all empty elements:
```PHP
$collection = collect(['taylor', 'abigail', null])->map(function (string $name) {
	return strtoupper($name);
})->reject(function (string $name) {
	return empty($name);
});
```
As you can see, the `Collection` class allows you to chain its methods to perform fluent mapping and reducing of the underlying array. In general, collections are immutable, meaning every `Collection` method returns an entirely new `Collection` instance.
### Creating Collections
The `collect` helper returns a new `Illuminate\Support\Collection` instance for the given array. So, creating a collection is as simple as:
```PHP
$collection = collect([1, 2, 3]);
```
### Extending Collections
Collections are "macroable", which allows you to add additional methods to the `Collection` class at run time. The `Illuminate\Support\Collection` class' `macro` method accepts a closure that will be executed when your macro is called. The macro closure may access the collection's other methods via `$this`, just as if it were a real method of the collection class. For example, the following code adds a `toUpper` method to the `Collection` class:
```PHP
use Illuminate\Support\Collection;
use Illuminate\Support\Str;

Collection::macro('toUpper', function () {
	return $this->map(function (string $value) {
		return Str::upper($value);
	});
});

$collection = collect(['first', 'second']);
$upper = $collection->toUpper();
// ['FIRST', 'SECOND']
```
Typically, you should declare collection macros in the `boot` method of a [service provider](https://laravel.com/docs/10.x/providers).

If necessary, you may define macros that accept additional arguments:

```PHP
use Illuminate\Support\Collection;
use Illuminate\Support\Facades\Lang;

Collection::macro('toLocale', function (string $locale) {
	return $this->map(function (string $value) use ($locale) {
		return Lang::get($value, [], $locale);
	});
});

$collection = collect(['first', 'second']);

$translated = $collection->toLocale('es');
```
# Available Methods
[Click on this link to see the heaven](https://laravel.com/docs/10.x/collections#available-methods)
# Higher Order Messages
Collections also provide support for "higher order messages", which are short-cuts for performing common actions on collections. The collection methods that provide higher order messages are: [`average`](https://laravel.com/docs/10.x/collections#method-average), [`avg`](https://laravel.com/docs/10.x/collections#method-avg), [`contains`](https://laravel.com/docs/10.x/collections#method-contains), [`each`](https://laravel.com/docs/10.x/collections#method-each), [`every`](https://laravel.com/docs/10.x/collections#method-every), [`filter`](https://laravel.com/docs/10.x/collections#method-filter), [`first`](https://laravel.com/docs/10.x/collections#method-first), [`flatMap`](https://laravel.com/docs/10.x/collections#method-flatmap), [`groupBy`](https://laravel.com/docs/10.x/collections#method-groupby), [`keyBy`](https://laravel.com/docs/10.x/collections#method-keyby), [`map`](https://laravel.com/docs/10.x/collections#method-map), [`max`](https://laravel.com/docs/10.x/collections#method-max), [`min`](https://laravel.com/docs/10.x/collections#method-min), [`partition`](https://laravel.com/docs/10.x/collections#method-partition), [`reject`](https://laravel.com/docs/10.x/collections#method-reject), [`skipUntil`](https://laravel.com/docs/10.x/collections#method-skipuntil), [`skipWhile`](https://laravel.com/docs/10.x/collections#method-skipwhile), [`some`](https://laravel.com/docs/10.x/collections#method-some), [`sortBy`](https://laravel.com/docs/10.x/collections#method-sortby), [`sortByDesc`](https://laravel.com/docs/10.x/collections#method-sortbydesc), [`sum`](https://laravel.com/docs/10.x/collections#method-sum), [`takeUntil`](https://laravel.com/docs/10.x/collections#method-takeuntil), [`takeWhile`](https://laravel.com/docs/10.x/collections#method-takewhile), and [`unique`](https://laravel.com/docs/10.x/collections#method-unique).

Each higher order message can be accessed as a dynamic property on a collection instance. For instance, let's use the `each` higher order message to call a method on each object within a collection:
```PHP
use App\Models\User;

$users = User::where('votes', '>', 500)->get();

$users->each->markAsVip();
```

Likewise, we can use the `sum` higher order message to gather the total number of "votes" for a collection of users:
```PHP
$users = User::where('group', 'Development')->get();

return $users->sum->votes;
```
# Lazy Collections
### Introduction
To supplement the already powerful `Collection` class, the `LazyCollection` class leverages PHP's [generators](https://www.php.net/manual/en/language.generators.overview.php) to allow you to work with very large datasets while keeping memory usage low.

The query builder's `cursor` method returns a `LazyCollection` instance. This allows you to still only run a single query against the database but also only keep one Eloquent model loaded in memory at a time. In this example, the `filter` callback is not executed until we actually iterate over each user individually, allowing for a drastic reduction in memory usage:
```PHP
use App\Models\User;

$users = User::cursor()->filter(function (User $user) {
	return $user->id > 500;
});

foreach ($users as $user) {
	echo $user->id;
}
```
### Creating Lazy Collections
To create a lazy collection instance, you should pass a PHP generator function to the collection's `make` method:
```PHP
use Illuminate\Support\LazyCollection;

LazyCollection::make(function () {

	$handle = fopen('log.txt', 'r');
	
	while (($line = fgets($handle)) !== false) {
		yield $line;
	}
});
```
### The Enumerable Contract
[Click here to see a smaller heaven](https://laravel.com/docs/10.x/collections#the-enumerable-contract)
### Lazy Collection Methods
In addition to the methods defined in the `Enumerable` contract, the `LazyCollection` class contains the following methods:

`takeUntilTimeout()` method:
The `takeUntilTimeout` method returns a new lazy collection that will enumerate values until the specified time. After that time, the collection will then stop enumerating:

```PHP
$lazyCollection = LazyCollection::times(INF)
	->takeUntilTimeout(now()->addMinute());

$lazyCollection->each(function (int $number) {
	dump($number);
	sleep(1);
});

// 1
// 2
// ...
// 58
// 59
```

`tabEach()` method:
While the `each` method calls the given callback for each item in the collection right away, the `tapEach` method only calls the given callback as the items are being pulled out of the list one by one:
```PHP
// Nothing has been dumped so far...

$lazyCollection = LazyCollection::times(INF)->tapEach(function (int $value) {
	dump($value);
});

// Three items are dumped...
$array = $lazyCollection->take(3)->all();
// 1
// 2
// 3
```

`remember()` method:
The `remember` method returns a new lazy collection that will remember any values that have already been enumerated and will not retrieve them again on subsequent collection enumerations:

```PHP
// No query has been executed yet...
$users = User::cursor()->remember();

// The query is executed...
// The first 5 users are hydrated from the database...
$users->take(5)->all();

// First 5 users come from the collection's cache...
// The rest are hydrated from the database...
$users->take(20)->all();
```
