
1. Вес таблиц в схеме
```SQL
select table_schema, table_name, pg_size_pretty(pg_total_relation_size(table_schema || '.' || table_name)) from information_schema.tables where table_schema in ('public')
```
