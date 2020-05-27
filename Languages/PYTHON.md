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
+ [Работа с файлами](#files)
+ [Работа JSON](#json)
  + [Чтение](#readjson)
  + [Запись](#writejson)
+ [Пакеты и модули](#packeges)
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