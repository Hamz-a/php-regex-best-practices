# Pattern modifiers

PHP regex has a lot of modifiers. It even has modifiers that are non-existant in PCRE! Therefore I would highly recommend to read about them if you aren't familiar with them. [See the documentation page](http://php.net/manual/en/reference.pcre.pattern.modifiers.php)

A general advice would be to use modifiers only when you're convinced it should be there. I have seen a lot of people adding random modifiers without knowing exactly what it does. It works but when it needs an update they wonder why it's so complicated.

## `i` modifier

If this modifier is set, it will make the pattern case insensitive. So when you have a regex like: `/[a-zA-Z0-9]+/`, you might simplify it to `/[a-z0-9]+/i`. It will be a personal choice. You might want to avoid the `i` modifier to be as explicit as possible. Remember that this might lengthen your pattern.

## `s` modifier

Also known as **DOTALL** modifier/mode. If this modifier is set, it will make dots `.` match everything including newlines. Example:

`/a.*b/` will match:
```
a test b
```
but won't match:
```
a test
test b
```
while `/a.*b/s` matches both inputs.
Note that a dot in a character class `[.]` loses its meaning: it will match a literal dot.

## `m` modifier

Also known as **MULTILINE** modifier/mode. Quoting from the documentation:

> By default, PCRE treats the subject string as consisting of a single "line" of characters (even if it actually contains several newlines). The "start of line" metacharacter `^` matches only at the start of the string, while the "end of line" metacharacter `$` matches only at the end of the string, or before a terminating newline (unless `D` modifier is set). This is the same as Perl. When this modifier is set, the "start of line" and "end of line" constructs match immediately following or immediately before any newline in the subject string, respectively, as well as at the very start and end. This is equivalent to Perl's `/m` modifier. If there are no `\n` characters in a subject string, or no occurrences of `^` or `$` in a pattern, setting this modifier has no effect.

Let's see what this means in practice. Let's say we want to match one or more digits `[0-9]+` at the *beginning* of each line. The regex would look like this `/^[0-9]+/m`. Without the `m` modifier, it would have only matched the digits in the first line

```
123     # Matched by /^[0-9]+/ and /^[0-9]+/m
1234    # Matched by /^[0-9]+/m
12345   # Matched by /^[0-9]+/m
```

## `D` modifier

Quoting from the documentation:

> If this modifier is set, a dollar metacharacter `$` in the pattern matches only at the end of the subject string. Without this modifier, a dollar also matches immediately before the final character if it is a newline (but not before any other newlines). This modifier is ignored if m modifier is set. There is no equivalent to this modifier in Perl.

Personally, I haven't seen this modifier being used that much in the wild. Here's an example:

```php
$input_1 = 'end';
$input_2 = "end\n";   // Note newline
$input_3 = "end\r\n"; // Note newline (windows style)

$regex_1 = '/end$/';
$regex_2 = '/end$/D';

var_dump(preg_match($regex_1, $input_1)); // returns int 1
var_dump(preg_match($regex_1, $input_2)); // returns int 1
var_dump(preg_match($regex_1, $input_3)); // returns int 0

var_dump(preg_match($regex_2, $input_1)); // returns int 1
var_dump(preg_match($regex_2, $input_2)); // returns int 0
var_dump(preg_match($regex_2, $input_3)); // returns int 0
```
In essence, the `D` modifier might be useful to prevent a positive match for a string with an appended newline `\n` at the end.

## `U` modifier

Upper case letter `u`. Quoting from the documentation:

> This modifier inverts the "greediness" of the quantifiers so that they are not greedy by default, but become greedy if followed by `?`. It is not compatible with Perl. It can also be set by a `(?U)` modifier setting within the pattern or by a question mark behind a quantifier (e.g. `.*?`).

I would highly recommend **not using** this modifier. It only creates chaos and confusion. Do you want non-greedy/lazy/reluctant behaviour? Then be explicit about it and add `?`.

If you don't know about greediness. Read this [StackOverflow answer](http://stackoverflow.com/questions/3075130/difference-between-and-for-regex/3075532#3075532).

## `u` modifier

Lower case letter `u`. If this modifier is set, the input string and regex are treated as UTF-8. This means whenever you're working with UTF-8 strings, you should enable this modifier. There are some quirks, see the comments from the manual [#54805](http://php.net/manual/en/reference.pcre.pattern.modifiers.php#54805) and [#103348](http://php.net/manual/en/reference.pcre.pattern.modifiers.php#103348).

I would recommend enabling this modifier unless you're absolutely sure that you will be working with ASCII (or single-byte character sets). Note that shorthand character classes like `\w`, `\d`, `\s`, `\b`, ... will become Unicode aware when this modifier is set.

Recommended readings:

 - [UTF-8 all the way through MySQL, PHP, and HTML](http://stackoverflow.com/questions/279170/utf-8-all-the-way-through-mysql-php-and-html)
 - [Handling Unicode Front To Back In A Web App](http://kunststube.net/frontback/)