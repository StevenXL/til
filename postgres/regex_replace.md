# Regex Replace

Postgres comes with [a slew of functions that operate on
strings](https://www.postgresql.org/docs/11/functions-string.html).

One such function that I find myself reaching for often is `regexp_replace`.
This function allows us to replace a substring that matches a certain pattern.

For example, in a web app that I work with often, there is a column on the
user's table called `phone`, with no validation / normalization done on the
data. As a result, users can enter a phone number with dashes, parenthesis, or
other characters - e.g., `(202) 456-1111`, `202-456-1111`, etc.

However, when I want to export this data, I want to normalize the phone number a
bit more. If I want to only keep digits, I can run:

```
SELECT
  regexp_replace(phone1, '\D', '', 'ig')
FROM
  accounts
```

Notice the 4th argument to the `regexp_replace` function - a list of options to
modify the function's behavior.
