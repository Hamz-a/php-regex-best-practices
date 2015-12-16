# Avoid using regex whenever possible

Let's face it, regex is hard. So avoid using them whenever possible. Just to name a few cases where you should absolutely avoid them with a suitable alternative solution:

- **Validating number ranges:** use php comparison operators
- **Validating email addresses:** use [`filter_var()`](http://php.net/filter_var) with `FILTER_VALIDATE_EMAIL` and/or send an email already*
- **Validating IP addresses:** use [`filter_var()`](http://php.net/filter_var) with `FILTER_VALIDATE_IP`
- **Validating URLs:** use [`filter_var()`](http://php.net/filter_var) with `FILTER_VALIDATE_URL`*
- **Validating dates:** see this [Stack Overflow thread](http://stackoverflow.com/questions/19271381/correctly-determine-if-date-string-is-a-valid-date-in-that-format)
- **Parsing JSON:** use [`json_decode()`](http://php.net/json_decode)
- **Parsing HTML / XML:** use a dedicated parser. See [*How do you parse and process HTML/XML in PHP?*](http://stackoverflow.com/questions/3577641/how-do-you-parse-and-process-html-xml-in-php)
- **Parsing CSV:** use [`str_getcsv()`](http://php.net/manual/en/function.str-getcsv.php) or [`fgetcsv()`](http://php.net/manual/en/function.fgetcsv.php)
- **Parsing complex grammars:** there's almost always a dedicated parser for your favourite language
- **Check if string contains substring:** use [`strpos`](http://php.net/strpos) or [`stripos`](http://php.net/stripos)

A few other cases you *might* consider avoiding:
- **Name validation:** read about [*Falsehoods Programmers Believe About Names*](http://www.kalzumeus.com/2010/06/17/falsehoods-programmers-believe-about-names/)
- **Address validation:** read about [*Falsehoods programmers believe about addresses*](https://www.mjt.me.uk/posts/falsehoods-programmers-believe-about-addresses/)

\* It might not work with internationalized domains and email addresses. See [top comments](https://secure.php.net/manual/en/function.filter-var.php#111828) at the documentation.