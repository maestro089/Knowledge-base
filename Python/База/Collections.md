
## Counter

Метод возвращает словарю, символ/количество вхождений

``` python

import collections
 

str = "Hello, world!"

a = collections.Counter(str)

print(a)
#Counter({'l': 3, 'o': 2, 'H': 1, 'e': 1, ',': 1, ' ': 1, 'w': 1, 'r': 1, 'd': 1, '!': 1})

print(type(a))
#<class 'collections.Counter'>

print(a.most_common(3)#Топ часто встречающиеся
#[('l', 3), ('o', 2), ('H', 1)]
```
Счетчики можно складывать и вычитать друг с другом.

Примеры использования
```python
>>> sum(letter_cnt.values()) 
# число всех посчитанных элементов 11 
>>> list(letter_cnt) 
# список уникальных элементов исходной последовательности ['а', 'б', 'р', 'к', 'д'] 
>>> set(letter_cnt) {'а', 'б', 'д', 'к', 'р'} 
>>> dict(letter_cnt) 
# счетчик это подкласс словаря, можно преобразовать в обычный dict 
{'а': 5, 'б': 2, 'р': 2, 'к': 1, 'д': 1}
>>> +c # способ вывести положительные подсчеты 
Counter({'a': 3}) 
>>> -c # способ вывести отрицательные подсчеты Counter({'c': 3, 'd': 6}) 
>>> c.clear() # Очищаем счетчик 
>>> c 
Counter()
```

## defaultdict
При попытке обратиться к несуществующему ключу обычно выдается исключение, этот метод вернет  вместо исключение деволтное исключение, пустая строка для мтрок ноль для чисел
```python
import collections

  

listTest = collections.defaultdict(int)

print(listTest[134])
# 0
```

## OrderedDict
Ощутимость пользы `OrderedDict` так повлияла на обычный `dict`, что в новых версиях Python различий между ними становится всё меньше. В былые времена `OrderedDict` кардинально отличался от обычного словаря тем, что умел запоминать порядок вставки. Но с версии Python 3.6 на [это способен и обычный словарь](https://docs.python.org/3.6/whatsnew/3.6.html#new-dict-implementation). Однако некоторые различия между ними все равно остаются:

1. Обычный `dict` был разработан, чтобы быть лучшим в операциях, связанных с [мапированием](https://ru.wikipedia.org/wiki/%D0%9C%D0%B0%D0%BF%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5). Отслеживание порядка вставки для него – дело вторичное. И наоборот, `OrderedDict` хорош в операциях переупорядочения, а эффективность, скорость итераций и производительность не главное.
2. Алгоритмически `OrderedDict` может обрабатывать частые операции переупорядочения лучше, чем `dict.`

Так как `OrderedDict` это упорядоченная последовательность, объекты содержат соответствующие методы, реорганизующие структуру:

1. `popitem(last=True)` – удаляет последний элемент если `last=True`, и первый, если `last=False`
2. `move_to_end(key, last=True)` – переносит ключ `key` в конец, если `last=True`, и в начало, если `last=False`

```python
>>> d = collections.OrderedDict.fromkeys('abcde') 
>>> d.move_to_end('b') 
>>> ''.join(d.keys()) 'acdeb' 
>>> d.move_to_end('b', last=False) 
>>> ''.join(d.keys()) 'bacde'`
```
## ChainMap
После разговора о словарях самое время обсудить класс, умеющий объединять словари в надструктуру – `ChainMap`. При этом получается не один общий словарь, а их совокупность, в которой каждый словарь остаётся независимой составляющей:

```Python
>>> letters = {'a':1, 'b':2} 
>>> vowels = {'a':1, 'b':0, 'c':0, 'd': 0, 'e':1} 
>>> chain = collections.ChainMap(letters, vowels) 
>>> chain ChainMap({'a': 1, 'b': 2}, {'a': 1, 'b': 0, 'c': 0, 'd': 0, 'e': 1})
```  

При обращении к `ChainMap` по ключу одного из словарей, происходит поиск значения среди всех словарей, при этом нет необходимости указывать конкретный словарь:

``` python
>>> chain['e'] 1
```
`

При поиске `ChainMap` выводит первое найденное значение (проходя словари по очереди добавления). В том числе если в словарях несколько одинаковых ключей:

```python
>>> chain['b'] 2
```

Изменение содержания словаря изменяет и `ChainMap`. Нет необходимости перезаписывать надструктуру:
```python
>>> letters['c'] = 3 
>>> chain ChainMap({'a': 1, 'b': 2, 'c': 3}, {'a': 1, 'b': 0, 'c': 0, 'd': 0, 'e': 1})
```


Так как `ChainMap` это комбинация словарей, логично, что у неё есть методы `keys()` и `values()`:
```python
>>> list(chain.keys()) ['c', 'd', 'a', 'e', 'b'] 
>>> list(chain.values()) [3, 0, 1, 1, 2]
    
```


Значения `values` соответствуют списку `keys`, как это было описано выше. То есть в случае несколько совпадающих ключей, выводится значение для первого из словарей, где встречается этот ключ.

При необходимости расширить составленный ранее `ChainMap` можно методом `new_child()`:
```python
>>> consons = {'a':0, 'b':1, 'c':1} 
>>> chain.new_child(consons) ChainMap({'a': 0, 'b': 1, 'c': 1}, {'a': 1, 'b': 2, 'c': 3}, {'a': 1, 'b': 0, 'c': 0, 'd': 0, 'e': 1})
    
```

Обратите внимание, что метод не обновляет старую структуру, а создаёт новую.
## Двусторонняя очередь (deque)
Объект типа `deque` (читается как «дэк», [двусторонняя или двусвязная очередь](https://ru.wikipedia.org/wiki/%D0%94%D0%B2%D1%83%D1%85%D1%81%D1%82%D0%BE%D1%80%D0%BE%D0%BD%D0%BD%D1%8F%D1%8F_%D0%BE%D1%87%D0%B5%D1%80%D0%B5%D0%B4%D1%8C)) является усовершенствованным вариантом списка с оптимизированной вставкой/удалением элементов с обоих концов. Реализация `deque` оптимизирована так, что операции слева и справа имеют примерно одинаковую производительность `O(1)`. Добавление новых элементов в конец происходит не сильно медленнее, чем во встроенных списках, но добавление в начало выполняется существенно быстрее.

 ```python
>>> seq = list("bcd") 
>>> deq = collections.deque(seq) 
>>> deq deque(['b', 'c', 'd']) 
>>> deq.append('e')      # добавление в конец 
>>> deq.appendleft('a')  # добавление в начало (левый конец) 
>>> deq deque(['a', 'b', 'c', 'd', 'e'])`
```
    

Чтобы добавлять не одиночный элемент, а группу итерируемого объекта `iterable` используйте соответственно `extend(iterable)` и `extendleft(iterable`).

Аналогично методу `append()` метод `pop()` для `deque` работает с обоих концов:

 ```python
 >>> deq.pop() 
 >>> deq.popleft() 
 >>> deq deque(['b', 'c', 'd'])`
```

Если нужно посчитать число вхождений элемента в последовательность, применяем метод `count()`:

```python
>>> deq.count('b'), deq.count('a') (1, 0)
```
Кроме перечисленных, доступны следующие методы:

1. `remove(value)` – удаление первого вхождения `value`
2. `reverse()` – разворачивает очередь)
3. `rotate(n=1)` – последовательно переносит `n` элементов из начала в конец (если `n` отрицательно, то с конца в начало). В этом поведение `deque` напоминает [кольцевой связный список](https://ru.wikipedia.org/wiki/%D0%A1%D0%B2%D1%8F%D0%B7%D0%BD%D1%8B%D0%B9_%D1%81%D0%BF%D0%B8%D1%81%D0%BE%D0%BA#%D0%9A%D0%BE%D0%BB%D1%8C%D1%86%D0%B5%D0%B2%D0%BE%D0%B9_%D1%81%D0%B2%D1%8F%D0%B7%D0%BD%D1%8B%D0%B9_%D1%81%D0%BF%D0%B8%D1%81%D0%BE%D0%BA)

Очередь `deque` имеет аргумент `maxlen`, позволяющий ограничить ее размер. При заполнении ограниченной очереди добавление `n` новых объектов «слева» вызовет удаление `n` элементов справа.

Ограниченные очереди обеспечивают функциональность, похожую на `tail`-фильтр в Unix:

 ```python
def tail(filename, n=10):     
	"""Возвращает n последних строк файла'"""     
	with open(filename) as f:         
		return collections.deque(f, n)
```

Другой шаблон применения `deque` – хранение последних добавленных элементов с выбрасыванием более старых. Пример компактной и быстрой реализации функции [скользящего среднего](https://ru.wikipedia.org/wiki/%D0%A1%D0%BA%D0%BE%D0%BB%D1%8C%D0%B7%D1%8F%D1%89%D0%B0%D1%8F_%D1%81%D1%80%D0%B5%D0%B4%D0%BD%D1%8F%D1%8F):

   ```python
import itertools  
def moving_average(iterable, n=3):     
	# moving_average([40, 30, 50, 46, 39, 44]) --> 40.0 42.0 45.0 43.0     
	it = iter(iterable)     
	d = collections.deque(itertools.islice(it, n-1))     
	d.appendleft(0)     
	s = sum(d)     
	for elem in it:        
		s += elem - d.popleft()         
		d.append(elem)         
		yield s / n`
```

Алгоритм распределения нагрузки [Round-robin](https://ru.wikipedia.org/wiki/Round-robin_(%D0%B0%D0%BB%D0%B3%D0%BE%D1%80%D0%B8%D1%82%D0%BC)) можно реализовать с помощью итераторов, хранящихся в `deque`. Значения выводятся из активного итератора в нулевой позиции. Если этот итератор исчерпан, его можно удалить методом `popleft ()`; в противном случае его можно циклически «провернуть» до конца методом `rotate()`:

```python
def roundrobin(*iterables):     
	"roundrobin('ABC', 'D', 'EF') --> A D E B F C"     
	iterators = collections.deque(map(iter, iterables))     
	while iterators:         
	try:             
		while True:                 
		yield next(iterators[0])                 
		iterators.rotate(-1)         
	except StopIteration:             
		# Удалить "закончившийся" итератор`
		iterators.popleft()
```

## Именованный кортеж и функция namedtuple()

`namedtuple()` – функция-фабрика для создания именованных кортежей. Этот тип данных похож на `struct` в других языках программирования:

```python
>>> cols = ['fname', 'pname', 'lname', 'age'] 
>>> User = collections.namedtuple('User', cols) 
>>> user1 = User('Петр', 'Иванович', 'Сидоров', 30) 
>>> user1 User(fname='Петр', pname='Иванович', lname='Сидоров', age=30) 
>>> user1.lname Сидоров 
>>> Point = collections.namedtuple('Point', ['x', 'y']) 
>>> p = Point(3, 4) 
>>> p.x**2 + p.y**2 25
```
Именованные кортежи делают код яснее – вместо индексирования составляющие объекта вызываются по явным именам. Остаётся доступной и численная индексация:
```python
>>> p[0]**2 + p[1]**2 25
```

Именованные кортежи часто используются для назначения имён полей кортежам, возвращаемым модулями `csv` или `sqlite3`:

```python
EmployeeRecord = collections.namedtuple('EmployeeRecord', 'name, age, title, department, paygrade')  
import csv 
	for emp in map(EmployeeRecord._make, csv.reader(open("employees.csv", "rb"))):     print(emp.name, emp.title) 
	 
import sqlite3 conn = sqlite3.connect('/companydata') 
cursor = conn.cursor() 
cursor.execute('SELECT name, age, title, department, paygrade FROM employees') 
for emp in map(EmployeeRecord._make, cursor.fetchall()):     
	print(emp.name, emp.title)
```

Структура `namedtuple` похожа на словарь. Посредством метода `_asdict` можно представить те же данные в виде `OrderedDict`:
```python
>>> p._asdict() 
OrderedDict([('x', 3), ('y', 4)])
```
Чтобы вызвать значение через строковый ключ, необязательно преобразовывать `namedtuple` – подходит стандартная функция `getattr()`:
```python
>>> getattr(p, 'x') 
3
```
Чтобы преобразовать словарь в именованный кортеж заданного типа, достаточно распаковать его оператором `**`:

  ```python
>>> d = {'x': 0, 'y': 1} 
>>> Point(**d) 
Point(x=0, y=1)
```

Имена полей `namedtuple` перечислены в `_fields`:

  ```python
>>> user1._fields, p._fields (('fname', 'pname', 'lname', 'age'), ('x', 'y'))
```

С версии 3.7 можно присвоить полям [значения по умолчанию](https://docs.python.org/3/library/collections.html#collections.somenamedtuple._field_defaults).

Поскольку именованный кортеж является обычным классом Python, в него легко привнести новую функциональность или изменить старую. Например, добавим к `Point` расчёт гипотенузы и формат вывода данных:

  ```python
class Point(collections.namedtuple('Point', ['x', 'y'])):     
	_slots__ = ()  
	# предотвращает создание словарей экземпляров     
	@property     
	def hypot(self):         
		return (self.x**2 + self.y**2) ** 0.5     
		
	def __str__(self):         
		return 'Point: x=%6.3f  y=%6.3f  hypot=%6.3f' % (self.x, self.y, self.hypot)  
		
for p in Point(3, 4), Point(14, 5/7):     
	print(p)
    

Point: x= 3.000  y= 4.000  hypot= 5.000 
Point: x=14.000  y= 0.714  hypot=14.018
```

Если вам пришлась по душе компактность `namedtuple` в сравнении с обычными классами и ваш проект может работать с версиями Python не меньше 3.7, присмотритесь к модулю [dataclasses](https://docs.python.org/3/library/dataclasses.html). Эта встроенная библиотека предоставляет декоратор и функции для автоматического добавления в пользовательские классы сгенерированных специальных методов, таких как `__init__()` и `__repr__()`.