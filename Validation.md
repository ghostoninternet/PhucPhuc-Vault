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

# Form Request Validation
## Creating Form Requests
For more complex validation scenarios, you may wish to create a "form request". Form requests are custom request classes that encapsulate their own validation and authorization logic. To create a form request class, you may use the `make:request` Artisan CLI command:
```PHP
php artisan make:request StorePostRequest
```

The generated form request class will be placed in the `app/Http/Requests` directory. If this directory does not exist, it will be created when you run the `make:request` command. Each form request generated by Laravel has two methods: `authorize` and `rules`.

The `authorize` method is responsible for determining if the currently authenticated user can perform the action represented by the request, while the `rules` method returns the validation rules that should apply to the request's data:
```PHP
/**
 * Get the validation rules that apply to the request.
 *
 * @return array<string, \Illuminate\Contracts\Validation\Rule|array|string>
 */
public function rules(): array
{
	return [
		'title' => 'required|unique:posts|max:255',
		'body' => 'required',
	];
}
```

So, how are the validation rules evaluated? All you need to do is type-hint the request on your controller method. The incoming form request is validated before the controller method is called, meaning you do not need to clutter your controller with any validation logic:
```PHP
/**
 * Store a new blog post.
 */

public function store(StorePostRequest $request): RedirectResponse
{
	// The incoming request is valid...
	
	// Retrieve the validated input data...
	$validated = $request->validated();
	
	// Retrieve a portion of the validated input data...
	$validated = $request->safe()->only(['name', 'email']);
	$validated = $request->safe()->except(['name', 'email']);
	
	// Store the blog post...
	
	return redirect('/posts');
}
```

If validation fails, a redirect response will be generated to send the user back to their previous location. The errors will also be flashed to the session so they are available for display. If the request was an XHR request, an HTTP response with a 422 status code will be returned to the user including a [JSON representation of the validation errors](https://laravel.com/docs/10.x/validation#validation-error-response-format).
### Performing Additional Validation
Sometimes you need to perform additional validation after your initial validation is complete. You can accomplish this using the form request's `after` method.

The `after` method should return an array of callables or closures which will be invoked after validation is complete. The given callables will receive an `Illuminate\Validation\Validator` instance, allowing you to raise additional error messages if necessary:
```PHP
use Illuminate\Validation\Validator;

/**
* Get the "after" validation callables for the request.
*/

public function after(): array
{
	return [
		function (Validator $validator) {
			if ($this->somethingElseIsInvalid()) {
				$validator->errors()->add(
					'field',
					'Something is wrong with this field!'
				);
			}
		}
	];
}
```
### Stopping On The First Validation Failure
By adding a `stopOnFirstFailure` property to your request class, you may inform the validator that it should stop validating all attributes once a single validation failure has occurred:
```PHP
/**
* Indicates if the validator should stop on the first rule failure.
*
* @var bool
*/

protected $stopOnFirstFailure = true;
```
### Customizing The Redirect Location
As previously discussed, a redirect response will be generated to send the user back to their previous location when form request validation fails. However, you are free to customize this behavior. To do so, define a `$redirect` property on your form request
```PHP
/**
* The URI that users should be redirected to if validation fails.
*
* @var string
*/

protected $redirect = '/dashboard';
```

Or, if you would like to redirect users to a named route, you may define a `$redirectRoute` property instead:
```PHP
/**
* The route that users should be redirected to if validation fails.
*
* @var string
*/

protected $redirectRoute = 'dashboard';
```
## Authorizing Form Requests
The form request class also contains an `authorize` method. Within this method, you may determine if the authenticated user actually has the authority to update a given resource. Most likely, you will interact with your [authorization gates and policies](https://laravel.com/docs/10.x/authorization) within this method:
```PHP
use App\Models\Comment;

/**
 * Determine if the user is authorized to make this request.
 */
public function authorize(): bool
{
	$comment = Comment::find($this->route('comment'));
	
	return $comment && $this->user()->can('update', $comment);
}
```

Since all form requests extend the base Laravel request class, we may use the `user` method to access the currently authenticated user. Also, note the call to the `route` method in the example above. This method grants you access to the URI parameters defined on the route being called, such as the `{comment}` parameter in the example below:
```PHP
Route::post('/comment/{comment}');
```
Therefore, if your application is taking advantage of [route model binding](https://laravel.com/docs/10.x/routing#route-model-binding), your code may be made even more succinct by accessing the resolved model as a property of the request:
```PHP
return $this->user()->can('update', $this->comment);
```

If the `authorize` method returns `false`, an HTTP response with a 403 status code will automatically be returned and your controller method will not execute.

If you plan to handle authorization logic for the request in another part of your application, you may simply return `true` from the `authorize` method:
```PHP
/**
* Determine if the user is authorized to make this request.
*/

public function authorize(): bool
{
	return true;
}
```
## Customizing The Error Messages
You may customize the error messages used by the form request by overriding the `messages` method. This method should return an array of attribute / rule pairs and their corresponding error messages:
```PHP
/**
* Get the error messages for the defined validation rules.
*
* @return array<string, string>
*/

public function messages(): array
{
	return [
		'title.required' => 'A title is required',
		'body.required' => 'A message is required',
	];
}
```

## Preparing Input For Validation
If you need to prepare or sanitize any data from the request before you apply your validation rules, you may use the `prepareForValidation` method:
```PHP
use Illuminate\Support\Str;

/**
* Prepare the data for validation.
*/
protected function prepareForValidation(): void
{
	$this->merge([
		'slug' => Str::slug($this->slug),
	]);
}
```

Likewise, if you need to normalize any request data after validation is complete, you may use the `passedValidation` method:
```PHP
/**
* Handle a passed validation attempt.
*/
protected function passedValidation(): void
{
	$this->replace(['name' => 'Taylor']);
}
```
# Manually Creating Validators
If you do not want to use the `validate` method on the request, you may create a validator instance manually using the `Validator` [facade](https://laravel.com/docs/10.x/facades). The `make` method on the facade generates a new validator instance:
```PHP
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use Illuminate\Http\RedirectResponse;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Validator;

class PostController extends Controller
{
/**
* Store a new blog post.
*/
	public function store(Request $request): RedirectResponse
	{
		$validator = Validator::make($request->all(), [
			'title' => 'required|unique:posts|max:255',
			'body' => 'required',
		]);
		
		if ($validator->fails()) {
			return redirect('post/create')
					->withErrors($validator)
					->withInput();
		}
		
		// Retrieve the validated input...
		$validated = $validator->validated();
		
		// Retrieve a portion of the validated input...
		
		$validated = $validator->safe()->only(['name', 'email']);
		$validated = $validator->safe()->except(['name', 'email']);
	
		// Store the blog post...
	
		return redirect('/posts');
	}
}
```
The first argument passed to the `make` method is the data under validation. The second argument is an array of the validation rules that should be applied to the data.

After determining whether the request validation failed, you may use the `withErrors` method to flash the error messages to the session. When using this method, the `$errors` variable will automatically be shared with your views after redirection, allowing you to easily display them back to the user. The `withErrors` method accepts a validator, a `MessageBag`, or a PHP `array`
### Stopping On First Validation Failure
The `stopOnFirstFailure` method will inform the validator that it should stop validating all attributes once a single validation failure has occurred:
```PHP
if ($validator->stopOnFirstFailure()->fails()) {
	// ...
}
```
## Automatic Redirection
If you would like to create a validator instance manually but still take advantage of the automatic redirection offered by the HTTP request's `validate` method, you may call the `validate` method on an existing validator instance. If validation fails, the user will automatically be redirected or, in the case of an XHR request, a [JSON response will be returned](https://laravel.com/docs/10.x/validation#validation-error-response-format):
```PHP
Validator::make($request->all(), [
	'title' => 'required|unique:posts|max:255',
	'body' => 'required',
])->validate();
```

You may use the `validateWithBag` method to store the error messages in a [named error bag](https://laravel.com/docs/10.x/validation#named-error-bags) if validation fails:
```PHP
Validator::make($request->all(), [
	'title' => 'required|unique:posts|max:255',
	'body' => 'required',
])->validateWithBag('post');
```
## Named Error Bags
If you have multiple forms on a single page, you may wish to name the `MessageBag` containing the validation errors, allowing you to retrieve the error messages for a specific form. To achieve this, pass a name as the second argument to `withErrors`:
```PHP
return redirect('register')->withErrors($validator, 'login');
```
You may then access the named `MessageBag` instance from the `$errors` variable:

```PHP
{{ $errors->login->first('email') }}
```
## Customizing The Error Messages
If needed, you may provide custom error messages that a validator instance should use instead of the default error messages provided by Laravel. There are several ways to specify custom messages. First, you may pass the custom messages as the third argument to the `Validator::make` method:
```PHP
$validator = Validator::make($input, $rules, $messages = [
	'required' => 'The :attribute field is required.',
]);
```
In this example, the `:attribute` placeholder will be replaced by the actual name of the field under validation.
### Specifying A Custom Message For A Given Attribute
```PHP
$messages = [
	'email.required' => 'We need to know your email address!',
];
```
### Specifying Custom Attribute Values
Many of Laravel's built-in error messages include an `:attribute` placeholder that is replaced with the name of the field or attribute under validation. To customize the values used to replace these placeholders for specific fields, you may pass an array of custom attributes as the fourth argument to the `Validator::make` method:
```PHP
$validator = Validator::make($input, $rules, $messages, [
	'email' => 'email address',
]);
```
## Performing Additional Validation
Sometimes you need to perform additional validation after your initial validation is complete. You can accomplish this using the validator's `after` method. The `after` method accepts a closure or an array of callables which will be invoked after validation is complete. The given callables will receive an `Illuminate\Validation\Validator` instance, allowing you to raise additional error messages if necessary:
```PHP
use Illuminate\Support\Facades\Validator;

$validator = Validator::make(/* ... */);

$validator->after(function ($validator) {
	if ($this->somethingElseIsInvalid()) {
		$validator->errors()->add(
			'field', 'Something is wrong with this field!'
		);
	}	
});

if ($validator->fails()) {
	// ...
}
```

As noted, the `after` method also accepts an array of callables, which is particularly convenient if your "after validation" logic is encapsulated in invokable classes, which will receive an `Illuminate\Validation\Validator` instance via their `__invoke` method:
```PHP
use App\Validation\ValidateShippingTime;
use App\Validation\ValidateUserStatus;

$validator->after([
	new ValidateUserStatus,
	new ValidateShippingTime,
	function ($validator) {
		// ...
	},
]);
```
# Working With Validated Input
First, you may call the `validated` method on a form request or validator instance. This method returns an array of the data that was validated:
```PHP
$validated = $request->validated();

$validated = $validator->validated();
```

Alternatively, you may call the `safe` method on a form request or validator instance. This method returns an instance of `Illuminate\Support\ValidatedInput`. This object exposes `only`, `except`, and `all` methods to retrieve a subset of the validated data or the entire array of validated data:
```PHP
$validated = $request->safe()->only(['name', 'email']);
$validated = $request->safe()->except(['name', 'email']);
$validated = $request->safe()->all();
```

In addition, the `Illuminate\Support\ValidatedInput` instance may be iterated over and accessed like an array:
```PHP
// Validated data may be iterated...
foreach ($request->safe() as $key => $value) {
	// ...
}

// Validated data may be accessed as an array...
$validated = $request->safe();

$email = $validated['email'];
```

If you would like to add additional fields to the validated data, you may call the `merge` method:
```PHP
$validated = $request->safe()->merge(['name' => 'Taylor Otwell']);
```

If you would like to retrieve the validated data as a [collection](https://laravel.com/docs/10.x/collections) instance, you may call the `collect` method:
```PHP
$collection = $request->safe()->collect();
```
# Working With Error Messages
After calling the `errors` method on a `Validator` instance, you will receive an `Illuminate\Support\MessageBag` instance, which has a variety of convenient methods for working with error messages. The `$errors` variable that is automatically made available to all views is also an instance of the `MessageBag` class.
### Retrieving The First Error Message For A Field
```PHP
$errors = $validator->errors();

echo $errors->first('email');
```
### Retrieving All Error Messages For A Field
If you need to retrieve an array of all the messages for a given field, use the `get` method:
```PHP
foreach ($errors->get('email') as $message) {
	// ...
}
```

If you are validating an array form field, you may retrieve all of the messages for each of the array elements using the `*` character:
```PHP
foreach ($errors->get('attachments.*') as $message) {
	// ...
}
```
### Retrieving All Error Messages For All Fields
```PHP
foreach ($errors->all() as $message) {
	// ...
}
```
### Determining If Messages Exist For A Field
```PHP
if ($errors->has('email')) {
	// ...
}
```
## Specifying Custom Messages In Language Files
Laravel's built-in validation rules each have an error message that is located in your application's `lang/en/validation.php` file. If your application does not have a `lang` directory, you may instruct Laravel to create it using the `lang:publish` Artisan command.

Within the `lang/en/validation.php` file, you will find a translation entry for each validation rule. You are free to change or modify these messages based on the needs of your application.

In addition, you may copy this file to another language directory to translate the messages for your application's language.
### Custom Messages For Specific Attributes
You may customize the error messages used for specified attribute and rule combinations within your application's validation language files. To do so, add your message customizations to the `custom` array of your application's `lang/xx/validation.php` language file:
```PHP
'custom' => [
	'email' => [
		'required' => 'We need to know your email address!',
		'max' => 'Your email address is too long!'
	],
],
```
## Specifying Attributes In Language Files
Many of Laravel's built-in error messages include an `:attribute` placeholder that is replaced with the name of the field or attribute under validation. If you would like the `:attribute` portion of your validation message to be replaced with a custom value, you may specify the custom attribute name in the `attributes` array of your `lang/xx/validation.php` language file:
```PHP
'attributes' => [
	'email' => 'email address',
],
```
## Specifying Values In Language Files
Some of Laravel's built-in validation rule error messages contain a `:value` placeholder that is replaced with the current value of the request attribute. However, you may occasionally need the `:value` portion of your validation message to be replaced with a custom representation of the value. For example, consider the following rule that specifies that a credit card number is required if the `payment_type` has a value of `cc`:
```PHP
Validator::make($request->all(), [
	'credit_card_number' => 'required_if:payment_type,cc'
]);
```

```PHP
'values' => [
	'payment_type' => [
		'cc' => 'credit card'
	],
],
```
After defining this value, the validation rule will produce the following error message:
```
The credit card number field is required when payment type is credit card.
```
# Conditionally Adding Rules
# Validating Arrays
## Validating Nested Array Input
## Error Message Indexes & Positions
# Validating Files
# Validating Passwords
# Custom Validation Rules
## Using Rule Objects
## Using Closures
## Implicit Rules
