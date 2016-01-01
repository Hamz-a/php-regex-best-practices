# A note on security

There's almost always an aspect of security when writing code. The same goes when using regex. Let's list a few issues:

##The "e" modifier

the "e" stands for ~~evil~~ eval.  When using using it with `preg_replace()` it will perform a regex substitution and evaluate it as PHP code:

```php
$input = 'up this case!';

$output = preg_replace('/\w+/e', 'strtoupper($0)', $input);
echo $output; // UP THIS CASE!
```
This might escalate quickly:
```php
$input = '{phpinfo()}';

$output = preg_replace('/\{(.*?)\}/e', 'strtoupper($1)', $input);
// phpinfo() gets executed
```
No wonder it got *deprecated* as of PHP 5.5 and *removed* from PHP 7.

The solution would be to use [`preg_replace_callback()`](http://php.net/manual/en/function.preg-replace-callback.php) instead:

```php
$input = '{phpinfo()}';

$output = preg_replace_callback('/\{(.*?)\}/', function($m) {
    return strtoupper($m[1]);
}, $input);

echo $output; // PHPINFO()
```


##Dangerous wild input

Sometimes we need to include user input into our regex:
```php
$regex = '/' . $input . '(?=.*?look)/';
```
but what if a malicious user tried to perform a [ReDOS](https://en.wikipedia.org/wiki/ReDoS) by injecting a special crafted regex sequence?
Fixing this is easy using [`preg_quote()`](http://php.net/manual/en/function.preg-quote.php) to escape those regex sequences!

```php
$regex = '/' . preg_quote($input, '/') . '(?=.*?look)/';
```
Don't forget to specify the second parameter which stands for the delimiter that should be escaped as well.


### [<< previous](02 Get to know the available php regex functions.md) | [next >>](04 Comments.md)