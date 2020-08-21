# Структурные паттерны
**Отвечают за иерархии классов**

+ [Адаптер(Wrapper)](#wrapper)
+ [Мост](#bridge)
+ [Компоновщик](#composite)
+ [Декоратор](#decorator)

### <a name="wrapper></a> Адаптер 
+ Cтруктурный паттерн проектирования, который позволяет объектам с несовместимыми интерфейсами работать вместе.
+ ПЛЮСЫ:
    + система становится независимой от интерфейса внешних классов;
+ МИНУСЫ:
    + усложняет код, введением новых классов
+ КОГДА ПРИМЕНЯТЬ:
    + Когда вы хотите использовать сторонний класс, но его интерфейс не соответствует остальному коду приложения.
+ РЕАЛИЗАЦИЯ:
    + наследование:
        ```py
        class Target:

            def request(self) -> str:
                return "Target: The default target's behavior."


        class Adaptee:

            def specific_request(self) -> str:
                return ".eetpadA eht fo roivaheb laicepS"


        class Adapter(Target, Adaptee):

            def request(self) -> str:
                return f"Adapter: (TRANSLATED) {self.specific_request()[::-1]}"


        def client_code(target: "Target") -> None:
            """
            Клиентский код поддерживает все классы, использующие интерфейс Target.
            """

            print(target.request(), end="")


        if __name__ == "__main__":
            print("Client: I can work just fine with the Target objects:")
            target = Target()
            client_code(target)
            print("\n")

            adaptee = Adaptee()
            print("Client: The Adaptee class has a weird interface. "
                  "See, I don't understand it:")
            print(f"Adaptee: {adaptee.specific_request()}", end="\n\n")

            print("Client: But I can work with it via the Adapter:")
            adapter = Adapter()
            client_code(adapter)
        ```
    + композиция:
        ```py
        class Target:

            def request(self) -> str:
                return "Target: The default target's behavior."


        class Adaptee:
        
            def specific_request(self) -> str:
                return ".eetpadA eht fo roivaheb laicepS"


        class Adapter(Target):
        
            def __init__(self, adaptee: Adaptee) -> None:
                self.adaptee = adaptee

            def request(self) -> str:
                return f"Adapter: (TRANSLATED) {self.adaptee.specific_request()[::-1]}"


        def client_code(target: Target) -> None:
        
            print(target.request(), end="")


        if __name__ == "__main__":
            print("Client: I can work just fine with the Target objects:")
            target = Target()
            client_code(target)
            print("\n")

            adaptee = Adaptee()
            print("Client: The Adaptee class has a weird interface. "
                  "See, I don't understand it:")
            print(f"Adaptee: {adaptee.specific_request()}", end="\n\n")

            print("Client: But I can work with it via the Adapter:")
            adapter = Adapter(adaptee)
            client_code(adapter)
        ```

### <a name="bridge"> </a> Мост
+ Разделяет абстракцию и реализацию так, чтобы они могли изменяться независимо».
+ ПЛЮСЫ:
    + Скрывает лишние или опасные детали реализации от клиентского кода.
+ МИНУСЫ:
    + усложняет код программы
+ КОГДА ИСПОЛЬЗОВАТЬ:
    + Когда класс нужно расширять в двух независимых плоскостях.
    + Когда приходится делать кросс-платформенные приложения, поддерживать несколько типов баз данных или работать с разными поставщиками похожего API 
+ РЕАЛИЗАЦИЯ:
    ```py
        # ConcreteImplementor 1/2
        class DrawingAPI1:
            def draw_circle(self, x, y, radius):
                print("API1.circle at {}:{} radius {}".format(x, y, radius))


        # ConcreteImplementor 2/2
        class DrawingAPI2:
            def draw_circle(self, x, y, radius):
                print("API2.circle at {}:{} radius {}".format(x, y, radius))


        # Refined Abstraction
        class CircleShape:
            def __init__(self, x, y, radius, drawing_api):
                self._x = x
                self._y = y
                self._radius = radius
                self._drawing_api = drawing_api

            # low-level i.e. Implementation specific
            def draw(self):
                self._drawing_api.draw_circle(self._x, self._y, self._radius)

            # high-level i.e. Abstraction specific
            def scale(self, pct):
                self._radius *= pct


        def main():
            """
            >>> shapes = (CircleShape(1, 2, 3, DrawingAPI1()), CircleShape(5, 7, 11, DrawingAPI2()))
            >>> for shape in shapes:
            ...    shape.scale(2.5)
            ...    shape.draw()
            API1.circle at 1:2 radius 7.5
            API2.circle at 5:7 radius 27.5
            """


        if __name__ == "__main__":
            import doctest

            doctest.testmod()
    ```

    еще один пример:
    ```py
    from __future__ import annotations
    from abc import ABC, abstractmethod


    class Abstraction:

        def __init__(self, implementation: Implementation) -> None:
            self.implementation = implementation

        def operation(self) -> str:
            return (f"Abstraction: Base operation with:\n"
                    f"{self.implementation.operation_implementation()}")


    class ExtendedAbstraction(Abstraction):

        def operation(self) -> str:
            return (f"ExtendedAbstraction: Extended operation with:\n"
                    f"{self.implementation.operation_implementation()}")


    class Implementation(ABC):
        
        @abstractmethod
        def operation_implementation(self) -> str:
            pass


    class ConcreteImplementationA(Implementation):
        def operation_implementation(self) -> str:
            return "ConcreteImplementationA: Here's the result on the platform A."


    class ConcreteImplementationB(Implementation):
        def operation_implementation(self) -> str:
            return "ConcreteImplementationB: Here's the result on the platform B."


    def client_code(abstraction: Abstraction) -> None:

        # ...

        print(abstraction.operation(), end="")

        # ...


    if __name__ == "__main__":


        implementation = ConcreteImplementationA()
        abstraction = Abstraction(implementation)
        client_code(abstraction)

        print("\n")

        implementation = ConcreteImplementationB()
        abstraction = ExtendedAbstraction(implementation)
        client_code(abstraction)
    ```
### <a name="composite"></a> Компоновщик
+ Структурный шаблон проектирования, объединяющий объекты в древовидную структуру для представления иерархии от частного к целому. Компоновщик позволяет клиентам обращаться к отдельным объектам и к группам объектов одинаково.
+ ПЛЮСЫ:
    + Упрощает архитектуру клиента при работе со сложным деревом компонентов.
+ МИНУСЫ:
    + Создаёт слишком общий дизайн классов.
+ КОГДА ПРИМЕНЯТЬ:
    + Тогда, когда основная модель программы может быть структурирована в виде дерева.
+ РЕАЛИЗАЦИЯ:
    ```py
    from abc import ABCMeta, abstractmethod


    class Unit(metaclass=ABCMeta):
        """
        Абстрактный компонент, в данном случае это - отряд (отряд может
        состоять из одного солдата или более)
        """

        @abstractmethod
        def print(self) -> None:
            """
            Вывод данных о компоненте
            """
            pass


    class Archer(Unit):
        """
        Лучник
        """

        def print(self) -> None:
            print('лучник', end=' ')


    class Knight(Unit):
        """
        Рыцарь
        """

        def print(self) -> None:
            print('рыцарь', end=' ')


    class Swordsman(Unit):
        """
        Мечник
        """

        def print(self) -> None:
            print('мечник', end=' ')


    class Squad(Unit):
        """
        Компоновщик - отряд, состоящий более чем из одного человека. Также
        может включать в себя другие отряды-компоновщики.
        """

        def __init__(self):
            self._units = []

        def print(self) -> None:
            print("Отряд {} (".format(self.__hash__()), end=' ')
            for u in self._units:
                u.print()
            print(')')

        def add(self, unit: Unit) -> None:
            """
            Добавление нового отряда

            :param unit: отряд (может быть как базовым, так и компоновщиком)
            """
            self._units.append(unit)
            unit.print()
            print('присоединился к отряду {}'.format(self.__hash__()))
            print()

        def remove(self, unit: Unit) -> None:
            """
            Удаление отряда из текущего компоновщика

            :param unit: объект отряда
            """
            for u in self._units:
                if u == unit:
                    self._units.remove(u)
                    u.print()
                    print('покинул отряд {}'.format(self.__hash__()))
                    print()
                    break
            else:
                unit.print()
                print('в отряде {} не найден'.format(self.__hash__()))
                print()


    if __name__ == '__main__':
        print('OUTPUT:')
        squad = Squad()
        squad.add(Knight())
        squad.add(Knight())
        squad.add(Archer())
        swordsman = Swordsman()
        squad.add(swordsman)
        squad.remove(swordsman)
        squad.print()
        squad_big = Squad()
        squad_big.add(Swordsman())
        squad_big.add(Swordsman())
        squad_big.add(squad)
        squad_big.print()

    '''
    OUTPUT:
    рыцарь присоединился к отряду -9223363262492103834

    рыцарь присоединился к отряду -9223363262492103834

    лучник присоединился к отряду -9223363262492103834

    мечник присоединился к отряду -9223363262492103834

    мечник покинул отряд -9223363262492103834

    Отряд -9223363262492103834 ( рыцарь рыцарь лучник )
    мечник присоединился к отряду 8774362671992

    мечник присоединился к отряду 8774362671992

    Отряд -9223363262492103834 ( рыцарь рыцарь лучник )
    присоединился к отряду 8774362671992

    Отряд 8774362671992 ( мечник мечник Отряд -9223363262492103834 ( рыцарь рыцарь лучник )
    )
    '''
    ```
    Еще один пример:
    ```py
    from abc import ABC, abstractmethod
    from typing import List
    
    
    class Component(ABC):
    
        @property
        def parent(self) -> Component:
            return self._parent
    
        @parent.setter
        def parent(self, parent: Component):
            
            self._parent = parent
     
        def add(self, component: Component) -> None:
            pass
    
        def remove(self, component: Component) -> None:
            pass
    
        def is_composite(self) -> bool:
            return False
    
        @abstractmethod
        def operation(self) -> str:
            pass
    
    
    class Leaf(Component):
        
        def operation(self) -> str:
            return "Leaf"
    
    
    class Composite(Component):
    
        def __init__(self) -> None:
            self._children: List[Component] = []
    
        def add(self, component: Component) -> None:
            self._children.append(component)
            component.parent = self
    
        def remove(self, component: Component) -> None:
            self._children.remove(component)
            component.parent = None
    
        def is_composite(self) -> bool:
            return True
    
        def operation(self) -> str:
    
            results = []
            for child in self._children:
                results.append(child.operation())
            return f"Branch({'+'.join(results)})"
    
    
    def client_code(component: Component) -> None:
    
        print(f"RESULT: {component.operation()}", end="")
    
    
    def client_code2(component1: Component, component2: Component) -> None:
    
        if component1.is_composite():
            component1.add(component2)
    
        print(f"RESULT: {component1.operation()}", end="")
    
    
    if __name__ == "__main__":
        simple = Leaf()
        print("Client: I've got a simple component:")
        client_code(simple)
        print("\n")
    
        tree = Composite()
    
        branch1 = Composite()
        branch1.add(Leaf())
        branch1.add(Leaf())
    
        branch2 = Composite()
        branch2.add(Leaf())
    
        tree.add(branch1)
        tree.add(branch2)
    
        print("Client: Now I've got a composite tree:")
        client_code(tree)
        print("\n")
    
        print("Client: I don't need to check the components classes even when managing the tree:")
        client_code2(tree, simple)
    ```

### <a name="decorator"></a>  Докоратор
+ структурный паттерн проектирования, который позволяет динамически добавлять объектам новую функциональность, оборачивая их в полезные «обёртки». Декоратор предусматривает расширение функциональности объекта без определения подклассов.
+ ПЛЮСЫ:
    + Большая гибкость, чем у наследования
    + Позволяет добавлять обязанности на лету
+ МИНУСЫ:
    + Обилие крошечных классов.
    + Трудно конфигурировать многократно обёрнутые объекты.
+ КОГДА ПРИМЕНЯТЬ:
    + Когда нужно добавлять обязанности объектам на лету, незаметно для кода, который их использует.
+ РЕАЛИЗАЦИЯ:
    ```py
    from abc import ABCMeta, abstractmethod

    class IOperator(object):
        """
        Интерфейс, который должны реализовать как декоратор,
        так и оборачиваемый объект.
        """
        __metaclass__ = ABCMeta

        @abstractmethod
        def operator(self):
            pass


    class Component(IOperator):
        """Компонент программы"""
        def operator(self):
            return 10.0


    class Wrapper(IOperator):
        """Декоратор"""
        def __init__(self, obj):
            self.obj = obj

        def operator(self):
            return self.obj.operator() + 5.0


    comp = Component()
    comp = Wrapper(comp)
    print comp.operator()
    # 15.0
    ```