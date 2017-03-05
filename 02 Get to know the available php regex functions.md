# Get to know the available php regex functions

I've seen a lot of cases where people don't seem to be aware of functions other than the classical `preg_match()` and `preg_replace()`. Below we'll cover some use cases:

##[`preg_split()`](http://php.net/manual/en/function.preg-split.php)

Sometimes you want splitting instead of matching. Instead of writing the following code:
```php
$input = 'preg__split_for___fun';

if(preg_match_all('/[^_]+/', $input, $m)) {
    print_r($m[0]);
} else {
    echo 'no match';
}
```
Try to write it like this:
```php
$input = 'preg__split_for___fun';
$output = preg_split('/_+/', $input);
print_r($output);
```

##[`preg_grep()`](http://php.net/manual/en/function.preg-grep.php)

Say you want to loop through an array and try to match values against a specific regex. I've seen people trying:

```php
$input = ['data1', 'data2', 'exclude', 'data3'];
$result = [];
foreach ($input as $v) {
    if(preg_match('/data\d+/', $v)) {
        $result[] = $v;
    }
}
print_r($result); // Array ( [0] => data1 [1] => data2 [2] => data3 )
```
While you could achieve the same effect using `preg_grep()`:
```php
$input = ['data1', 'data2', 'exclude', 'data3'];
$result = preg_grep('/data\d+/', $input);
print_r($result); // Array ( [0] => data1 [1] => data2 [3] => data3 ) 
```
The only thing you need to take into account is that it returns an array indexed using the keys from the input array.



##[`preg_filter()`](http://php.net/manual/en/function.preg-filter.php)

Per the documentation:

> preg_filter() is identical to preg_replace() except it only returns the (possibly transformed) subjects where there was a match.

In essence, this is the same as `preg_grep()` but with a replace option.


##[`preg_replace_callback()`](http://php.net/manual/en/function.preg-replace-callback.php)

Sometimes you don't want a simple replace. This function let's you use a function as callback. Say we want to match some words and convert them to upper case:

```php
$input = 'words are important';
$output = preg_replace_callback('/\w+/', function($m) {
    return strtoupper($m[0]);
}, $input);
echo $output;
```
We'll cover more `preg_replace_callback()` tricks in another chapter.



##[`preg_quote()`](http://php.net/manual/en/function.preg-quote.php)

Sometimes we want to include dynamic user input into our regex. For example: search for a sequence chosen by the user followed by digits. The code would look roughly like this:

```php
$user_input = isset($_GET['input']) ? (string) $_GET['input'] : '';
$haystack = 'List: pid1000, pid2000, pid3000...';
$regex = '/' . $user_input . '\d+/';

if(preg_match_all($regex, $haystack, $m)) {
    print_r($m[0]);
} else {
    echo 'no match';
}
```

But what if the user entered some (incorrect) regex sequence like `pid{}+++`?. We obviously want to prevent that:
```php
$regex = '/' . preg_quote($user_input, '/') . '\d+/'; 
```


##[`preg_last_error()`](http://php.net/manual/en/function.preg-last-error.php)

This function might come handy for debugging purposes. It will return the error code of the last PCRE regex execution. Someone has written a [nice function](http://php.net/manual/en/function.preg-last-error.php#112449) to convert the error code to actual text.

Since we're talking about errors and debugging, you might as well want to check the return value of the used function. The meaning might vary. For example `preg_match()`, `preg_match_all()`, `preg_split()` returns `false` on error while `preg_replace()` returns `NULL` on error. Always consult the documentation.


### [<< previous](01 Avoid using regex whenever possible.md) | [next >>](03 A note on security.md)
