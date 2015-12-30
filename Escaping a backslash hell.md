# Escaping a backslash hell

Backslash hell is when you need to escape a lot of characters which results into cluttering your regex with a lot of backslashes. The following tips will help you escape such a mess.

## Choosing the right delimiter

The forward slash `/` is commonly used as a delimiter in the regex world. Sometimes it might be better to use different delimiters. Especially when you have some forward slashes in your regex:

```php
$regex = '/^\/user\/(\d+)\/?/i';

$regex = '~^/user/(\d+)/?~i';
```

Other common delimiters are `#`, `!`, `%`. Keep in mind to use one that's less likely to be included in your regex.