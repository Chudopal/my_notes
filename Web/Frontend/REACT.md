# React
**JavaScript библеотека для создания фронтэнда**

+ [Компонент](#component)
+ [JSX](#jsx)

### <a name="component"> </a> Компонент
+ Любое приложение на React состоит из множества компонент.
+ Компоненты позволяют разбить приложение на независимые части, а также позже использовать эти **компоненты повторно в коде**. Компоненты могут состоять из других компонент.
+ Примеры компонент: `пост, комментарий, лайк и т д`
+ Создание компоненты:
    ```javascript
        import React from "react"
        import {render} from 'react-dom'

        function HelloWorld(){
            return (
                <h1>Hello world</h1>
            )
        }

        render(<hello_world/>, document.getElementById('root'))//рендерим функцию и указываем, куда поместить отрендеренный код
    
    ```

### <a name="jsx"> </a> JSX
+ **JSX** очень похож на HTML, однако данный код компилируется в JS.
    ```javascript
        function HelloWorld(){
            return (
                <h1>Hello world</h1>
            )
        }
    ```
+ В JSX есть некоторые особенности, которые отличают его от HTML:
    + Вся компонента должна быть представленна одним тегом:
        ```javascript
        function HelloWorld(){
            return (
                <h1> Hello world </h1>
                <h2> This message is error </h2> //это не скомлируется так как представленно двумя тегами
            )
        }
        ```
        Чтобы это заработало можно вернуть данную разметку в виде листа:
        ```javascript
        function HelloWorld(){
            return [ //заменили круглую скобку на квадратную
                <h1> Hello world </h1>
                <h2> This message is error </h2>
            ]
        }
        ```
        Или просто взять все в один общий тег:
        ```javascript
        function HelloWorld(){
            return (
                <div> //добавили обрамляюший тег
                    <h1> Hello world </h1>
                    <h2> This message is error </h2>
                </div>
            )
        }
        ```
    + Одни компоненты можно интегрировать в другие в виде тега. Однако JSX **требует закрытия каждого тега**, а так же **написание компонент с большой буквы**:
    ```javascript
    function Hello(){ //определили компоненту
        return(
            <div>
                <h2> Hello </h2>
                <p> Nice to meet you!!! </p>
            </div>
        )
    }

    function App(){ //эта компонента будет использовать компоненту выше
        return (
            <div>
                <h1> This app is created by REACT.js </h1>
                <Hello></Hello> //теперь разметка из компоненты  Hello будет подставленна сюда. 
            </div>
        )
    }

    function App1(){ //эта компонента будет использовать компоненту выше
        return (
            <div>
                <h1> This app is created by REACT.js </h1>
                <Hello /> //более короткий способ записать открытие и закрытие тега 
            </div>
        )
    }
    ```
    + Использование JS переменных. Для интегрирования переменных в код JSX используются **фигурные скобки** **`{}`**:
    ```javascript
    function App(){
        name = "Bob"
        return (
            <div>
                <h1> Hello {name} </h1>
            </div>
        )
    }
    ```
    + Добваление параметров в теги. Для указания класса используется ключеваое слово `className`, параметры `style` берутся в двойные фигурные скобки:
    ```javascript
    function App(){
        return (
            <div className="hello" style={{ colot: red; }}>
                <h1> Hello </h1>
            </div>
        )
    }
    ```