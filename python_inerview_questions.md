# Итераторы
Итератор — это сущность порождаемая функцией iter, с помощью которой происходит итерирование итерируемого объекта. Обеспечивается это добавлением мгалических методов `__iter__`(возвращается итерируемый объект) и `__next__`(возвращает следующий элемент в списте) в класс. Создав класс по протоколу итератора, его можно обойти с помощью for, или использовать встроенные функции `iter()` и `next()`
   
```py
    class InfiniteSquaring:
    """Класс обеспечивает бесконечное последовательное возведение в квадрат заданного числа."""
        def __init__(self, initial_number):
            # Здесь хранится промежуточное значение
            self.number_to_square = initial_number

        def __next__(self):
            # Здесь мы обновляем значение и возвращаем результат
            self.number_to_square = self.number_to_square ** 2
            return self.number_to_square

        def __iter__(self):
            """Этот метод позволяет при передаче объекта функции iter возвращать самого себя, тем самым в точности реализуя протокол итератора."""
            return self
```
https://docs.python.org/3.6/howto/functional.html
    
# Геренераторы
- Генератор — это объект, который сразу при создании не вычисляет значения всех своих элементов.
- Он хранит в памяти только последний вычисленный элемент, правило перехода к следующему и условие, при котором выполнение прерывается.
- Вычисление следующего значения происходит лишь при выполнении метода next(). Предыдущее значение при этом теряется. 

Этим генераторы отличаются от списков — те хранят в памяти все свои элементы, и удалить их можно только программно. Вычисления с помощью генераторов называются ленивыми, они экономят память.

К генератору применимы те же функции, что и к итератору. Созать его можно создать с помощью ключевого слова `yield`


- .close() — останавливает выполнение генератора;
- .throw() — генератор бросает исключение;
- .send() — позволяет отправлять значения генератору.

    ```py
    def f_gen():
        n = 1
        while True:
            yield n**2
            n += 1

    generator1 = f_gen()
    generator2 = f_gen()

    for i in generator1:
        print(i)
        if i > 10:
            generator1.close()

    for i in generator2:
        print(i)
        if i > 20:
               generator2.throw(Exception("Плохо!"))
    
    generator1.send(10) #100
    ```
https://docs.python.org/3.6/howto/functional.html

# Сборщики мусора

Стандартный интерпретатор питона (CPython) использует сразу два алгоритма, подсчет ссылок и generational garbage collector (далее GC), более известный как стандартный модуль gc из Python.

Объекты удаляются как только на них больше нет ссылок.

Каждый объект содержит в себе счетчик количества ссылок. Как только объект попадает в лист или на него указывает какая-либо переменная, то счетчик увеличивается на 1.

Для исправления циклических ссылок используется дополнительный сборщик мусора.

- C Реализация использует подсчет ссылок для отслеживания недостижимых объектов. Вместо этого он не отслеживает объекты в каждой строке выполнения, а периодически выполняет алгоритм обнаружения цикла, который ищет недоступные объекты и очищает их.
- Jython использует сборщик мусора JVM. Тоже самое относится к IronPython, который использует сборщик мусора CLR.
Источник: https://pythonim.ru/osnovy/sborka-musora-v-python 

## Как связаны с GIL
Программа на Python работает в однопоточном режиме довольно быстро. А вот когда программа запускается в многопоточном режиме и если не используется GIL, то возникают проблемы с преждевременным удалением объектов и другими проблемами связанными с управлением памяти. Это происходит из-за того, что несколько потоков одновременно могут увеличивать или уменьшать значения счётчика ссылок на объект.

Поэтому для решения этих проблем в Python(CPython) был добавлен GIL. GIL легко было реализовать и интегрировать в Python. Реализация GIL увеличил производительность однопоточных приложений, поскольку управление велось только одним блокиратором. И еще это архитектурное решение на заре развития Python привел к тому , что очень многие библиотеки написанные на Си были интегрированны для Python , которые повлияли на его популярность. Ведь разработчикам , которым важна была скорость разработки , важно было наличие готовых библиотек, а не скорость работы программ в многопоточных приложениях.

# Многопроцессорность
## Что это
Выполняются процессы на разных ядрах устройства. Имеют разделенную память и количество GIL'ов равное количеству потоков. 
## Когда использовать

- Вычисления

## Как реализовано в Питоне

- модуль multiprocessing
```py
import time
import multiprocessing


start = time.perf_counter()

def please_sleep(n):
    print("Sleeping for {} seconds".format(n))
    time.sleep(n)
    print("Done Sleeping for {} seconds".format(n))

processes = []

for i in range(1,6):
    p = multiprocessing.Process(target = please_sleep, args = [i])
    p.start()
    processes.append(p)

for p in processes:
    p.join()

finish = time.perf_counter()
print("Finished in {} seconds".format(finish-start))
```
- модуль subprocess
```py
import subprocess

subprocess.run(["ls", "-l", "/dev/null"], capture_output=True)
```

- os.fork (only Linux/Unix)
```py
# Importing os module
import os
 
# Creating child processes using fork() method
os.fork() # 2 процесса (1 - глвный, 2 - чайлд)
os.fork() # 4 процесса (1 - главный, 3 - чайлд)
 
# This will be executed by both parent & child processes
print("AskPython") # 4 раза. 1 - родительский процесс, 3 - чайлд процессы
```

# Многопоточность
## Что это
Операции выполняются в пределах одного процесса. Имеют общее пространство памяти. Имеют ограничение в виде GIL(CPython)
## Когда использовать

- I\O

## Как реализовано в Питоне

- модуль _thread (устарел)
```py

from _thread import start_new_thread

threadId = 1

def factorial(n):
  global threadId
  if n < 1:  
      print "%s: %d" % ("Thread", threadId )
      threadId += 1
      return 1
  else:
      returnNumber = n * factorial( n - 1 )  # рекусрсивынй вызов
      print(str(n) + '! = ' + str(returnNumber))
      return returnNumber

start_new_thread(factorial,(5, ))
start_new_thread(factorial,(4, ))

c = raw_input("Waiting for threads to return...\n")

```
- модуль threading
```py
import time
import threading


start = time.perf_counter()

def please_sleep(n):
    print("Sleeping for {} seconds".format(n))
    time.sleep(n)
    print("Done Sleeping for {} seconds".format(n))

threads = []

for i in range(1,5):
    t = threading.Thread(target = please_sleep, args = [i])
    t.start()
    threads.append(t)

for t in threads:
    t.join()

finish = time.perf_counter()

print("Finished in {} seconds".format(finish-start))
```

# Асинхронность

## Что это

Когда говорят о выполнении программ, то под «асинхронным выполнением» понимают такую ситуацию, когда программа не ждёт завершения некоего процесса, а продолжает работу независимо от него.
Источник: https://habr.com/ru/company/ruvds/blog/475246/

Асинхронное программирование — это потоковая обработка программного обеспечения / пользовательского пространства, где приложение, а не процессор, управляет потоками и переключением контекста. В асинхронном программировании контекст переключается только в заданных точках переключения, а не с периодичностью, определенной CPU.

Зеленые потоки (green threads) являются примитивным уровнем асинхронного программирования. Зеленый поток — это обычный поток, за исключением того, что переключения между потоками производятся в коде приложения, а не в процессоре.

Источник: https://tproger.ru/translations/asynchronous-programming-in-python/

## Когда использовать

- часто нужно в сетевых запросах, когда клиент ждет ответ сервера

## Как реализовано в Питоне

- tornado
```py
import tornado.ioloop
from tornado.httpclient import AsyncHTTPClient
urls = ['http://www.google.com', 'http://www.yandex.ru', 'http://www.python.org']

def handle_response(response):
    if response.error:
        print("Error:", response.error)
    else:
        url = response.request.url
        data = response.body
        print('{}: {} bytes: {}'.format(url, len(data), data))

http_client = AsyncHTTPClient()
for url in urls:
    http_client.fetch(url, handle_response)
    
tornado.ioloop.IOLoop.instance().start()
```

- gevent
```py

import gevent

def foo():
    print('Running in foo')
    gevent.sleep(0)
    print('Explicit context switch to foo again')

def bar():
    print('Explicit context to bar')
    gevent.sleep(0)
    print('Implicit context switch back to bar')

gevent.joinall([
    gevent.spawn(foo),
    gevent.spawn(bar),
])

# Running in foo
# Explicit context to bar
# Explicit context switch to foo again
# Implicit context switch back to bar
```
- asyncio
```py
import asyncio

async def foo():
    print('Running in foo')
    await asyncio.sleep(0)
    print('Explicit context switch to foo again')


async def bar():
    print('Explicit context to bar')
    await asyncio.sleep(0)
    print('Implicit context switch back to bar')


ioloop = asyncio.get_event_loop()
tasks = [ioloop.create_task(foo()), ioloop.create_task(bar())]
wait_tasks = asyncio.wait(tasks)
ioloop.run_until_complete(wait_tasks)
ioloop.close()
```

# MRO в Питоне
- **порядок разрешения методов** (MRO - method resolution order) — это способ, с помощью которого составляется линеаризация класса.
- **линеаризацией класса** называется список из самого класса и всех его предков (родителей и прородителей) в котором по порядку слева направо будет производиться поиск метода
## 2.0
В старых версиях Питона порядок разрешения методов был достаточно примитивным: поиск вёлся во всех родительских классах слева направо на максимальную глубину. Т.е. если у родительского класса в свою очередь не было нужного метода, но были родители, то поиск производился в них по тем же правилам

    A     C
    |     |
    B     D
     \   /
       E
При обращении к методу экземпляра класса E такой алгоритм произвёл бы поиск последовательно в классах E->B->A->D->C. Таким образом поиск вёлся сначала в первом классе-родителе и во всех его предках, затем во втором классе-родителе со всеми предками и т. д. Этот способ не вызывал особых нареканий, пока у классов не было общего предка.

Однако, начиная с версии 2.2, появился базовый класс object, от которого рекомендовалось наследовать все пользовательские классы.

     object
     /   \
    A     B
     \   /
       C

При старом виде наследования, если метод не объявлен в A, но есть в object (например `__init__`) и более специфический в B, то возьмется менее специфичный метод из object.


## 3.0
Для для решения проблемы ромбовидного насчледования выше целей в Питоне 3 используется алгоритм C3.

    class C(D, E)
    С -> D -> E -> object
    object
    |
    D
    | \
    |  E
    | /
    C


    class Music(object): pass
    class Rock(Music): pass
    class Gothic(Music): pass
    class Metal(Rock): pass
    class GothicRock(Rock, Gothic): pass
    class GothicMetal(Metal, Gothic): pass
    class The69Eyes(GothicRock, GothicMetal): pass

    The69Eyes -> GothicRock -> GothicMetal -> Metal -> Rock -> Gothic -> Music

        Music
        /      \
    Rock      Gothic ---------
      /   \        /          \
    Metal    Gothic Rock       \
      |             |           \
       \----------------- GothicMetal
                    |          /
                     The69Eyes

# Аутентификации

# Middleware

# SQL

# НФ

# Индексы
+
## Плюсы

## Минусы

# Транзакции

## В Django

# Django Queryset

## Какие команды есть

## select_related

## prefetch_related

## Q

## F

## annotate

## bulk_create

# Docker

## Composer

# Nginx

# celery

# Redis

# RabbitMq

# Кеширование

# Unittest

## Что такое mock

# wsgi/asgi

# сокеты

# Fast API
