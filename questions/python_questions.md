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


## Как связаны с GIL

# Многопроцессорность

## Что это

## Когда использовать

## Как реализовано в Питоне

# Многопоточность

## Что это

## Когда использовать

## Как реализовано в Питоне

# Асинхронность

## Что это

## Когда использовать

## Как реализовано в Питоне

# MRO в Питоне
- **порядок разрешения методов** (MRO) — это способ, с помощью которого составляется линеаризация класса.
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