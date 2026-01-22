# PostgreSQL Starter

A simple PostgreSQL starter for Underscore.is.

> NOTE: You might have to reload the window once the dependencies are installed for the SQL extension to work as expected.

## Connect to DB

To connect to the DB, run the following command in terminal:

```
psql postgresql://user:mypassword@localhost:5432/youtube?sslmode=disable
psql (15.7)
Type "help" for help.
youtube=# select * from videos limit 2;
```

You can also use the SQLTools extension to inspect the DB.

## Next steps

Check the `undr-template.json` file to learn more about how the data was loaded in the DB.
