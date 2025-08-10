---
tags:
  - sqpark
  - oracle
  - database
---
1. Делаем шаги со страницы [[Базы данных/Oracle/Функции|Функции]]
2. На основе пункта 1 делаем запрос 
```sql
SELECT t.id, t.name, t.email,t.REGISTRATION_DATE, t.amount, t.status, '1024_4' data_chunk_id

FROM SYS.LARGE_DATA_TABLE t

WHERE ((rowid >= dbms_rowid.rowid_create(1, 75865, 1, 27472, 0) AND rowid <= dbms_rowid.rowid_create(1, 75865, 1, 35431, 0)))

UNION ALL

SELECT t.id, t.name, t.email,t.REGISTRATION_DATE, t.amount, t.status, '1024_4' data_chunk_id

FROM SYS.LARGE_DATA_TABLE t

WHERE ((rowid >= dbms_rowid.rowid_create(1, 75865, 1, 35456, 0) AND rowid <= dbms_rowid.rowid_create(1, 75865, 1, 36095, 0)))
```

## ССЫЛКИ
[ХАБР](https://habr.com/ru/companies/beeline/articles/679876/)
