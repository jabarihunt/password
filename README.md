# Password Class

This is a simple class that uses the standard PHP methods _[password_hash()]_ and _[password_verify()]_ to create a hashed password and compare the hash to entered passwords respectively.  The primary purpose of this class is to remove some of the boilerplate code required when performing these password operations.

The class defaults to using the default algorithm that PHP selects (per version) and a cost of 10.  Both can be set when creating the hash.

> _**NOTE:**_ It is recommended that database columns that store hashes created with `password_hash()` are at least 255 characters wide, as the length of hashes will grow in future versions of PHP!

## INSTALLING

#### Via Composer

Run the following command in the same directory as your composer.json file:

`php composer.phar require jabarihunt/password`

#### Via Github

1. Clone this repository into a working directory: `git clone git@github.com:jabarihunt/password .`

2. Include the Password class in your project...

```php
require('/path/to/Password.php')
```
...or if using an auto-loader...
```php
 use jabarihunt/Password;
```

## USAGE

The `Password::create()` and `Password::compare()` methods will throw exceptions if invalid data is passed to them, so be sure to use try/catch blocks!

#### Creating Password Hashes

```php
<?php

    $password = 'FooBar1@';
    
    try {
        $hash1 = Password::create($password);                       // basic usage
        $hash2 = Password::create($password, PASSWORD_BCRYPT);      // set algorithm
        $hash3 = Password::create($password, PASSWORD_BCRYPT, 12);  // set algorithm & cost
    } catch (\Exception $e) {
        var_dump($e);
    }
    
    echo "hash1: {$hash1} <br/> hash2: {$hash2} <br/> hash3: $hash3}";

?>
```

```
/* OUTPUT*/
hash1: $2y$10$lqfDbrxDEwnw34uaJCBN4OLatL3XKWnxuIwBTHqhcY5NVvvljlnd6 
hash2: $2y$10$2b2nHGE1Jx58AyHxVwWiq.EC039DNB9HLzcY.3b7tpEdIvLg6j30q 
hash3: $2y$12$e284Os/7zD4MsxXFX9h5UuKj3disIkmOkIJRzj4CnMoT3np7tyD2y
```
#### Comparing Passwords And Hashes

Using our first hash from above...

```php
<?php

    $password = 'FooBar1@';
    $hash     = '$2y$10$lqfDbrxDEwnw34uaJCBN4OLatL3XKWnxuIwBTHqhcY5NVvvljlnd6';
    
    try {
        $passwordIsValid = Password::compare($password, $hash);  // returns boolean
        var_dump($passwordIsValid);
    }
    catch (\Exception $e) {
        var_dump($e);
    }

?>
```

```
/* OUTPUT*/
/var/www/html/controllers/HomeController.php:7:boolean true
```

#### Validating Passwords

There is a third method, `Password::isValid()`, that validates if a password follows the below rules and returns a `boolean`.  Eventually I'll add functionallity to pass your own rules.  This method doesn't throw any exceptiones.

**Password Rules:**
- Is not empty
- Contains at least 8 charatcters
- Contains at least 1 uppercase letter
- Contains at least 1 lowercase letter
- Contains at least 1 number
- Contains at least 1 symbol

You can optionally pass a second `string` parameter called `$username` that will make sure the password does not contain the username (or whatever the passed string contains).

```php
<?php

    $username = 'Foo';

    $password1 = 'FooBar';
    $password2 = 'FooBar1@';

    var_dump(Password::isValid($password1));
    var_dump(Password::isValid($password2));
    var_dump(Password::isValid($password2, $username));

?>
```

```
/* OUTPUT*/
/var/www/html/controllers/HomeController.php:8:boolean false
/var/www/html/controllers/HomeController.php:9:boolean true
/var/www/html/controllers/HomeController.php:10:boolean false
```

## CONTRIBUTING

1. Fork Repository
2. Create a descriptive branch name
3. Make edits to your branch
4. Squash (rebase) your commits
5. Create a pull request

## LICENSE

This project is licensed under the MIT License - see the [LICENSE.md] file for details

[password_hash()]: <http://php.net/manual/en/function.password-hash.php>
[password_verify()]: <http://php.net/manual/en/function.password-verify.php>
[LICENSE.md]: <LICENSE.md>