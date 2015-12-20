# Comments

We always advise to write documented code. So why should we ignore this when writing regexes?  There's two ways to include comments into PCRE* regexes: inline comments and eXtended mode. There's also a php comments. The eXtended mode is commonly used.

## Inline comments

The inline comment goes like this: `/[0-9]+(?#match digits)/`. It's simple and direct. The only drawback is that it can get quite messy with bigger regexes: `/[0-9]+(?#match digits)[a-z]+(?#followed by letters),?(?#optional comma)[0-9]+(?#followed by digits)/`. 

## PHP comments

This technique consists of using php comments instead of regex comments:
```php
// Match one or more digits
$regex = '/[0-9]+/';
```
There's also a JavaScript way of commenting big regexes, basically you concatenate chunks of strings:
```php
$regex = '/' .       // delimiter
         '/[0-9]+' . // match digits
         '[a-z]+' .  // followed by letters
         ',?' .      // optional comma
         '[0-9]+' .  // followed by digits
         '/';        // no modifiers
```

## eXtended mode

This is the preferred and most common way of implementing comments by using the `x` modifier:

```php
$regex = '/       # delimiter
          [0-9]+  # match digits
          [a-z]+  # followed by letters
          ,?      # optional comma
          [0-9]+  # followed by digits
          /x';    # x modifier
```
Basically spaces are ignored, everything after a hashtag will get ignored as well.
Note that the last comment `# x modifier` is actually a php comment.
Here comes the **pitfall**: since spaces are ignored, what if I want to match a space?
There are several ways, I will show two of them. Say I want to match one or more spaces:

- Escaping the space `$regex = '/\ +/x';`
- Using a character class: `$regex = '/[ ]+/x'`

That said, regardless of the `x` modifier, I would always enclose the space in a character class. It's clear that way.