# CSS - Cascading Style Sheets.
позволяет отделить стильл от контента
+ [Основы SCC. Применение.](#sccforhtml)
+ [CSS в html](#cssinhtml)
+ [Шрифты](#fonts)

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
### <a name="fonts"></a> Шрифты
+ Свойство **`font-family`** *определяет шрифт* для элемента. Разделяя шрифты запятыми, можно   указать какой шрифт использовать в первую очередь, а какой (при отсуствии первого в системе)  испльзовать в качестве замены:
    HTML:
    ```html
    <p class="serif">
       This is a paragraph shown in serif font.
    </p>
    <p class="sansserif">
       This is a paragraph shown in sans-serif font.
    </p> 
    <p class="monospace">
       This is a paragraph shown in monospace font.
    </p> 
    <p class="cursive">
       This is a paragraph shown in cursive font.
    </p> 
    <p class="fantasy">
       This is a paragraph shown in fantasy font.
    </p> 
    ```
    CSS:
    ```css
    p.serif {
       font-family: "Times New Roman", Times, serif;
    }
    p.sansserif {
       font-family: Helvetica, Arial, sans-serif;
    }
    p.monospace {
       font-family: "Courier New", Courier, monospace;
    }
    p.cursive {
       font-family: Florence, cursive;
    }
    p.fantasy {
       font-family: Blippo, fantasy;
    }
    ```
+ Свойство **`font-size`** устанавливает *размер* шрифта:
    + xx-small - самый маленький
    + small - маленький
    + medium - средний
    + large - большой
    + larger - самый большой.
    Лучше использовать ключевые слова, так как каждый они адаптируются под каждый браузер или мобильное устройство:
    HTML:
    ```html
    <p class="small">
        Paragraph text set to be small
    </p>
    <p class="medium">
       Paragraph text set to be medium
    </p>
    <p class="large">
       Paragraph text set to be large
    </p>
    <p class="xlarge">
       Paragraph text set to be very large
    </p>
    ```
    CSS:
    ```css
    p.small {
        font-size: small;
    }
    p.medium {
       font-size: medium;
    }
    p.large {
       font-size: large;
    }
    p.xlarge {
       font-size: x-large;
    }
    ```
    Также можно в пискелах:
    ```css
    h1 { 
       font-size: 20 px ; 
    }
    ```
    Или в относительных единицах
    ```css
    h1 { 
       font-size: 1,25 em ; 
    }
    ```
+ Свойство **`font-style`** используется *для стиля текста*. Это свойство имеет 3 параметра:
    + normal - нормальный
    + italic - курсивный
    + oblique - наклонный

    ```css
    p.italic {
       font-style: italic;
    }
    ```
+ Свойство **`font-weight`** указывает вес шрифта:
    + normal - обычный
    + bold - жирный
    + bolder - жирнее
    + lighter - светлее
    ```css
    p.light {    
        font-weight:  lighter ; 
    } 
    p.bold {    
       font-weight:  bold ; 
    } 
    p.bolder { 
       font-weight:  bolder ; 
    }
    ```
    Так же можно указывать числом вес шрифта (от 100 до 900):
    ```css
    p.thicker {
       font-weight: 700;
    }
    ```
+ Свойство **'color'** - для цвета:
    ```css
    p.example {
       color: green;
    }
    h1 {
        color: #0000FF;
    }
    p.example {
       color: rgb(255,0,0);
    }
    ```
+ Свойство **`text-align`** - *выравнивает текст*:
    + left - по левому краю
    + right - по правому
    + center - по центру
    + justify - как в журнале, каждая строчка одинакового размера
    ```css
    p.left { 
    text-align: left ; 
    } 
    p.right { 
       text-align: right ; 
    } 
    p.center { 
       text-align: center ; 
    }
    ```
+ Свойство **`vertical-align`** - выравнивает текст *вертикально*:
    + top - вверху
    + middle - посередине
    + bottom - внизу
    HTML
    ```html
    <table border="1" cellpadding="2" cellspacing="0" style="height: 150px;">
      <tr>
         <td class="top">Top</td>
         <td class="middle">Middle</td>
         <td class="bottom">Bottom</td>
      </tr>
    </table>
    ```
    CSS:
    ```css
    td.top {
        vertical-align: top;
    }
    td.middle {
       vertical-align: middle;
    }
    td.bottom {
       vertical-align: bottom;
    }
    ```
+ Свойство **`text-decoration`** - как будет *декорирован* текст:
    + none - обычный текст
    + inherit - наследует это свойство от родительского элемета
    + overline - горизонтальная линия над текстом
    + underline - горизонтальная линия под текстом
    + line-through - зачеркивает текст
    ```css
    p.none {
        text-decoration: none;
    }
    p.inherit {
       text-decoration: inherit;
    }
    p.overline {
       text-decoration: overline;
    }
    p.underline {
       text-decoration: underline;
    }
    p.line-through {
       text-decoration: line-through;
    }
    ```
+ Свойство **`text-indent`** - показывает, сколько *расстояния нужно отсавить до начала первой строки*:
```css
p { 
   text-indent : 60px; 
}
```
+ Свойство **`text-shadow`** - добавляет *тень* тексту.Он принимает четыре значения: первое значение определяет расстояние тени в направлении **x** (по горизонтали) , второе значение задает расстояние в направлении **y** (по вертикали) , третье значение определяет **размытие** тени и четвертое значение устанавливает **цвет**:
```css
h1 {
   color: blue;
   font-size: 30pt;
   text-shadow: 5px 2px 4px grey;
}
```
+ Свойство **`text-transform`** *трансформирует текст*:
    + capitalize - каждое слово с большой буквы
    + uppercase - все буквы в верхнем регистре
    + lowercase - все буквы в нижнем регистре
    ```css
    p.uppercase {
        text-transform: uppercase;
    }
    p.lowercase {
       text-transform: lowercase;
    }
    ``` 
+ Свойство **`letter-spacing`** - определяет *расстояние между буквами* в тексте:
    + normal - по умолчанию
    + length - определяет дополнительный пробел между символами, с использованием таких единиц измерения, как px, pt, cm, mm и т. д..
    + inherit - наследует свойство от его родительского элемента;
    ```css
    p.positive { 
        letter-spacing: 4px; 
    }
    p.negative { 
       letter-spacing: -1.5px; 
    } 
    ```
+ Свойство **`word-spacing`** - *расстояние между словами*:
```css
p.normal { 
   word-spacing: normal;
}
p.px { 
   word-spacing: 30px;
}
```