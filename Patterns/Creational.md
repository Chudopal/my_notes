# Порождающие паттерны.
**Паттерны, которые сздают новые объекты**

+ [Фабричный метод](#fabric)

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