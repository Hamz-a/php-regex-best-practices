# Escaping a backslash hell

Backslash hell is when you need to escape a lot of characters which results into cluttering your regex with a lot of backslashes. The following tips will help you escape such a mess.

## Choosing the right delimiter

The forward slash `/` is commonly used as a delimiter in the regex world. Sometimes it might be better to use different delimiters. Especially when you have some forward slashes in your regex:

```php
// Backslash
$regex = '/^\/user\/(\d+)\/?/i';

// Clean
$regex = '~^/user/(\d+)/?~i';
```

Other common delimiters are `#`, `!`, `%`. Keep in mind to use one that's less likely to be included in your regex.

One not so well known but interesting way is to use `()` as delimiters:

```php
$regex = '(^/user/(\d+)/?)i';
```

Notice that you don't need to escape the brackets inside the regex. You could see the first braces as "group 0" and the second (inner braces) as group 1.


## Choosing the right quotes

Sometimes you need to match double quotes, use single quotes for defining the regex string and vice versa:

```php
// Backslash
$regex = "\"([^\"]+)\"";
$regex = '\'([^\']+)\'';

// Clean
$regex = '"([^"]+)"';
$regex = "'([^']+)'";
```

When writing big regexes, you might want to chunk it down into pieces using the x modifier:

```php
$regex = <<<'regex'
~
    fancy       # fancy's explanation
    regex       # note ^ no escape needed
~x
regex;

preg_match_all($regex, $input,$m);
```

We used the nowdoc string syntax but we could also have used a heredoc. [Read the difference from the manual](http://php.net/manual/en/language.types.string.php#language.types.string.syntax.nowdoc).
