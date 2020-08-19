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
        # Implementor
        class DrawingAPI:
            def drawCircle(self, x, y, radius):
                pass


        # ConcreteImplementor 1/2
        class DrawingAPI1(DrawingAPI):
            def drawCircle(self, x, y, radius):
                print "API1.circle at %f:%f radius %f" % (x, y, radius)


        # ConcreteImplementor 2/2
        class DrawingAPI2(DrawingAPI):
            def drawCircle(self, x, y, radius):
                print "API2.circle at %f:%f radius %f" % (x, y, radius)


        # Abstraction
        class Shape:
            # Low-level
            def draw(self):
                pass

            # High-level
            def resizeByPercentage(self, pct):
                pass


        # Refined Abstraction
        class CircleShape(Shape):
            def __init__(self, x, y, radius, drawingAPI):
                self.__x = x
                self.__y = y
                self.__radius = radius
                self.__drawingAPI = drawingAPI

            # low-level i.e. Implementation specific
            def draw(self):
                self.__drawingAPI.drawCircle(self.__x, self.__y, self.__radius)

            # high-level i.e. Abstraction specific
            def resizeByPercentage(self, pct):
                self.__radius *= pct


        def main():
            shapes = [
                CircleShape(1, 2, 3, DrawingAPI1()),
                CircleShape(5, 7, 11, DrawingAPI2())
            ]

            for shape in shapes:
                shape.resizeByPercentage(2.5)
                shape.draw()

        if __name__ == "__main__":
            main()
    ```