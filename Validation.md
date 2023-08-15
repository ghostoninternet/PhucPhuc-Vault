# Introduction
Laravel provides several different approaches to validate your application's incoming data. It is most common to use the `validate` method available on all incoming HTTP requests.

Laravel includes a wide variety of convenient validation rules that you may apply to data, even providing the ability to validate if values are unique in a given database table.
# Validation Quickstart
## Defining The Routes
```PHP
use App\Http\Controllers\PostController;

Route::get('/post/create', [PostController::class, 'create']);
Route::post('/post', [PostController::class, 'store']);
```
## Creating The Controller
```PHP
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use Illuminate\Http\RedirectResponse;
use Illuminate\Http\Request;
use Illuminate\View\View;

class PostController extends Controller
{
	/**
	 * Show the form to create a new blog post.
	 */

	public function create(): View
	{
		return view('post.create');
	}
	
	/**
	 * Store a new blog post.
	 */
	
	public function store(Request $request): RedirectResponse
	{
		// Validate and store the blog post...
		
		$post = /** ... */
		
		return to_route('post.show', ['post' => $post->id]);
	}
}
```
## Writing The Validation Logic
The `validate` method provided by the `Illuminate\Http\Request` object. If the validation rules pass, your code will keep executing normally; however, if validation fails, an `Illuminate\Validation\ValidationException` exception will be thrown and the proper error response will automatically be sent back to the user.

If validation fails during a traditional HTTP request, a redirect response to the previous URL will be generated. If the incoming request is an XHR request, a [JSON response containing the validation error messages](https://laravel.com/docs/10.x/validation#validation-error-response-format) will be returned.

```PHP
/**
 * Store a new blog post.
 */

public function store(Request $request): RedirectResponse
{
	$validated = $request->validate([
		'title' => 'required|unique:posts|max:255',
		'body' => 'required',
	]);
	
	// The blog post is valid...
	
	return redirect('/posts');
}
```

Alternatively, validation rules may be specified as arrays of rules instead of a single `|` delimited string:
```PHP
$validatedData = $request->validate([
	'title' => ['required', 'unique:posts', 'max:255'],
	'body' => ['required'],
]);
```

In addition, you may use the `validateWithBag` method to validate a request and store any error messages within a [named error bag](https://laravel.com/docs/10.x/validation#named-error-bags):
```PHP
$validatedData = $request->validateWithBag('post', [
	'title' => ['required', 'unique:posts', 'max:255'],
	'body' => ['required'],
]);
```
### Stopping On First Validation Failure
Sometimes you may wish to stop running validation rules on an attribute after the first validation failure. To do so, assign the `bail` rule to the attribute:
```PHP
$request->validate([
	'title' => 'bail|required|unique:posts|max:255',
	'body' => 'required',
]);
```
In this example, if the `unique` rule on the `title` attribute fails, the `max` rule will not be checked. Rules will be validated in the order they are assigned.
### A Note On Nested Attributes
If the incoming HTTP request contains "nested" field data, you may specify these fields in your validation rules using "dot" syntax:
```PHP
$request->validate([
	'title' => 'required|unique:posts|max:255',
	'author.name' => 'required',
	'author.description' => 'required',
]);
```
On the other hand, if your field name contains a literal period, you can explicitly prevent this from being interpreted as "dot" syntax by escaping the period with a backslash:
```PHP
$request->validate([
	'title' => 'required|unique:posts|max:255',
	'v1\.0' => 'required',
]);
```
## Displaying The Validation Errors
So, what if the incoming request fields do not pass the given validation rules? As mentioned previously, Laravel will automatically redirect the user back to their previous location. In addition, all of the validation errors and [request input](https://laravel.com/docs/10.x/requests#retrieving-old-input) will automatically be [flashed to the session](https://laravel.com/docs/10.x/session#flash-data).

An `$errors` variable is shared with all of your application's views by the `Illuminate\View\Middleware\ShareErrorsFromSession` middleware, which is provided by the `web` middleware group. When this middleware is applied an `$errors` variable will always be available in your views, allowing you to conveniently assume the `$errors` variable is always defined and can be safely used. The `$errors` variable will be an instance of `Illuminate\Support\MessageBag`.
```PHP
<!-- /resources/views/post/create.blade.php -->

<h1>Create Post</h1>

@if ($errors->any())
	<div class="alert alert-danger">
		<ul>
		@foreach ($errors->all() as $error)
			<li>{{ $error }}</li>
		@endforeach
		</ul>
	</div>
@endif

<!-- Create Post Form -->
```
### Customizing The Error Messages
Laravel's built-in validation rules each have an error message that is located in your application's `lang/en/validation.php` file. If your application does not have a `lang` directory, you may instruct Laravel to create it using the `lang:publish` Artisan command.

Within the `lang/en/validation.php` file, you will find a translation entry for each validation rule. You are free to change or modify these messages based on the needs of your application.

In addition, you may copy this file to another language directory to translate the messages for your application's language.
### XHR Requests & Validation
In this example, we used a traditional form to send data to the application. However, many applications receive XHR requests from a JavaScript powered frontend. When using the `validate` method during an XHR request, Laravel will not generate a redirect response. Instead, Laravel generates a [JSON response containing all of the validation errors](https://laravel.com/docs/10.x/validation#validation-error-response-format). This JSON response will be sent with a 422 HTTP status code.
### The `@error` Directive
You may use the `@error` [Blade](https://laravel.com/docs/10.x/blade) directive to quickly determine if validation error messages exist for a given attribute. Within an `@error` directive, you may echo the `$message` variable to display the error message:
```PHP
<!-- /resources/views/post/create.blade.php -->

<label for="title">Post Title</label>

<input id="title"
	type="text"
	name="title"
	class="@error('title') is-invalid @enderror">

@error('title')
	<div class="alert alert-danger">{{ $message }}</div>
@enderror
```
If you are using [named error bags](https://laravel.com/docs/10.x/validation#named-error-bags), you may pass the name of the error bag as the second argument to the `@error` directive:
```PHP
<input ... class="@error('title', 'post') is-invalid @enderror">
```
## Repopulating Forms
When Laravel generates a redirect response due to a validation error, the framework will automatically [flash all of the request's input to the session](https://laravel.com/docs/10.x/session#flash-data). This is done so that you may conveniently access the input during the next request and repopulate the form that the user attempted to submit.

To retrieve flashed input from the previous request, invoke the `old` method on an instance of `Illuminate\Http\Request`. The `old` method will pull the previously flashed input data from the [session](https://laravel.com/docs/10.x/session):
```PHP
$title = $request->old('title');
```

Laravel also provides a global `old` helper. If you are displaying old input within a [Blade template](https://laravel.com/docs/10.x/blade), it is more convenient to use the `old` helper to repopulate the form. If no old input exists for the given field, `null` will be returned:
```PHP
<input type="text" name="title" value="{{ old('title') }}">
```
## A Note On Optional Fields
By default, Laravel includes the `TrimStrings` and `ConvertEmptyStringsToNull` middleware in your application's global middleware stack. These middleware are listed in the stack by the `App\Http\Kernel` class. Because of this, you will often need to mark your "optional" request fields as `nullable` if you do not want the validator to consider `null` values as invalid. For example:
```PHP
$request->validate([
	'title' => 'required|unique:posts|max:255',
	'body' => 'required',
	'publish_at' => 'nullable|date',
]);
```
## Validation Error Response Format
When your application throws a `Illuminate\Validation\ValidationException` exception and the incoming HTTP request is expecting a JSON response, Laravel will automatically format the error messages for you and return a `422 Unprocessable Entity` HTTP response.
```PHP
{
	"message": "The team name must be a string. (and 4 more errors)",
	"errors": {
		"team_name": [
			"The team name must be a string.",
			"The team name must be at least 1 characters."
		],
	
		"authorization.role": [
			"The selected authorization.role is invalid."
		],
	
		"users.0.email": [
			"The users.0.email field is required."
		],
	
		"users.2.email": [
		
			"The users.2.email must be a valid email address."
		
		]
	}
}
```
