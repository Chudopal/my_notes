# Python
[примеры кода здесь](https://github.com/Chudopal/Python_learning)

**Интерпретируемый высокоуровневый ЯП.**

+ [Переменные и простые типы данных](#var_and_simple_data)
+ [Структуры данных](#structs)
  + [Списки](#lists)
    + [Срезы списков](#slices)
  + [Кортежи](#pulps)
  + [Словари](#dictionaries)
  + [Множества](#sets)
+ [Функции](#functions)
  + [Декораторы](#decorators)
+ [Работа с файлами](#files)
+ [Работа JSON](#json)
  + [Чтение](#readjson)
  + [Запись](#writejson)
+ [ООП](#oop)
  + [Декораторы класса](#class_decorators)
  + [Метаклассы](#metaclasses)
  + [Итераторы](#iterators)
  + [Генераторы](#generators)
+ [Объектная модель в Python](#object_model)
+ [Пакеты и модули](#packeges)
+ [Потоки](#threads)
+ [Сокеты](#sockets)
+ [Дополнение](#dop)
  + [Ключевые слова](#keywords)



## <a name="var_and_simple_data"></a> Переменные и простые типы данных
Python - это язык с динамической типизацией, то есть переменные могут хранить любые типы данных без их явного указания.
+ Строки можно конкатенировать при помощи знака `+`
+ Кроме того, если написать например `s*2`, где s - строка, то данная строка просто сложится сама с собой
+ Действия над числами:
  + `+` - сложение
  + `-` - разность
  + `*` - умножение
  + `**`- возведение в степень
  + `/` - деление как вещественных чисел
  + `//` - целочисленное деление
  + `%` - остаток от деления
 + Преобразование типов:
    + `str(num)` - преобразовать число в строку
    + `int(str)` - преобразовать строку в число
    + `float(str)` - преобразование в число с плавающей точкой
## <a name="structs"></a> Структуры данных
**Структуры данных** – это структуры, которые могут хранить некоторые данные вместе. Другими словами, они используются для хранения связанных данных.
В Python существуют четыре встроенных структуры данных: список, кортеж, словарь и множество.

### <a name="lists"></a> Списки
+ Список — это набор элементов, следующих в определенном порядке. Можно
    создать список для хранения букв алфавита, цифр от 0 до 9 или слов. 
    В список можно поместить любую информацию, причем данные
    в списке даже не обязаны быть как-то связаны друг с другом. Так как список обычно содержит более одного элемента, рекомендуется присваивать спискам имена
    во множественном числе: letters, digits, names и т. д.
    ``` python
    bicycles = ['trek', 'cannondale', 'redline', 'specialized'] #это список
    ```
+ Чтобы обратиться к элементу списка, необходимо после его имени поставить квадратные скобки, в которых написать его индекс.
    ```python
    bicycles[0] #обращение к элементу 'trek'
    ```
+ Элементы в спике можно изменять. Для этого нужна обычная операция присвоения:
    ```python
    bicycles[0] = 'ducati'#['ducati','cannondale', 'redline', 'specialized']
    ```
+ Элементы можно добавлять в конец списка:
    ```python
    bicycles.append['new'] #['ducati','cannondale', 'redline', 'specialized', 'new']
    ```
+ Для перебора списка удобно использовать цикл `for`:
```python
cars = ['audi','tayota','bmw','suzuki']
for car in cars:#цикл вывидет все элементы списка
    print(car)
```
+ Генерация списков. Цикл `for` можно вставлять прямо в цикл для его создания, это называется генерацией списков(**list comprehension**):
```python
squares = [value**2 for value in range(1,11)]#сгенерирует список с квадратами чисел от 1 до 10
```
+ **enumerate(list)** - функция позволяет получить доступ к элементам и их индексам в списке в виде кортежей:
  ```python
  >>> a = [10, 20, 30, 40]
  >>> for i in enumerate(a):
  ...     print(i)
  ... 
  (0, 10)
  (1, 20)
  (2, 30)
  (3, 40)
  ```
  ```python
  >>> a = [10, 20, 30, 40]
  >>> for id, item in enumerate(a):
  ...     a[id] = item + 5
  ... 
  >>> a
  [15, 25, 35, 45]
  ```
+ #### <a name="slices"></a> Срезы списков
  + **item[START:STOP:STEP]** - берёт срез от номера START, до STOP (не включая его), с шагом STEP. По умолчанию START = 0, STOP = длине объекта, STEP = 1.
  + Чтобы создать срез списка, следует задать индексы первого и последнего элементов, с которыми вы намереваетесь работать. Python
    останавливается на элементе, предшествующем второму индексу. Скажем, чтобы вывести первые три элемента списка, запросите индексы с 0 по 3, и вы получите
    элементы 0, 1 и 2:
    ```python
    players = ['charles', 'martina', 'michael', 'florence', 'eli']
    print(players[0:3])#['charles', 'martina', 'michael']
    ```
  + Подмножество может включать любую часть списка. Например, чтобы ограничиться вторым, третьим и четвертым элементами списка, срез начинается с индекса 1 и              заканчивается на индексе 4:
    ```python
    players = ['charles', 'martina', 'michael', 'florence', 'eli']
    print(players[1:4])#['martina', 'michael', 'florence']
    ```
  + Если первый индекс не указан, то Python автоматически начинает с 0-го:
    ```python
    print(players[:4])#['charles', 'martina', 'michael', 'florence']
    ```
  + Аналогичный синтаксис работает и для срезов, включающих конец списка:
    ```python
    players = ['charles', 'martina', 'michael', 'florence', 'eli']
    print(players[2:])#['michael', 'florence', 'eli']
    ```
  + **Отрицательный** индекс возвращает элемент, находящийся на заданном расстоянии от конца списка; следовательно, можно получить любой срез от конца списка:
    ```python
    print(players[-3:])#['michael', 'florence', 'eli']
    ```
  + Копировать списки в Python не получится обычным оператором присваиавния, так как скопируется ссылка на объкт и, при изменении одного списка будет изменен и второй,    потому стоит использовать срезы для копирования:
    ```python
    array1 = [1,2,3,4]
    array2 = array1[:]#скопируются все элементы первого списка
    ```
  + В списках также можyо указывать шаг:
    ```python
    a = [1, 3, 8, 7]
    print(a[::2])#[1, 8]
    ```


### <a name="pulps"></a> Кортежи
Кортеж выглядит как список, не считая того, что вместо квадратных скобок используются круглые скобки. После определения кортежа вы можете обращаться к его отдельным элементам по индексам точно так же, как это делается при работе со списком. Однако кортеж, в отличае от списка, является неизменяемым.
```python
dimensions = (200, 50)
```
 + Элементы кортежа не могут изменяться, онако можно присвоить новое значение переменной, в которой хранится кортеж:
 ```python
 dimensions = (200, 50)
 dimensions = (400, 100)
 ```


### <a name="dictionaries"></a> Словари
Словарь - структура данных, предназначенная для объединения взаимосвязанной информации.
 + Создание словаря. Словарь очень похож на список, однако вместо числового индекса элемента здесь необходимо использовать индекс-слово. Для инициализации используются {}:
 ```python
 alien_0 = {'color': 'green', 'points': 5}
 print(alien_0['color'])#green
 print(alien_0['points'])#5
 ```
 + Добавление новых пар "ключ - значение":
 ```python
 alien_0 = {'color': 'green', 'points': 5}
 print(alien_0)#{'color': 'green', 'points': 5}
 alien_0['x_position'] = 0
 alien_0['y_position'] = 25
 print(alien_0)#{'color': 'green', 'points': 5, 'y_position': 25, 'x_position': 0}
 ```
 + Изменение значений в словаре:
 ```python
 alien_0 = {'color': 'green'}
 alien_0['color'] = 'yellow'
 ```
 + Удаление пар "ключ-значение":
 ```python
 del alien_0['points']
 ```
 + Перебор всех пар "ключ-значение":
 ```python
 for key, value in alien_0.items():
     print("\nKey: " + key)
     print("Value: " + value)
 ``` 
 + Перебор всех ключей в словаре:
 ```python
 for name in favorite_languages.keys():
     print(name.title())
 ```
   На самом деле перебор ключей используется по умолчанию при переборе словаря,
   так что этот код будет работать точно так же, как если бы вы написали:
 ```python
 for name in favorite_languages:
 ```
 + Перебор всех значений в словаре:
 ```python
 for language in favorite_languages.values():
     print(language.title())
 ```
 + Список словарей:
 ```python
 alien_0 = {'color': 'green', 'points': 5}
 alien_1 = {'color': 'yellow', 'points': 10}
 alien_2 = {'color': 'red', 'points': 15}
 aliens = [alien_0, alien_1, alien_2]
 ```
 + Список в словаре:
 ```python
 pizza = {
    'crust': 'thick',
    'toppings': ['mushrooms', 'extra cheese'],
 }
 ```
 + Словарь в словаре:
 ```python
 users = {
    'aeinstein': {
    'first': 'albert',
    'last': 'einstein',
    'location': 'princeton',
    },
    'mcurie': {
    'first': 'marie',
    'last': 'curie',
    'location': 'paris',
    },
 }
 ```
### <a name="sets"></a> Множества

## <a name="functions"></a>
+ Функции - это части кода которые можно использовать повторно неоднократное количество раз. Определяются при помощи ключевого слова **`def`**:
  ```py
  def print_hello():
    print("hello")
  ```
+ функции можно вызывать, тогда код, прописанный внутри них, будет исполняться:
  ```py
  print_hello
  #hello
  ```
+ функциям можно передавать параметры, для удобства можно ууказывать тип передаваемого аргумента:
  ```py
  def print_hello(name: str):
    print( f"hello, {name}")

  
  print_hello("Alex")
  #hello, Alex
  ```
+ Переменные, что объявленны внутри функции могут использоваться только в ней. Так же в функции могут быть использованы глобальные переменные, чтобы изменить их значение, можно использовать ключевое слово **`global`**:
  ```py
  a = 1
  b = 2
  c = 3

  def foo():
      a = 4
      print(a)#4
      b = 5
      print(b)#5
      global c
      c =  6
      print(c)#6


  foo()
  print(a)#1
  print(b)#2
  print(c)#6
  ```

+ функции могут возвращать значения при помощи ключевого слова **`return`**, для удобства можно указывать тип, который возвращает функция:
  ```py
  def  print_and_return_hello(name: str) -> str:
    print(f"hello, {name}")
    return f"hello, {name}"

  string = print_and_return_hello("Alex") #hello, Alex 
  print(string) #hello, Alex
  ```
+ функции, как и любые сущности в pyhon, являются объектами, потому их можно присваивать другим переменным:
  ```py
  def  print_and_return_hello(name: str) -> str:
    print(f"hello, {name}")
    return f"hello, {name}"

  foo = print_and_return_hello #передаем без вызова функции
  foo("Alex") #hello, Alex
  ```
### <a name="decorators"> </a> Декораторы
+ Декоратор — паттерн проектирования, при использовании которого класс или функция изменяет или дополняет функциональность другого класса или функции без использования наследования или прямого изменения исходного кода. Они оборачивают функцию и позволяют выполнять код до и после обернутой функции.
  ```py
  def null_decorator(func):
    print("decorize")
    return func

  @null_decorator
  def greet():
    return 'Hello'

  print(greet()) #decorize Hello
  ```
+ можно изменять состояние функции внутри декоратора, а так же применять несколько декораторов вместе:
  ```py
  def strong(func):
    def wrapper():
    return '<strong>' + func() + '</strong>'
    return wrapper

  
  def emphasis(func):
    def wrapper():
    return '<em>' + func() + '</em>'
    return wrapper


  @strong
  @emphasis
    def greet():
    return 'Привет!'  
    

  greet() #'<strong><em>Привет!</em></strong>'
  ```
+ Если в декорируемую функцию передать аргументы, то в декораторе можно использовать **`args*`** и **`kwargs*`**:
  ```py
  def trace(func):
    def wrapper(*args, **kwargs):
      print(f'ТРАССИРОВКА: вызвана {func.__name__}() '
      f'с {args}, {kwargs}')
      original_result = func(*args, **kwargs) #здесь выполняется функция
      print(f'ТРАССИРОВКА: {func.__name__}() '
      f'вернула {original_result!r}')
      return original_result
    return wrapper

  @trace 
  def say(name, line):
    return f'{name}: {line}'
  
  #output
  say('Джейн', 'Привет, Мир')
  'ТРАССИРОВКА: вызвана say() с ("Джейн", "Привет, Мир"), {}'
  'ТРАССИРОВКА: say() вер
  ```

+ Замыкание - это комбинация функции и множества ссылок на переменные в области видимости функции. Когда завершается функция, все ее локальные переменные уничтожаются, однако, можно сделать так, чтобы на эти переменные где-то существовала ссылка. В таком случае они будут все еще доступны. Такой прием и является **замыканием**. Функции, постоенные по принципу замыкания могут использоваться для построения других функций, т.е. являются фабриками функций:
  ```py
  def mul(a):
      def helper(b):
          return a * b
      return helper

  mul5 = mul(5)
  print(mul5(9))#45
  ```
+ Замыкание можно использовать при построении декораторов. Такой подход часто применяется в Flask:
  ```py
  def logged(time_format):
    def decorator(func):
        def decorated_func(*args, **kwargs):
            print("- Running '{}' on {} ".format(
                func.__name__,
                time.strftime(time_format)
            ))
            start_time = time.time()
            result = func(*args, **kwargs)
            end_time = time.time()
            print("- Finished '{}', execution time = {:0.3f}s ".format(
                func.__name__,
                end_time - start_time
            ))
            return result
        decorated_func.__name__ = func.__name__
        return decorated_func
    return decorator

  @logged("%b %d %Y - %H:%M:%S")
  def add1(x, y):
      time.sleep(1)
      return x + y


  @logged("%b %d %Y - %H:%M:%S")
  def add2(x, y):
      time.sleep(2)
      return x + y


  print(add1(1, 2))
  print(add2(1, 2))

  # Output:
  - Running 'add1' on Jul 24 2013 - 13:40:47
  - Finished 'add1', execution time = 1.001s
  3
  - Running 'add2' on Jul 24 2013 - 13:40:48
  - Finished 'add2', execution time = 2.001s
  3
  ```

+ Декоратор может быть методом класса. В этом случае в качестве первого параметра используется self. Пример из Flask: 
  ```py
  def setupmethod(f):
    """Wraps a method so that it performs a check in debug mode if the
    first request was already handled.
    """
    def wrapper_func(self, *args, **kwargs):
        if self.debug and self._got_first_request:
            raise AssertionError('A setup function was called after the '
                'first request was handled.  This usually indicates a bug '
                'in the application where a module was not imported '
                'and decorators or other functionality was called too late.\n'
                'To fix this make sure to import all your view modules, '
                'database models and everything related at a central place '
                'before the application starts serving requests.')
        return f(self, *args, **kwargs)
    return update_wrapper(wrapper_func, f)
  ```

## <a name="files"></a> Работа с файлами
+ **`open("path", "mode")`** - открыть файл. `"path"` - путь к файлу от самой программы, `"mode"` - режим, в котором будет открыт файл:
  + `'r'` - открытие на чтение (является значением по умолчанию).
  + `'w'` - открытие на запись, содержимое файла удаляется, если файла не существует, создается новый.
  + `'x'` - открытие на запись, если файла не существует, иначе исключение.
  + `'a'` - открытие на дозапись, информация добавляется в конец файла.
  + `'b'` - открытие в двоичном режиме.
  + `'t'` - открытие в текстовом режиме (является значением по умолчанию).
  + `'+'`- 	открытие на чтение и запись.
  + *Режимы могут быть объединены, то есть, к примеру, 'rb' - чтение в двоичном режиме. По умолчанию режим равен 'rt'.*
+ **`read()`** - метод для считывания файла:
  ```python
  f = open('text.txt')
  f.read()
  ```
+ **`write(info)`** - запись в файл. `info` - информация, котороую необходимо записать:
  ```python
  f = open('text.txt', 'w')
  f.write("hello world)
  ```
+ **`close()`** - после работы с файлом, его необходимо закрыть:
  ```python
  f = open('text.txt', 'w')
  f.write("hello world)
  f.close()
  ```

## <a name="json"></a> Работа с JSON
JSON по синтаксису очень похож на Python словари и достаточно удобен для восприятия.
В Python есть модуль, который позволяет легко записывать и читать данные в формате JSON.

Предположим есть файл:
```python
{
  "access": [
    "switchport mode access",
    "switchport access vlan",
    "switchport nonegotiate",
    "spanning-tree portfast",
    "spanning-tree bpduguard enable"
  ],
  "trunk": [
    "switchport trunk encapsulation dot1q",
    "switchport mode trunk",
    "switchport trunk native vlan 999",
    "switchport trunk allowed vlan"
  ]
}
```

### <a name="readjson">Чтение</a>
Для чтения есть 2 метода:
+ **json.load()** - метод считывает *файл* в формате JSON и возвращает объекты Python:
  ```python
  import json

  with open('sw_templates.json') as f:
      templates = json.load(f)

  print(templates)

  for section, commands in templates.items():
      print(section)
      print('\n'.join(commands))
  ```
  Вывод:
  ```python
  $ python json_read_load.py
  {'access': ['switchport mode access',   'switchport access vlan', 'switchport   nonegotiate', 'spanning-tree portfast',   'spanning-tree bpduguard enable'], 'trunk':   ['switchport trunk encapsulation dot1q',  'switchport mode trunk', 'switchport trunk   native vlan 999', 'switchport trunk allowed   vlan']}
  access
  switchport mode access
  switchport access vlan
  switchport nonegotiate
  spanning-tree portfast
  spanning-tree bpduguard enable
  trunk
  switchport trunk encapsulation dot1q
  switchport mode trunk
  switchport trunk native vlan 999
  switchport trunk allowed vlan
  ```
+ **json.loads()** - метод считывает *строку* в формате JSON и возвращает объекты Python:
  ```python
  import json

  with open('sw_templates.json') as f:
      file_content = f.read()
      templates = json.loads(file_content)

  print(templates)

  for section, commands in templates.items():
      print(section)
      print('\n'.join(commands))
  ```
  Результат будет аналогичным прошлому выводу.

### <a name="writejson">Запись</a> 
Для записи существует 2 метода:
+ **json.dump()** - метод записывает объект Python в файл в формате JSON. Подходит когда необходимо записать json в файл:
```python
import json

trunk_template = [
    'switchport trunk encapsulation dot1q', 'switchport mode trunk',
    'switchport trunk native vlan 999', 'switchport trunk allowed vlan'
]

access_template = [
    'switchport mode access', 'switchport access vlan',
    'switchport nonegotiate', 'spanning-tree portfast',
    'spanning-tree bpduguard enable'
]

to_json = {'trunk': trunk_template, 'access': access_template}

with open('sw_templates.json', 'w') as f:
    json.dump(to_json, f)

with open('sw_templates.json') as f:
    print(f.read())
```
+ **json.dumps()** - метод возвращает строку в формате JSON. Подходит когда нужно передать json, например, API:
```python
import json

trunk_template = [
    'switchport trunk encapsulation dot1q', 'switchport mode trunk',
    'switchport trunk native vlan 999', 'switchport trunk allowed vlan'
]

access_template = [
    'switchport mode access', 'switchport access vlan',
    'switchport nonegotiate', 'spanning-tree portfast',
    'spanning-tree bpduguard enable'
]

to_json = {'trunk': trunk_template, 'access': access_template}

with open('sw_templates.json', 'w') as f:
    f.write(json.dumps(to_json))

with open('sw_templates.json') as f:
    print(f.read())
```

**В формат JSON нельзя записать словарь, у которого ключи - кортежи**

## <a name="OOP"></a> ООП


### <a name="metaclasses"> </a> Метаклассы
+ Классы в Python это объекты. Объект способен сам создавать экземпляры, так что это класс. **Метаклассы** – это классы, экземпляры которых являются классами. Для создания нового класса вызывает метакласс.
+ Классы являются обьектами вот почему:
  + его можно назначить в качестве переменной:
    ```py
    >>> class Object(object):
    ...   pass

    >>> a = Object
    >>> a
    <class '__main__.Object'>
    >>> b = a()
    >>> b
    <__main__.Object object at 0x7fc70891f518>
    ```
  + копируется

  + есть возможность добавить к нему атрибуты
    ```py
    >>> print(hasattr(ObjectCreator, 'new_attribute'))
    False
    >>> ObjectCreator.new_attribute = 'foo' # you can add attributes to a class
    >>> print(hasattr(ObjectCreator, 'new_attribute'))
    True
    >>> print(ObjectCreator.new_attribute)
    foo
    ```
  + передается в роли параметра функции:
  ```py
  >>> def echo(o):
  ...       print(o)
  ...
  >>> echo(ObjectCreator) # you can pass a class as a parameter
  <class '__main__.ObjectCreator'>
  ```
+ Чтобы сделать метакласс необходимо его унаследовать от класса **type**:
+ `type` это встроенный метакласс, который использует Питон, но вы, конечно, можете создать свой.
+ **type()** вообще сама по всебе довольно интересная функция, она дает возможности:
  + определить тип объекта:
    ```py
    >>> print(type(1))
    <type 'int'>
    >>> print(type("1"))
    <type 'str'>
    >>> print(type(ObjectCreator))
    <type 'type'>
    >>> print(type(ObjectCreator()))
    <class '__main__.ObjectCreator'>
    ```
  + создавать классы "на ходу" в такой форме **`type(name of the class, tuple of the parent class (for inheritance, can be empty), dictionary containing attributes names and values)`**:
    + Создание пустого класса:
      ```py
      MyShinyClass = type('MyShinyClass', (), {}) # returns a class object
      ```
      Эквивалентно:
      ```py
      class MyShinyClass(object):
          pass
      ```
    + Создание класса со свойствами:
      ```py
      Foo = type('Foo', (), {'bar':True})
      ```
      Эквивалентно:
      ```py
      class Foo(object):
          bar = True
      ```
    + наследовать другие классы:
      ```py
      FooChild = type('FooChild', (Foo,), {})
      ``` 
      Эквивалентно:
      ```py
      class FooChild(Foo):
          pass
      ```
+ про **`__metaclass__`**:
  + в этом свойстве обычно указывается метакласс при помощи которого необходимо саздать класс.
  + Питон будет искать **`__metaclass__`** в определении класса. Если он его найдёт, то использует для создания класса. Если же нет, то будет использовать type.
  + Алгоритм поиска **`__metaclass__`**:
    + Если у класса есть атрибут **`__metaclass__`**, создаёт в памяти объект-класс, используя то, что указано в **`__metaclass__`**.
    + Если у класса нет атрибута **`__metaclass__`**, он ищет **`__metaclass__`** в родительском классе и попробует сделать то же самое.
    + Если же **`__metaclass__`** не находится ни в одном из родителей, Питон будет искать **`__metaclass__`** на уровне модуля.
    + И если он не может найти вообще ни одного **`__metaclass__`**, он использует type для создания объекта-класса.

### <a name="iterators"></a>
+ Итераторы — объекты, которые позволяют обходить(итерировать) коллекции:
  + Итерируемый — объект, в котором есть метод **`__iter__`**.
  + Итератор — объект, в котором есть два метода: **`__iter__`** и **`__next__`**. почти всегда iterator возвращает *себя* из метода **`__iter__`**, так как они выступают итераторами для самих себя.
  + В целом стоит избегать прямого вызова **`__iter__`** и **`__next__`**. При использовании for или генераторов списков Python вызывает эти методы сам. Можно так же использовать фанкции **`next()`** и **`iter()`**
  + Итераторы могут быть бесконечными:
  ```py
  class count_iterator:
    n = 0

    def __iter__(self):
        return self

    def __next__(self):
        y = self.n
        self.n += 1
        return y
  ```
  ```py
  >>> counter = count_iterator()
  >>> next(counter)
  0
  >>> next(counter)
  1
  >>> next(counter)
  2
  >>> next(counter)
  3
  >>> list(counter) #бесконечный список
  ```

### <a name="generators"></a> Генераторы
+ Генераторы — это функции, которые можно приостанавливать и возобновлять во время их выполнения, при этом **они возвращают объект, который можно итерировать**. Генераторы не могут возвращать значения, вместо этого **выдают элементы по готовности**. Python автоматизирует запоминание контекста генератора, то есть текущий поток управления, значение локальных переменных и так далее. Каждый вызов метода **`__next__`** у объекта генератора возвращает следующее значение. Метод **`__iter__`** также реализуется автоматически. То есть генераторы можно использовать везде, где требуются итераторы.
+ Для создание генераторов необходимо использовать кючевое слово **`yield`**:
  ```py
  def count_generator():
    n = 0
    while True:
      yield n
      n += 1
  
  >>> counter = count_generator()
  >>> counter
  <generator object count_generator at 0x106bf1aa0>
  >>> next(counter)
  0
  >>> next(counter)
  1
  >>> iter(counter)
  <generator object count_generator at 0x106bf1aa0>
  >>> iter(counter) is counter
  True
  >>> type(counter)
  <type 'generator'>
  ```

## <a name="object_model"> </a> Объектная модель
+ Всю мощь объектной модели Python можно осознать через изучение назначения и возможностей специальных методов. Это методы, которые вызываются интерпретатором для выполнения различных операций над объектами. В своем коде, программисты, как правило, напрямую не вызывают эти методы. Объектная модель – это своеобразный API, который вы можете реализовать, чтобы ваши объекты можно было использовать максимально естественно для заданной экосистемы (в нашем случае – Python).
+ Специальные методы:
  + **`__repr__()`** - строковое определение объекта
  + **`__str__()`** - строковое представление объекта, если не определено, то вместо него используется `__repr__()`
  + **`__bytes__()`** - возвращает объект типа bytes – представление текущего объекта в виде набора байт
  + **`__bool__()`** - Возвращает True или False.
  + **`__call__()`** - Позволяет использовать объект как функцию, т.е. его можно вызвать.
  + **`__len__()`** - Чаще всего реализуется в коллекциях и сходными с ними по логике работы типами, которые позволяют хранить наборы данных. Для списка (list) __len__() возвращает количество элементов в списке, для строки – количество символов в строке. Вызывается функцией len(), встроенной в язык Python.
  + Операторы сравнения. Эти функции используются, если объекты можно сравнивать между собой, как числа. Синтаксис работы с ними выглядит следующим образом: x.__lt__(y), что соответствует записи x < y:
    Функция       | Описание
    ------------- | -------------
    `__lt__()`    | <
    `__ne__()	`   | !=
    `__le__()`    | <=
    `__eq__()`    | ==
    `__ge__()`    | >=
  + Арифметические операции:
    Функция       | Описание
    ------------- | -------------
    `__add__()`   | `+`
    `__sub__()`   | `-`
    `__mul__()`   | `*`
    `__truediv__()`| `/`
    `__floordiv__()`| `//`
    `__mod__()`     | `%`
    `__pow__()`   | `**`
    Помимо указанных в таблице, в объектной модели Python, есть методы, позволяющие выполнять инверсные арифметические операции (случай, когда объекты, над которыми производится действие меняются местами), имена таких методов совпадают с перечисленными выше с добавлением символа **r** перед именем (примеры: `__radd__(), __rsub__()`). Методы для арифметических операции присваивания, таких как *=, += и т.п., имеют имена сходные с табличными с добавлением символа **i** перед именем (примеры: `__iadd__(), __isub__()`).
  ```py
  class Vector():
  
    def __init__(self, x, y):
        self.x = x
        self.y = y
  
    def __add__(self, v):
        return Vector(self.x + v.x, self.y + v.y)
  
    def __sub__(self, v):
        return Vector(self.x - v.x, self.y - v.y)
  
    def __repr__(self):
        return "Vector({}, {})".format(self.x, self.y)
  
    def __str__(self):
        return "({}, {})".format(self.x, self.y)
  
    def __abs__(self):
        return (self.x**2 + self.y**2)**0.5

  if __name__ == "__main__":
     v1 = Vector(3, 4)
     print("vector v1 = {}".format(v1))

     v2 = Vector(5, 7)
     print("vector v2 = {}".format(v2))

     print("v1 + v2 = {}".format(v1 + v2))
     print("v2 - v1 = {}".format(v2 - v1))
     print("|v1| = {}".format(abs(v1)))
  ``` 

## <a name="packeges"></a> Пакеты и модули
**Модулем** в Python называется любой файл с программой.

+ **import** - для подключения модуля:
  ```python
  import random as rn
  rn.randomint()
  ```
+ Для проверки того, выполняется ли модуль как отдельная программа, есть переменная **__name__**, если она равна **"__main__"**, то модуль выполняется как отдельная программа:
  ```python
  def prt(str):
    print(str)


  if __name__ == "__main__":
    prt("hello")
  ```

**Пакет в Python** – это каталог, включающий в себя другие каталоги и модули, но при этом дополнительно содержащий файл `__init__.py`. Пакеты используются для формирования пространства имен, что позволяет работать с модулями через указание уровня вложенности (через точку).

+ Файл `__init__.py` может быть пустым или может содержать переменную `__all__`, хранящую список модулей, который импортируется при загрузке через конструкцию, например:
  ```python
  __all__ = ["simper", "compper", "annuity"]#перечисление модулей
  ```


## <a name="threads"> </a> Потоки
+ **Поток** - инструмент, позволяющий выполнять подпрограммы одновременно. Однако в python параллельное выполнение на уровне потоков блокируется глобальным блокировщиком. Он гарантирует, что в одно время выполняется не более 1 потока. Это увеличивает безопасность приложения. Многопоточное приложение не запускает сразу несколько процессов, это делает многопроцессное приложение.
+ В python за многопоточность ответственнен модуль `threading`, основной класс в этой библиотеке это `Thread`:
  ```python
  import threading
  import time

  def T1():
      while True:
          time.sleep(1)
          print("This is thread 1")

  def T2():
      while True:
          time.sleep(1)
          print("This is thread 2")


  t1 = threading.Thread(target=T1, daemon=True)
  t2 = threading.Thread(target=T2, daemon=True)

  t1.start()
  t2.start()

  while True:
      time.sleep(1)
      print("This is main thread")

  # output:
  # This is thread 2
  # This is main thread
  # This is thread 1
  # This is main thread
  # This is thread 2
  # This is thread 1
  # This is main thread
  # This is thread 2
  # This is thread 1
  # This is main thread
  # This is thread 2
  # This is thread 1
  ```

## Сокеты
+ Сокеты встроены в Python в виде стандартной библиотеки **`socket`**. Первичные функци и методы этого модуля следующие:
  + socket();
  + bind();
  + listen();
  + accept();
  + connect();
  + connect_ex();
  + send();
  + recv();
  + close();
+ Создание простого echo-сервера;
  ```python
  import socket

  HOST = '127.0.0.1'  # Standard loopback interface address   (localhost)
  PORT = 65432        # Port to listen on (non-privileged ports are >   1023)

  with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
      s.bind((HOST, PORT))
      s.listen()
      conn, addr = s.accept()
      with conn:
          print('Connected by', addr)
          while True:
              data = conn.recv(1024)
              if not data:
                  break
              conn.sendall(data)
  ```


## <a name="dop"></a> Дополнение
Здесь перечисленны все ключевые слова для Python, а также остальная, часто встречающаяся, справочная информация.

### <a name="keywords"></a> Ключевые слова
+ Список ключевых слов можно посмотреть при помощи модуля  **keyword**.

+ **keyword.kwlist** - список всех ключевых слов

+ **keyword.iskeyword("str")** - является ли строка ключевым словом.

Команда       | Описание
------------- | -------------
**False**     | Ложь         
**True**      | Правда
**None**      | Пустой объект
**and**       | Логическое "и"
**with/ as**  | Используется для оборачивания выполнения блока инструкций менеджером контекста. Иногда это более удобная конструкция, чем try...except...finally
**assert**    | Возбуждает исключение, если условие ложно
**break**     | Выход из цикла
**class**     | Создает пользовательский тип данных
**continue**  | Переход на следующую итерацию цикла
**def**       | Определение функции
**del**       | Удаление объекта
**elif**      | Иначе если
**except**    | Перехват исключений
**finally**   | Вкупе с инструкцией try, выполняет инструкции независимо от того, было ли исключение или нет
**for**       | Цикл for
**from**      | Импорт нескольких функций из модуля
**global**    | Позволяет присвоить значение глобальной переменной внутри функции
**if**        | Условный оператор
**import**    | Импорт модулей 
**in**        | Проверка на вхождение
**is**        | Ссылаются ли 2 объекта на одно место в памяти
**lambda**    | Определение анонимной функции
**nonlocal**  | Позволяет сделать значение переменной, присвоенное ей внутри функции, доступным в объемлющей инструкции
**not**       | Логическое "нет"
**or**        | Логическое "или"
**pass**      | Ничего не делающая конструкция
**raise**     | Возбудить исключение
**return**    | Вернуть результат
**try**       | Выполнить конструкции, перехватывающие исключения
**while**     | Цикл while
**yield**     | Определение функции-генератора
