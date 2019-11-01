# Update

When updating data in a particular table, we often only want to update a subset
of the rows in the table. If the filter logic only utilizes data from the
current table, then SQL UPDATE statement boils down to:

```sql
UPDATE persons
SET first_name = 'Alex', last_name = 'Rodriguez'
WHERE id = 1
```

However, if our filter logic requires data from other tables, we have a choice
to make. We can either use a *FROM clause*, or we can use a *sub-select* in the
WHERE clause.

```sql
UPDATE persons
SET first_name = 'Alex', last_name = 'Rodriguez'
FROM cats
WHERE
  persons.id = cats.person_id
```

When we go the *FROM clause* route, we need to be aware that we are building a
table that is the *Cartesian product* between all the tables listed. In our
example, we would get every possible persons / cats combo. It is up to the
*WHERE clause* to do the actual filtering. (Note that the statement above would
update every person's name to Alex Rodriguez if they "owned" a cat).

The above query can be expressed using a *sub-select* as well.

```sql
UPDATE persons
SET first_name = 'Alex', last_name = 'Rodriguez'
WHERE persons.id IN (
  SELECT person_id
  FROM cats
)
```

It is often [safer](https://www.postgresql.org/docs/11/sql-update.html) to use a
*sub-select*.
