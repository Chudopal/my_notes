# CSS - Cascading Style Sheets.
позволяет отделить стильл от контента
+ [Основы SCC. Применение.](#sccforhtml)
+ [CSS в html](#cssinhtml)


### <a name="sccforhtml"></a> Основы SCC. Применение.
CSS состоит из правил стилей, которые браузер интерпретирует, а затем применяет к соответствующим элементам в документе.
Правило стиля состоит из трех частей: **селектор, свойство и значение**.
Объявление CSS всегда заканчивается точкой с запятой, а группы объявлений заключаются в фигурные скобки.
Селектор указывает на элемент, к которому применять:
```CSS
    h1 { color: orange; }
```
    Код выше сделает все загловки с тегом `<h1>` оранжевыми.
+ **Селекторы типов**:
    Выбирают все объекты данного типа и применяют к ним стили. Чтобы выбрать по типу, перед селектором **не надо ничего ставить**:
    ```CSS
    p {
       color: red;
       font-size:130%;
    }
    ```
        Данный пример сделает весь текст в параграфах красным и увеличит шрифт в 130%.
+ **Селекторы классов**:
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
+ **Селекторы по ID**:
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
```HTML
    <div id="intro">
        <p class="first">This is a <em> paragraph.</em></p>
        <p> This is the second paragraph. </p>
    </div>
    <p class="first"> This is not in the intro section.</p>
    <p> The second paragraph is not in the intro section. </p>
```

Потомки селекторов - это селекторы, которые надодятся внутри других селекторов. Можно обращатся к потомкам селекторов. Можно выбрать столько уровней, сколько надо
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

### <a name="cssinhtml"></a> CSS в html
+ **Встроенный**:
    Определяетя внутри тега и только к нему и применяется. Чтобы использовать его, необходимо использовать атрибут `style=""` внутри нужного тега:
    ```html
    <p style="color:white; background-color:gray;">
        This is an example of inline styling. 
    </p>
    ```
+ **Внутренний**:
    Опеределяеются внутри элемента `<style>` внутри раздела `head` HTML-страницы:
    ```html
    <html>
       <head>
          <style>
          p {
             color:white;
             background-color:gray;
          }
          </style>
       </head>
       <body>
          <p>This is my first paragraph. </p>
          <p>This is my second paragraph. </p>
       </body>
    </html>
    ```   
+ **Внешний**:
    Отделяется от html в отдельный файл с расширением .css. Затем на этот CSS-файл ссылаются в HTML с помощью тега `<link>`. Элемент `<link>` находится внутри раздела head:
    HTML:
    ```html
    <head>
       <link rel="stylesheet" href="example.css">
    </head>
    <body>
       <p>This is my first paragraph.</p>
       <p>This is my second paragraph. </p>
       <p>This is my third paragraph. </p>
    </body>
    ```
    CSS:
    ```css
    p {
       color:white;
       background-color:gray;
    }
    ```
