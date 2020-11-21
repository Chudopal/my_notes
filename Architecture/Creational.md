# Порождающие паттерны.
**Паттерны, которые сздают новые объекты**

+ [Фабричный метод](#fabric)
+ [Абстрактная фабрика](#abstract_fabric)
+ [Строитель](#builder)
+ [Прототип](#prototype)
+ [Одиночка](#singleton)

### <a name="fabric"> </a> Фабричный метод
+ Порождающий шаблон проектирования, предоставляющий подклассам **интерфейс для создания экземпляров некоторого класса**. То есть, по сути, создание объекта просто переносится внутрь класса для большей гибкости. В интерфайсе или абстрактном классе определяется метод, а реализуется он в подклассах посредством тех сущностей, которые необходимы в данной ситуации.
+ ПЛЮСЫ:
    + Упрощает добавление новых продуктов в программу.
    + Создание объектов, независимо от их типов и сложности процесса создания.
+ МИНУСЫ:
    + Даже для одного объекта необходимо создать соответствующую фабрику, что увеличивает код.
    + Может привести к созданию больших параллельных иерархий классов, так как для каждого класса продукта надо создать свой подкласс создателя.
+ КОГДА ПРИМЕНЯТЬ:
    + классу заранее неизвестно, **объекты каких подклассов ему нужно создавать**.
    + класс спроектирован так, чтобы объекты, которые он создаёт, **специфицировались подклассами**.
    + вы хотите дать возможность пользователям **расширять части вашего фреймворка или библиотеки**.
    + класс делегирует свои обязанности одному из нескольких вспомогательных подклассов, и планируется локализовать знание о том, какой класс принимает эти обязанности на себя.
+ РЕАЛИЗАЦИЯ:
    ```py
    import abc
    
    
    class Creator(metaclass=abc.ABCMeta):
        """
        Declare the factory method, which returns an object of type Product.
        Creator may also define a default implementation of the factory
        method that returns a default ConcreteProduct object.
        Call the factory method to create a Product object.
        """
    
        def __init__(self):
            self.product = self._factory_method()
    
        @abc.abstractmethod
        def _factory_method(self):
            pass
    
        def some_operation(self):
            self.product.interface()
    
    
    class ConcreteCreator1(Creator):
        """
        Override the factory method to return an instance of a
        ConcreteProduct1.
        """
    
        def _factory_method(self):
            return ConcreteProduct1()
    
    
    class ConcreteCreator2(Creator):
        """
        Override the factory method to return an instance of a
        ConcreteProduct2.
        """
    
        def _factory_method(self):
            return ConcreteProduct2()
    
    
    class Product(metaclass=abc.ABCMeta):
        """
        Define the interface of objects the factory method creates.
        """
    
        @abc.abstractmethod
        def interface(self):
            pass
    
    
    class ConcreteProduct1(Product):
        """
        Implement the Product interface.
        """
    
        def interface(self):
            pass
    
    
    class ConcreteProduct2(Product):
        """
        Implement the Product interface.
        """
    
        def interface(self):
            pass
    
    
    def main():
        concrete_creator = ConcreteCreator1()
        concrete_creator.product.interface()
        concrete_creator.some_operation()
    
    
    if __name__ == "__main__":
        main()

    ```

### <a name="abstract_fabric"> </a> Абстрактная фабрика
+ это порождающий паттерн проектирования, который позволяет создавать объекты одного типа, объединяя их в общий интерфейс, в котором для создания каждого объекта имеется метод, типа создатьКресло, создатьСтол и т.д. **Абстрактная фабрика специализируется на создании семейств связанных продуктов.**
+ ПЛЮСЫ:
    + Гарантирует сочетаемость создаваемых продуктов.
    + Избавляет клиентский код от привязки к конкретным классам продуктов.
    + Упрощает добавление новых продуктов в программу.
+ МИНУСЫ:
    + Упрощает добавление новых продуктов в программу.
    + Усложняет код программы из-за введения множества дополнительных классов.
    + Требует наличия всех типов продуктов в каждой вариации.
+ КОГДА ПРИМЕНЯТЬ:
    + Когда бизнес-логика программы должна работать с разными видами связанных друг с другом продуктов, не завися от конкретных классов продуктов.
+ РЕАЛИЗАЦИЯ:
    ```py
    from abc import ABCMeta, abstractmethod


    class Beer(metaclass=ABCMeta):
    	pass


    class Snack(metaclass=ABCMeta):

    	@abstractmethod
    	def interact(self, beer: Beer) -> None:
    		pass


    class AbstractShop(metaclass=ABCMeta):

    	@abstractmethod
    	def buy_beer(self) -> Beer:
    		pass

    	@abstractmethod
    	def buy_snack(self) -> Snack:
    		pass


    class Tuborg(Beer):
    	pass


    class Staropramen(Beer):
    	pass


    class Peanuts(Snack):

    	def interact(self, beer: Beer) -> None:
    		print('Мы выпили по бутылке пива {} и закусили его арахисом'.format(
    			beer.__class__.__name__))


    class Chips(Snack):

    	def interact(self, beer: Beer) -> None:
    		print('Мы выпили несколько банок пива {} и съели пачку чипсов'.format(
    			beer.__class__.__name__))


    class ExpensiveShop(AbstractShop):

    	def buy_beer(self) -> Beer:
    		return Tuborg()

    	def buy_snack(self) -> Snack:
    		return Peanuts()


    class CheapShop(AbstractShop):

    	def buy_beer(self) -> Beer:
    		return Staropramen()

    	def buy_snack(self) -> Snack:
    		return Chips()


    if __name__ == '__main__':
    	expensive_shop = ExpensiveShop()
    	cheap_shop = CheapShop()
    	print('OUTPUT:')
    	beer = expensive_shop.buy_beer()
    	snack = cheap_shop.buy_snack()
    	snack.interact(beer)
    	beer = cheap_shop.buy_beer()
    	snack = expensive_shop.buy_snack()
    	snack.interact(beer)

    '''
    OUTPUT:
    Мы выпили несколько банок пива Tuborg и съели пачку чипсов
    Мы выпили по бутылке пива Staropramen и закусили его арахисом
    '''
    ```

### <a name="builder"></a> Строитель
+ предоставляет методы, которые позволяют создавать сложный объект пошагово. Если выеделить методы из стрителя в отедельный класс, то этот класс будет называться **директором**. Медоты строителя не обязательно вызывать через директора, однако
+ ПЛЮСЫ:
    + Позволяет создавать продукты пошагово
    + избавляется от телескопического конструктора
    + более тонкий контроль над процессом конструирования, чем другие порождающие паттерны
+ МИНУСЫ:
    + Усложняет код программы из-за введения дополнительных классов.
+ КОГДА ПРИМЕНЯТЬ:
    + Когда вам необходимо собрать сложные, составные объекты.
+ РЕАЛИЗАЦИЯ:
    ```py
    from abc import ABC, abstractmethod, abstractproperty
    from typing import Any


    class Builder(ABC):

        @abstractproperty
        def product(self) -> None:
            pass

        @abstractmethod
        def produce_part_a(self) -> None:
            pass

        @abstractmethod
        def produce_part_b(self) -> None:
            pass

        @abstractmethod
        def produce_part_c(self) -> None:
            pass


    class Product1():

        def __init__(self) -> None:
            self.parts = []

        def add(self, part: Any) -> None:
            self.parts.append(part)

        def list_parts(self) -> None:
            print(f"Product parts: {', '.join(self.parts)}", end="")


    class ConcreteBuilder1(Builder):

        def __init__(self) -> None:
            self.reset()

        def reset(self) -> None:
            self._product = Product1()

        @property
        def product(self) -> Product1:
            product = self._product
            self.reset()
            return product

        def produce_part_a(self) -> None:
            self._product.add("PartA1")

        def produce_part_b(self) -> None:
            self._product.add("PartB1")

        def produce_part_c(self) -> None:
            self._product.add("PartC1")


    class Director:

        def __init__(self) -> None:
            self._builder = None

        @property
        def builder(self) -> Builder:
            return self._builder

        @builder.setter
        def builder(self, builder: Builder) -> None:
            self._builder = builder

        def build_minimal_viable_product(self) -> None:
            self.builder.produce_part_a()

        def build_full_featured_product(self) -> None:
            self.builder.produce_part_a()
            self.builder.produce_part_b()
            self.builder.produce_part_c()
    ```

### <a name="prototype"> </a> Прототип
+ порождающий паттерн проектирования, который позволяет копировать объекты, не вдаваясь в подробности их реализации.
+ ПЛЮСЫ:
    + Ускоряет создание объектов.
+ МИНУСЫ: нет
+ РЕАЛИЗАЦИЯ:
    ```py
    import copy

    class Prototype:

        def __init__(self):
            self._objects = {}

        def register_object(self, name, obj):
            """Register an object"""
            self._objects[name] = obj

        def unregister_object(self, name):
            """Unregister an object"""
            del self._objects[name]

        def clone(self, name, **attr):
            """Clone a registered object and update inner attributes dictionary"""
            obj = copy.deepcopy(self._objects.get(name))
            obj.__dict__.update(attr)
            return obj

    class A:
        def __init__(self):
            self.x = 3
            self.y = 8
            self.z = 15
            self.garbage = [38, 11, 19]

        def __str__(self):
            return '{} {} {} {}'.format(self.x, self.y, self.z, self.garbage)

    def main():
        a = A()
        prototype = Prototype()
        prototype.register_object('objecta', a)
        b = prototype.clone('objecta')
        c = prototype.clone('objecta', x=1, y=2, garbage=[88, 1])
        print([str(i) for i in (a, b, c)])

    if __name__ == '__main__':
        main()
    ```

### <a name="singleton"></a> Одиночка
+ паттерн проектирования, который гарантирует, что у класса будет только один объект с глобальным доступом в однопоточном приложении
+ ПЛЮСЫ:
    + Контролирует доступ к единственному экземпляру
+ МИНУСЫ:
    + Нарушает принцип идинственной ответственности класса
    + Проблема мультипоточности
    + глобальные объекты могут быть вредны для объектного программирования, в некоторых случаях приводя к созданию немасштабируемого проекта.
+ КОГДА ПРИМЕНЯТЬ:
    + Когда в программе должен быть единственный экземпляр какого-то класса, доступный всем клиентам (например, общий доступ к базе данных из разных частей программы).
+ РЕАЛИЗАЦИЯ:
    + на декраторах: 
        ```py
        def singleton(cls):
            instances = {}
            def getinstance():
                if cls not in instances:
                    instances[cls] = cls()
                return instances[cls]
            return getinstance

        @singleton
        class Singleton:
            ...
        ```
    + метаклассы:
    ```py
    class MetaSingleton(type):
        _instances = {}

        def __call__(cls, *args, **kwargs):
            if cls not in cls._instances:
                cls._instances[cls] = super(MetaSingleton, cls).__call__(*args, **kwargs)
            return cls._instances[cls]

    class Singleton(metaclass=MetaSingleton):
    ```
    + Через `__new__()`
    ```py
    class Singleton(object):
        def __new__(cls):
            if not hasattr(cls, 'instance'):
                cls.instance = super(Singleton, cls).__new__(cls)
            return cls.instance

    s = Singleton()
    print("Object created", s)
    s1 = Singleton()
    print("Object created", s1)
    ```