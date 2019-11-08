# Loading a CSV

CSV files are a common format in which program share information. Loading a CSV
into Postgres is realtively painless.

For example, let's say we have a table called `persons`, and that table contains
three columsn - `first_name`, `last_name`, and `email`.

We also have the following CSV file:

```csv
first_name,last_name,email
steven,leiva,sleiva@example.com
john,smith,jsmith@example.com
```

We can load this CSV file into our database by running:

```
COPY persons(first_name, last_name, email)
FROM './data.csv' with (format csv, header true);
```

Notice that in the options, we are declaring what format the file we are loading
is in, as well as the fact that our CSV file contains a header row.
