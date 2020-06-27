# REST (REpresentational State Transfer)
**REST - архитектурный стиль взаимодействия компонентов распределённого приложения в сети.**

+ [RESTful API](#restful)
+ [HTTP](#http)
+ [HTTP заголовок](#httpheader)
+ [Архитектурные ограницения REST](#restconstraint)
+ [Структура ресурса и его именование](#resourcestructure)

## <a name="restful"></a> RESTful API
+ REST - это архитектурный стиль, который часто используется для связи в веб-сервисной разработке. REST сам по себе очень гибкий, однако разработчики используют определенные практики при разработке REST API. REST имеет возможность управлять различными типами запросов и возвращать разные типы данных (типа **JSON** или **XML**)
+ RESTful API - это, основанная на REST технология. В основном используется для HTTP web APIs, поскольку не требует дополнительных библиотек или пакетов. Запросы посылаются на определенный URL, который, определен на сервере. После обработки, сервер делает ответ с этого URL. Важно делать интуитивно понятные и удобные API. Запросы к API URLs делаются при помощи HTTP методов, таких как **GET, POST, PUT, DELETE**.

Примеры:
+ **GET** запрос к Facebook Graph API, чтобы получить информацию о пользователе:
    ```
    https://graph.facebook.com/v3.0/{user-id}
    ``` 
+ Запрос на получение Google Geolocate API:
    ```
    https://maps.googleapis.com/maps/api/geocode/json
    ```
## <a name="http"></a> HTTP
+ HTTP(Hypertext Transfer Protocol) - это протокол прикладного уровня для передачи гепермедиа документов. По умолчанию HTTP построен на клиент-серверной модели, где клиент делает запрос серверу и ждет ответа. Ответ содержит информацию о состоянии завершения запроса и запрошенное содержимое в теле сообщения.
+ HTTP не сохраняет состояние! То есть всю необходиую для ответа информацию необходимо соедержать в одном запросе, а не посылать несколько.
+ HTTP сообщение - это блок данных, пересылаемых между HTTP-приложениями. Каждое сообщение содержит либо запрос от клиента, либо ответ от сервера. Ответ и запрос состоят из 3-х частей:
    + *Начальная строка* - описывает сообщение.
    + Необязательный *заголовок* - содержит атрибуты
    + Необязательное *тело* - содержит данные.

Примеры:
+ Запрос:
    ```
    GET http://www.w3schools.com/ HTTP/1.1
    Host: www.w3schools.com
    Connection: keep-alive
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
    User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) 
    Accept-Encoding: gzip,deflate,sdch
    Accept-Language: en-US;q=0.8,en;q=0.6
    Cookie: __gads=ID=42213729da4df8df:T=1368250765:S=ALNI_MaOAFe3U1T9Syh; 
    (empty line)
    (message body goes here)
    ```
+ Ответ:
    ```
    HTTP/1.1 200 OK
    Cache-Control: private
    Content-Type: text/html
    Content-Encoding: gzip
    Vary: Accept-Encoding
    Server: Microsoft-IIS/7.5
    Set-Cookie: ASPSESSIONIDQQRBACTR=FOCCINICEFAMEKODNKIBFOJP; path=/
    X-Powered-By: ASP.NET
    Date: Sun, 04 Aug 2013 13:33:59 GMT
    Content-Length: 8434
    (empty line)
    (page content follows)
    ```

## <a name="httpheader"></a> HTTP заголовок
+ HTTP-заголовок - это важная часть HTTP-сообщения, которая передает дополнительную информацию вместе с телом сообщения. Содержит имя, за которым идет ":", а потом его значение. Имя является регистронезависимым. В примере ниже "Content-Type" - это имя, "image/png" - это значение:
    ```
    Content-Type: image/png
    ```
+ Обычно, работая с web-сайтами, посылается 2 типа сообщений: *запрос* и *ответ*:
    + заголовок **запроса** содержит информацию о данных, которые хочет получить клиент, а также информацию о самом клиенте.
    + заголовок **ответа** содержит информацию об отпрвляемом ресурсе, такую как имя сервера, местоположение, версия и т. д.:
Пример запроса:    
```
GET /file.html HTTP/version
Host: HostName
From: NameOfRequestingUser
Cache-Control: no-cache
```

## <a name="restconstraint"> </a> Архитектурные ограничения REST
+ REST предпологает, что приложения используют HTTP, как это изначально предпологалось. То есть для поиска данных используется **GET**, для создания - **POST**, для изменения - **PUT**, для удаления - **DELETE**. REST позволяет осуществлять независимую эволюцию приложений без сервисов, моделей или действий, тесно связанных с самим уровнем API.
+ Разрабатывая REST API, необходимо, чтобы все клиент-серверные взаимодействия не зависили от состояний друг-друга. Сервер не должен хранить ничего о последнем запросе с клиента. **Он должен обрабатывать каждый запрос как новый.** Если клиентское приложение нуждается в сохранении информации конечного пользователя, то оно должно в каждом запросе содержать информацию об этом пользователе (включая данные аутенфикации и авторизации).

## <a name="resourcestructure"></a> Структура ресурса и его именование

+ В REST, данные, которые мы обрабатываем, называются **ресурсами**. Чтобы было проще использовать ресурсы, стоит следовать некоторым правилам именования API. 

+ Ресурс может быть **singleton или colection**. Например, user -  singleton, users - collection. Можно назвать ресурс collection как `/users`, а ресурс singleton как `/users/{id}`. Также у конкретного user может быть sub-collection: `/users/{id}/posts/{id}` - ресурс конкретного post, конкретного  user.

+ Некоротые правила именования ресурса:
    + Использовать вместо глаголов **существительные**. Главная причина - существительные имеют свойства, а глаголы - нет:
    ```
    http://api.example.com/users
    http://api.example.com/users/{id}
    ```

    + При получении singleton'a необходимо указывать его **экземпляр** из collection:
    ```
    http://api.example.com/users/{id}           #ПОЛУЧИТЬ ПО ID
    http://api.example.com/files/license-2018   #ПОЛУЧИТЬ ПО УНИКАЛЬНОМУ ИМЕНИ
    ```
    
    + Чтобы обозначать collection, необходимо использовать **множетсвенное число**:
    ```
    http://api.example.com/users
    http://api.example.com/users/{id}/posts
    ```

    + **Для управления** ресурсами используются **глаголы**:
    ```
    http://api.example.com/users/{id}/projects/{id}/run
    http://api.example.com/user/{id}/playlists/{id}/play
    ```

    + Для указания пути используется обратный слеш ("**/**"). Обратный слеш указыват иерархию между ресурсами:
    ```
    http://api.example.com/users/{id}/posts
    ```

    + Путь **не должен заканчиваться обратным слешем**:
    ```
    http://api.example.com/users/{id}/posts/ #ПЛОХО
    http://api.example.com/users/{id}/posts  #ХОРОШО
    ```

    + **Дефисы** - хороший способ **разделять** слова:
    ```
    http://api.example.com/users/accessLevels  #ПЛОХО
    http://api.example.com/users/access-levels #ХОРОШО
    ```

    + Исользовать только **нижний регистр**:
    ```
    http://api.example.com/home/my-file #ХОРОШО
    http://API.EXAMPLE.COM/home/my-file #ПЛОХО 
    http://api.example.com/Home/My-File #ПЛОХО
    ```
    *В данном случае представлены 3 одинаковых API, однако 3й может еще и вызвать проблемы в зависимости от реализации и типа сервера*

    + Никогда **не указвается расширение** файла:
    ```
    http://api.example.com/files/license.pdf #ПЛОХО
    http://api.example.com/files/license     #ХОРОШО
    ```

    + Использовать **query-компоненты(?, &)** для фильтрации запросов:
    ```
    http://api.example.com/projects/{id}/run?lang=cpp
    http://api.example.com/projects/{id}/run?lang=cpp&type=gcc
    ```

    + Не использовать имена из CRUD (create, read, update, delete). Ничего в URL не должно показывать, какое действие исполняется:
    ```
    GET http://api.example.com/users            #Получить всех users
    POST http://api.example.com/users           #Создать нового user
    GET http://api.example.com/users/{id}       #Получить конкретного user
    PUT http://api.example.com/users/{id}       #Изменить конкретного user
    DELETE http://api.example.com/users/{id}    #Удалить конкретного user
    ```
