# Comments

We always advise to write documented code. So why should we ignore this when writing regexes?  There's two ways to include comments into PCRE* regexes: inline comments and eXtended mode. There's also a php comments. The eXtended mode is commonly used.

## Inline comments

The inline comment goes like this: `/[0-9]+(?#match digits)/`. It's simple and direct. The only drawback is that it can get quite messy with bigger regexes: `/[0-9]+(?#match digits)[a-z]+(?#followed by letters),?(?#optional comma)[0-9]+(?#followed by digits)/`. 

## PHP comments

This technique consists of using php comments instead of regex comments:

```php
to be continue
```


## eXtended mode


