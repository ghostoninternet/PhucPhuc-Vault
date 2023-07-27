# Starting our first project in the browser
>1. Go to the project folder in the terminal
>2. Type in `php artisan serve`
>	`serve` command is basically a shortcut for the **php built-in web server**, which will call the main of our application, which then will be runs.
>3. By default, HTTP will listen to port 8000. We can change this by using the following command: `php artisan serve --port=<your number>` 

# First look at our application
>**app directory**: 
>	1. Store **Models directory**.
>	2. Store **Http directory**, which contains **Controllers directory**.
>**resources directory**: store **js directory**, **css directory**, and **views directory**.

# What is the .env file
>A file to store environment setup. Everything about the server that is running in the project, and many different values for different servers are setup right here.
>**NOT RECOMMEND** to use environment variables all the time. But mostly for domain names, urls, authentication, mail addresses, service account names.

# Passing data to the views
>**Compact function**: Creating an array of the variable with its value
>
>**with method**: Can only pass in one data at a time

# Views
>Two benefits come with blade are **template inheritance** and **sections**.
>**@yield()**: define a section in a layout and it is used constantly used to get content from a child page into a master page.
>
>To insert image:
>1. Use `URL(<image's path>)` inside the `src` of `<img>`. (An alternative: you can replace `URL` with `asset`, but you must store all your CSS, JS, image files inside **public** folder because this is the only place that `asset` will find.)
>2. Use symbolic link: `php artisan storage:link`
>	Using this method, you are saving all private images and documents in the storage directory which will give you full control over files and later on you can allow certain type of users to access the images or documents
>
>`@unless()`: The condition is the same as `@if (!condition)`
>`@empty()`: Check if a variable is empty 
>`@isset()`: Check if a variable has been set

# Databases and Migrations
>**Eloquent** is an **ORM**.
>**ORM** stand for **object relational mapper**. ORM allows you to write quries using the **object oriented paradigm**. 
>
>You can see migration as the version control for your database
>`php artisan make:model <model name> -m`: create model (table) and make migration
>`php artisan migrate`: make migration
>`php artisan migrate:install`: keep track of which migrations you have or haven't run
>`php artisan migrate:reset`: call **down()** method
>`php artisan migrate:refresh`: roll back every database migration you've run on this instance and then runs every migration available

# Factory Model
>**Factory Model**: A pattern for creating fake entries for our database tables 

# Query Builder
>A fluent interface for interacting with several different types of databases with a single clear API