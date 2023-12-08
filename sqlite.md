# sqlite

Some useful commands


* `.tables`  <- display tables eq `SHOW TABLES`
* `.schema products`  <- displays the creating script of the table eq `DESC products`


Reading json from a field

```sqlite
SELECT JSON_EXTRACT(data, '$.timestamp') AS timestamp
FROM json_data;
```

It would raise an error if encounters rows with no valid json, to delete those rows run:

```
DELETE FROM json_data
WHERE JSON_VALID(data) = FALSE;
```
