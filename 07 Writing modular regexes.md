# Writing modular regexes

It's possible to write modular regexes by using subroutines. First things first:
```php
$regex1 = '/(\d+),\1/';     // Backreference
$regex2 = '/(\d+),(?1)/';   // Subroutine
```

In the first regex we used a backreference `\1`, it will match what was matched in group 1. So the first regex will match `123,123` but won't match `123,156`.

In the second regex we used a subroutine (or recursion) `(?1)`, it will repeat the pattern that was used in group 1. You might say it's the same as writing `/(\d+),\d+/`. It will match `123,123` and `123,156`.

We can also use named groups instead:
```php
$regex = '/(?<number>\d+),(?&number)/';   // Named subroutine
```

Let's use this to our advantage
```php

$regex = <<<'regex'
~
(?(DEFINE)
   (?<id>\d+)
   (?<username>user(?&id))
   (?<protocol>https?|ftp)
   (?<domain>example[.]com)
   (?<url>(?&protocol)://(?&domain)/users/(?&id)/(?&username)/?)
)

^(?&url)$   # Anchor our match
~x
regex;
```

We first have a bunch of definitions and then we use one of those definitions in our pattern. For some reason you must change the regex so that an id must be alphanumeric instead of digits, support other protocols or change the domain. You name it. You would only need to change that specific definition instead of rewriting a complete regex.

If you want to read about the DEFINE construct. Check out this [StackOverflow answer](https://stackoverflow.com/a/18151617)



### [<< previous](06%20Escaping%20a%20backslash%20hell.md)