# Структурные паттерны
**Отвечают за иерархии классов**

+ [Адаптер(Wrapper)](#wrapper)
+ [Мост](#bridge)

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