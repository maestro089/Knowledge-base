
В Airflow есть несколько способов задать параметры через веб интерфейс

### Variable

На web странице переходим в admin->Variable и создаем переменную.
В коде, доступ к переменной выглядит так:

```python
from airflow.models import Variable

myVar = Variable.get("Название нашей переменной", default="Если ненайден паарметр")
```

### Connection
На web странице переходим в admin->Connetions и создаем подключение.
В коде выглядит так:

```python
from airflow.hooks.base import BaseHook

connection = BaseHook.get_connection("1")  
print(f"Variable!{connection.host}")
```
Данный способ использоется для подключения к разным сервисам, БД и т.д.