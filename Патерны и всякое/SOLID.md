  
SOLID  - это аббревиатура от 5 принципов, описанных Робертом Мартином, которые способствуют созданию хорошего объектно-ориентированного (и не только) кода.

1. **S**: Single Responsibility Principle (Принцип единственной ответственности).

 Каждый класс должен решать лишь одну задачу.
 
Вот пример кода, который нарушает данный паттерн.

```python
class Database:  
	def __init__(self, name):  
		self.name = name  
	  
	def select(self, table):  
	...  
	  
	def insert(self, table):  
	...  
	  
	def draw(self):  
	...

```
Класс, который отвечает за работу с БД, имеет функцию draw, которая рисует график - это неверно, так как класс отвечает за работу с БД, ни чего другого не должно быть.

Вот правильный вариант:

```python
class Database:  
	def __init__(self, name):  
		self.name = name  
	  
	def select(self, table):  
	...  
	  
	def insert(self, table):  
	...  
  
  
class DataDrawer:  
	def draw(self):  
	...

```

Я вынес лишнюю функцию в другой класс и теперь все занимаются своими делами.

2. **O**: Open-Closed Principle (Принцип открытости-закрытости).
Программные сущности (классы, модули, функции) должны быть открыты для расширения, но не для модификации.

```python
from abc import abstractmethod, ABC  
  
  
class Database(ABC):  
	@abstractmethod  
	def select(self):  
		pass  
  
class MySql(Database):  
	def select(self):  
		pass  
  
class PostgreSql(Database):  
	def select(self):  
		pass
```
Суть паттерна в том, что бы мы не изменяли основной класс для внесения нововго функционала, а унаследовались от него и писать логику под конкретную задачу.


2. Liskov Substitution Principle (Принцип подстановки Барбары Лисков).

Необходимо, чтобы подклассы могли бы служить заменой для своих суперклассов.

Суть паттерна заключается в том, что дочерний класс мог заменить родительский

```python
#Родительский класс
class Database:  
	def __init__(self, name):  
		self.name = name  
  
	def select(self, table):  
		...  
	  
	def insert(self, table):  
		...
	# МЫ решили изменить немного функционал, пример как нельзя делать
	# В select добавил новый паарметр без дефолтного значения 
class Database2(Database):  
	def __init__(self, name):  
		super().__init__(name)  
	  
	def select(self, table, param):  
		...

#Правильный вариант:
class Database2(Database):  
	def __init__(self, name):  
		super().__init__(name)  
	  
	def select(self, table, param=None):  
		...


```
Теперь во всем коде можно класс Database заменить на Database2 и всё будет работать.

4. Interface Segregation Principle (Принцип разделения интерфейса).
Создавайте узкоспециализированные интерфейсы, предназначенные для конкретного клиента. Клиенты не должны зависеть от интерфейсов, которые они не используют.

``` python
class Printer(ABC):  
	@abstractmethod  
	def print(self, document):  
		pass  
  
class Scanner(ABC):  
	@abstractmethod  
	def scan(self, document):  
		pass  
  
class Fax(ABC):  
	@abstractmethod  
	def fax(self, document):  
		pass 
  
class AllInOne(Printer):  
	def print(self, document):  
		print(f"{document}")  
	
```

Мы предоставляем пользователю только функционал принтера, так как сканер и факс ему не нужен.

5. Dependency Inversion Principle (Принцип инверсии зависимостей).
Объектом зависимости должна быть абстракция, а не что-то конкретное.
    - Модули верхних уровней не должны зависеть от модулей нижних уровней. Оба типа модулей должны зависеть от абстракций.
    - Абстракции не должны зависеть от деталей. Детали должны зависеть от абстракций.
```python

from abc import ABC, abstractmethod  
  
  
class Notification(ABC):  
	@abstractmethod  
	def send_notification(self, message):  
		pass  
  
  
class EmailSender(Notification):  
	def send_notification(self, message):  
	# Логика отправки уведомления по электронной почте  
		pass  
	  
  
class SMSNotification(Notification):  
	def send_notification(self, message):  
	# Логика отправки уведомления по SMS  
		pass  
  
  
class User:  
	def __init__(self, username, email, notification_service):  
		self.username = username  
		self.email = email  
		self.notification_service = notification_service 
  
	def send_notification(self, message):  
		self.notification_service.send_notification(message)
```

Классы СМС и Почты унаследуются от одного абстрактного класса.
Класс User принимает не конкретный класс рассылки, их родительский класс.