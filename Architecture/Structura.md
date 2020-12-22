# Структурные паттерны
**Отвечают за иерархии классов**

+ [Адаптер(Wrapper)](#wrapper)
+ [Мост](#bridge)
+ [Компоновщик](#composite)
+ [Декоратор](#decorator)
+ [Фасад](#fasade)
+ [Легковес(приспособленец)](#flyweight)

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
+ Разделяет абстракцию и реализацию так, чтобы они могли **изменяться независимо**.
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
### <a name="fasade"></a> Фасад
+ структурный шаблон проектирования, позволяющий скрыть сложность системы путём сведения всех возможных внешних вызовов к одному объекту, делегирующему их соответствующим объектам системы.
+ ПЛЮСЫ:
    + Изолирует клиентов от компонентов сложной подсистемы.
+ МИНУСЫ:
    + Обилие крошечных классов.
    + Трудно конфигурировать многократно обёрнутые объекты.
    + Есть вероятность написать божественный класс
+ КОГДА ПРИМЕНЯТЬ:
    + Когда вам нужно представить простой или урезанный интерфейс к сложной подсистеме.
+ РЕАЛИЗАЦИЯ:
    ```py
    # Сложные части системы
    class CPU(object):
        def __init__(self):
            # ...
            pass

        def freeze(self):
            # ...
            pass

        def jump(self, address):
            # ...
            pass

        def execute(self):
            # ...
            pass

    class Memory(object):
        def __init__(self):
            # ...
            pass

        def load(self, position, data):
            # ...
            pass

    class HardDrive(object):
        def __init__(self):
            # ...
            pass

        def read(self, lba, size):
            # ...
            pass

    # Фасад
    class Computer(object):
        def __init__(self):
            self._cpu = CPU()
            self._memory = Memory()
            self._hardDrive = HardDrive()

        def startComputer(self):
            self._cpu.freeze()
            self._memory.load(BOOT_ADDRESS, self._hardDrive.read(BOOT_SECTOR, SECTOR_SIZE))
            self._cpu.jump(BOOT_ADDRESS)
            self._cpu.execute()

    # Клиентская часть
    if __name__ == "__main__":
        facade = Computer()
        facade.startComputer()
    ```
### <a name="flyweight"></a> Легковес(приспособленец)
+ структурный паттерн проектирования, который позволяет вместить бóльшее количество объектов в отведённую оперативную память. Легковес экономит память, разделяя общее состояние объектов между собой, вместо хранения одинаковых данных в каждом объекте.
    + Неизменяемые данные объекта принято называть «внутренним **состоянием»**. Все остальные данные — это **«внешнее состояние»**.
+ ПЛЮСЫ:
    + Экономит оперативную память.
+ МИНУСЫ:
    + Расходует процессорное время на поиск/вычисление контекста.
    + Усложняет код программы из-за введения множества дополнительных классов.
+ КОГДА ПРИМЕНЯТЬ:
    + Когда не хватает оперативной памяти для поддержки всех нужных объектов.
+ РЕАЛИЗАЦИЯ:
    ```py
    class Lamp(object):

        def __init__(self, color):
            self.color = color


    class LampFactory:

        lamps = {}

        @staticmethod #можно и через @classmethod
        def get_lamp(color):
            return LampFactory.lamps.setdefault(color, Lamp(color))#добавляет новый объект, если если не находит существующий


    class TreeBranch(object):
        def __init__(self, branch_number):
            self.branch_number = branch_number

        def hang(self, lamp):
            print("Hang {} [{}] lamp on branch {} [{}]".format(lamp.color, id(lamp), self.branch_number, id(self)))


    class ChristmasTree(object):
        def __init__(self):
            self.lamps_hung = 0
            self.branches = {}

        def get_branch(self, number):
            return self.branches.setdefault(number, TreeBranch(number))

        def dress_up_the_tree(self):
            self.hang_lamp('red', 1)
            self.hang_lamp('blue', 1)
            self.hang_lamp('yellow', 1)
            self.hang_lamp('red', 2)
            self.hang_lamp('blue', 2)
            self.hang_lamp('yellow', 2)
            self.hang_lamp('red', 3)
            self.hang_lamp('blue', 3)
            self.hang_lamp('yellow', 3)
            self.hang_lamp('red', 4)
            self.hang_lamp('blue', 4)
            self.hang_lamp('yellow', 4)
            self.hang_lamp('red', 5)
            self.hang_lamp('blue', 5)
            self.hang_lamp('yellow', 5)
            self.hang_lamp('red', 6)
            self.hang_lamp('blue', 6)
            self.hang_lamp('yellow', 6)
            self.hang_lamp('red', 7)
            self.hang_lamp('blue', 7)
            self.hang_lamp('yellow', 7)

        def hang_lamp(self, color, branch_number):
            self.get_branch(branch_number).hang(LampFactory.get_lamp(color))
            self.lamps_hung += 1


    if __name__ == '__main__':
        ChristmasTree().dress_up_the_tree()
    ```
    Еще один вариант с метаклассом:
    ```py
    import weakref


    class FlyweightMeta(type):
    
        def __new__(mcs, name, parents, dct):
            """
            Set up object pool
            :param name: class name
            :param parents: class parents
            :param dct: dict: includes class attributes, class methods,
            static methods, etc
            :return: new class
            """
            dct["pool"] = weakref.WeakValueDictionary()
            return super().__new__(mcs, name, parents, dct)

        @staticmethod
        def _serialize_params(cls, *args, **kwargs):
            """
            Serialize input parameters to a key.
            Simple implementation is just to serialize it as a string
            """
            args_list = list(map(str, args))
            args_list.extend([str(kwargs), cls.__name__])
            key = "".join(args_list)
            return key

        def __call__(cls, *args, **kwargs):
            key = FlyweightMeta._serialize_params(cls, *args, **kwargs)
            pool = getattr(cls, "pool", {})

            instance = pool.get(key)
            if instance is None:
                instance = super().__call__(*args, **kwargs)
                pool[key] = instance
            return instance


    class Card2(metaclass=FlyweightMeta):

        def __init__(self, *args, **kwargs):
            print('Init {}: {}'.format(self.__class__, (args, kwargs)))
            pass


    if __name__ == "__main__":
        instances_pool = getattr(Card2, "pool")
        cm1 = Card2("10", "h", a=1)
        cm2 = Card2("10", "h", a=1)
        cm3 = Card2("10", "h", a=2)

        assert (cm1 == cm2) and (cm1 != cm3)
        assert (cm1 is cm2) and (cm1 is not cm3)
        assert len(instances_pool) == 2

        del cm1
        assert len(instances_pool) == 2

        del cm2
        assert len(instances_pool) == 1

        del cm3
        assert len(instances_pool) == 0
    ```
