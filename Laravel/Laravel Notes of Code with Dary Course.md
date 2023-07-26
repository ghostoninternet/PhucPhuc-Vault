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

# Composer JSON and lock file
>