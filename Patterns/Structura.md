# Структурные паттерны
**Отвечают за иерархии классов**

+ [Адаптер(Wrapper)](#wrapper)

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