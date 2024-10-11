Инициализация:

``` python
alembic init -t async alembic
```
Флаг *-t async* необязательны, он используется для ассинхроной поддержки.

Нужно в файл env.py добавить метаданные моделей таблиц, так же в файл alembic.ini добавить ссылку для подключения к БД.

Теперь в консоли нужно вписать:

``` python
alembic revision --autogenerate -m "Initial tables"
```
Создаем ревизию:
```python
alembic upgrade head 
```
Запуск последней созданной ревизии