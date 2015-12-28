# Comments

We always advise to write documented code. So why should we ignore this when writing regexes?  There are several techniques to document your regex. Using eXtended mode is the recommended way. I took the regex from this [StackOverflow answer](http://stackoverflow.com/a/3802238) to demonstrate this method.

## eXtended mode

This is the preferred and most common way of implementing comments by using the `x` modifier:

```php
// Regex for password validation
$regex = '/
^                 # start-of-string
(?=.*[0-9])       # a digit must occur at least once
(?=.*[a-z])       # a lower case letter must occur at least once
(?=.*[A-Z])       # an upper case letter must occur at least once
(?=.*[@#$%^&+=])  # a special character must occur at least once
(?=\S+$)          # no whitespace allowed in the entire string
.{8,}             # anything, at least eight places though
$                 # end-of-string
/x';
```

Basically spaces are ignored, everything after a hashtag will get ignored as well.
Here comes the **pitfall**: since spaces are ignored, what if I want to match a space?
There are several ways, I will show two of them. Say I want to match one or more spaces:

- Escaping the space `$regex = '/\ +/x';`
- Using a character class: `$regex = '/[ ]+/x'`

That said, regardless of the `x` modifier, I would always enclose the space in a character class. It's clear that way.