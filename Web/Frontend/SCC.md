# SCC - Cascading Style Sheets.
позволяет отделить стильл от контента
+ [Применение SCC к HTML](#sccforhtml)


### <a name="sccforhtml"></a> Применение SCC к HTML
CSS состоит из правил стилей, которые браузер интерпретирует, а затем применяет к соответствующим элементам в документе.
Правило стиля состоит из трех частей: **селектор, свойство и значение**.
Объявление CSS всегда заканчивается точкой с запятой, а группы объявлений заключаются в фигурные скобки.
Селектор указывает на элемент, к которому применять:
```CSS
    h1 { color: orange; }
```
    Код выше сделает все загловки с тегом `<h1>` оранжевыми.
+ Селекторы типов:
    Выбирают все объекты данного типа и применяют к ним стили. Чтобы выбрать по типу, перед селектором **не надо ничего ставить**:
    ```CSS
    p {
       color: red;
       font-size:130%;
    }
    ```
        Данный пример сделает весь текст в параграфах красным и увеличит шрифт в 130%.
+ Селекторы классов:
    Применяются к определенным классам. В классы могу быьб объеденины много элементов. Они как ID, которое можно применить сразу к нескольким объектам. Чтобы выбрать по классу, перед селектором **ставится точка**:
    HTML:
    ```HTML
    <div>
        <p class="first">This is a paragraph</p>
        <p> This is the second paragraph. </p>
    </div>
    <p class="first"> This is not in the intro section</p>
    <p> The second paragraph is not in the intro section. </p>
    ```
    CSS:
    ```CSS
    .first {font-size: 200%;}
    ```
+ Селекторы по ID:
    ID присваиваются одному элементу. Следовательно, если селектор по ID, то стили будут применены только к одному элементу на странице. Чтобы выбрать по ID, перед селектором **ставится хэш-символ**;
    HTML:
     ```HTML
    <div id="intro">
       <p> This paragraph is in the intro section.</p>
    </div>
    <p> This paragraph is not in the intro section.</p>
     ```
    CSS:
    ```CSS
    #intro {
       color: white;
       background-color: gray;
    }
    ```

Потомки селекторов - это селекторы, которые надодятся внутри других селекторов. Можно обращатся к потомкам селекторов. Можно выбрать столько уровней, сколько надо:
    HTML:
    ```HTML
    <div id="intro">
        <p class="first">This is a <em> paragraph.</em></p>
        <p> This is the second paragraph. </p>
    </div>
    <p class="first"> This is not in the intro section.</p>
    <p> The second paragraph is not in the intro section. </p>
    ```
    CSS:
    ```CSS
    #intro .first em {
       color: pink; 
       background-color: gray;
    }
    ```