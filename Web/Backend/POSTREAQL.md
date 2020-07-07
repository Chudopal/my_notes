# postgreSQL
**postgreSQL - объектно-реляционная система управления базами данных**

+ [Основные команды](#basic_commands)
+ [Типы данных](#data_types)
+ [Первичный ключ](#primarykey)
+ [Внешний ключ](#forign)
+ [Изменение таблицы](#changetable)
+ [Добавление данных](#addeddata)
+ [Получение данных](#select)
+ [Фильтрация](#where)
+ [Обновление данных](#update)
+ [Удаление данных](#delete)
+ [Запросы](#requests)
+ [Агрегатные функции](#agregators)

### <a name="basic_commands"></a> Основные команды:
+ **`sudo -u postgres psql`** - запуск СУБД;
+ **`\q`** - выход из СУБД;
+ **`CREATE DATABASE test;`** - создать базу данных test. Если не поставить точку с запятой СУБД будет думать, что команда не закончена;
+ **`\c test`** - перейти к бд. Сокращенно от connection
+ создание новой таблицы users с 3-мя полями: *Id, Name, Age* :
    ```
    CREATE TABLE users (
            Id SERIAL PRIMARY KEY, 
            Name CHARACTER VARYING(30), 
            Age INTEGER
        );
    ```
+ **`INSERT INTO users (Name, Age) VALUES ('Tom', 33);`** - добавление нового элемента в таблицу, Id добавится автоматически, так как является первичным ключем;
+ **`SELECT * FROM users;`** - вывести все записи в таблице users;
+ **`DROP DATABASE users;`** - удалить базу данных. Удаляемая база должна быть неактивна, т е подключение к ней должно быть закрыто;
+ **`DROP TABLE users;`** - удалить таблицу;

### <a name="data_types"></a> Типы данных
Тип Даных     | Описание
------------- | -------------
**SERIAL**    | представляет автоинкрементирующееся числовое значение, которое занимает 4 байта и может хранить числа от 1 до 2147483647. *Значение данного типа образуется путем автоинкремента значения предыдущей строки. Поэтому, как правило, данный тип используется для определения идентификаторов строки.*
**SMALLSERIAL**| представляет автоинкрементирующееся числовое значение, которое занимает 2 байта и может хранить числа от 1 до 32767. Аналог типа serial для небольших чисел.
**BIGSERIAL** | представляет автоинкрементирующееся числовое значение, которое занимает 8 байт и может хранить числа от 1 до 9223372036854775807. Аналог типа serial для больших чисел.
**SMALLINT**  | хранит числа от -32768 до +32767. Занимает 2 байта. Имеет псевдоним **int2**.
**INTEGER**   | хранит числа от -2147483648 до +2147483647. Занимает 4 байта. Имеет псевдонимы **int** и **int4**.
**BIGINT**    | хранит числа от -9223372036854775808 до +9223372036854775807. Занимает 8 байт. Имеет псевдоним **int8**.
**NUMERIC**   | хранит числа с фиксированной точностью, которые могут иметь до 131072 знаков в целой части и до 16383 знаков после запятой. umeric(precision, scale). precision - количество цифер в числе, scale - количество цифер после запятой.
**DECIMAL**   | хранит числа с фиксированной точностью, которые могут иметь до 131072 знаков в целой части и до 16383 знаков в дробной части. То же самое, что и numeric.
**REAL**      | хранит числа с плавающей точкой из диапазона от 1E-37 до 1E+37. Занимает 4 байта. Имеет псевдоним **float4**.
**DOUBLE PRECISION** | хранит числа с плавающей точкой из диапазона от 1E-307 до 1E+308. Занимает 8 байт. Имеет псевдоним **float8**.
**MONEY**     | тип для денежных операций, который может принимать значения в диапазоне от -92233720368547758.08 до +92233720368547758.07 и занимает 8 байт.
**CHARACTER(n)** | представляет строку из фиксированного количества символов. С помощью параметра задается задается количетво символов в строке. Имеет псевдоним char(n).
**CHARACTER VARYING(n)**| представляет строку из фиксированного количества символов. С помощью параметра задается задается количетво символов в строке. Имеет псевдоним **varchar(n)**.
**TEXT**      | текст произвольной длины
**BYTEA**     | тип для хранения бинарных данных
**TIMESTAMP** | хранит дату и время. Занимает 8 байт. Для дат самое нижнее значение - 4713 г до н.э., самое верхнее значение - 294276 г н.э.
**TIMESTAMP WITH TIME ZONE**|  то же самое, что и timestamp, только добавляет данные о часовом поясе.
**DATE**      | представляет дату от 4713 г. до н.э. до 5874897 г н.э. Занимает 4 байта.
**TIME**      | хранит время с точностью до 1 микросекунды без указания часового пояса. Принимает значения от 00:00:00 до 24:00:00. Занимает 8 байт.
**TIME WITH TIME ZINE**| хранит время с точностью до 1 микросекунды с указанием часового пояса. Принимает значения от 00:00:00+1459 до 24:00:00-1459. Занимает 12 байт.
**INTERVAL**  | представляет временной интервал. Занимает 16 байт.
**BOOLEAN**   | может хранить одно из двух значений: true или false.
**CIDR**      | интернет-адрес в формате IPv4 и IPv6. Например, 192.168.0.1. Занимает от 7 до 19 байт.
**INET**      | интернет-адрес в формате cidr/y, где cidr это адрес в формате IPv4 или IPv6, а /y - количество бит в адресе (если этот параметр не указан, то используется 34 для IPv4, 128 для IPv6). Например, 192.168.0.1/24 или 2001:4f8:3:ba:2e0:81ff:fe22:d1f1/128. Занимает от 7 до 19 байт.
**MACADDR**   | хранит MAC-адрес. Занимает 6 байт.
**MACADDR8**  | хранит MAC-адрес в формате EUI-64. Занимает 8 байт.
**JSON**      | хранит данные json в текстовом виде.
**JSONB**     | хранит данные json в бинарном формате.
**XML**       | хранит даные в формате XML.

### <a name='primarykey'></a> Первичный ключ
+ **`PRIMARY KEY`** - сделать столбец первичным ключем. Первичный уникально идентифицирует строку в таблице:
    ```
    CREATE TABLE Customers
    (
        Id SERIAL PRIMARY KEY,
        FirstName CHARACTER VARYING(30),
        LastName CHARACTER VARYING(30),
        Email CHARACTER VARYING(30),
        Age INTEGER
    )
    ```
    Или так
    ```
    CREATE TABLE Customers
    (
        Id SERIAL,
        FirstName CHARACTER VARYING(30),
        LastName CHARACTER VARYING(30),
        Email CHARACTER VARYING(30),
        Age INTEGER,
        PRIMARY KEY(Id)
    );
    ```
    Первичный ключ может быть составным, такое может понадобиться если нужно, чтобы два столбца вместе уникально идентифицировали строку в таблице, OrderId и ProductId вместе выступают как составной первичный ключ.:
    ```
    CREATE TABLE OrderLines
    (
        OrderId INTEGER,
        ProductId INTEGER,
        Quantity INTEGER,
        Price MONEY,
        PRIMARY KEY(OrderId, ProductId)
    );
    ```
+ **`UNIQUE`** - запрещает добавлять в разные слобцы одинаковое значение:
    ```
    CREATE TABLE Customers
    (
        Id INT PRIMARY KEY IDENTITY,
        Age INT,
        FirstName NVARCHAR(20),
        LastName NVARCHAR(20),
        Email VARCHAR(30) UNIQUE,
        Phone VARCHAR(20) UNIQUE
    )
    ```
    Или так
    ```
    CREATE TABLE Customers
    (
        Id SERIAL PRIMARY KEY,
        FirstName CHARACTER VARYING(20),
        LastName CHARACTER VARYING(20),
        Email CHARACTER VARYING(30),
        Phone CHARACTER VARYING(30),
        Age INTEGER,
        UNIQUE(Email, Phone)
    );
    ```
    Или так
    ```
    CREATE TABLE Customers
    (
        Id SERIAL PRIMARY KEY,
        FirstName CHARACTER VARYING(20),
        LastName CHARACTER VARYING(20),
        Email CHARACTER VARYING(30),
        Phone CHARACTER VARYING(30),
        Age INTEGER,
        UNIQUE(Email), 
        UNIQUE(Phone)
    );
    ```
+ **`NULL | NOT NULL`** - NULL - столбец может содержать пустые строки. По умолчанию всезде стоит NULL, кроме столбца с первичным ключем:
    ```
    CREATE TABLE Customers
    (
        Id SERIAL PRIMARY KEY,
        FirstName CHARACTER VARYING(20) NOT NULL,
        LastName CHARACTER VARYING(20) NOT NULL,
        Age INTEGER
    );
    ```
+ **`DEFAULT`** - определяет значение столбца по умолчнию:
    ```
    CREATE TABLE Customers
    (
        Id SERIAL PRIMARY KEY,
        FirstName VARCHAR(20),
        LastName VARCHAR(20),
        Age INTEGER DEFAULT 18
    );
    ```
+ **`CHECK`** - задает проверку, если данные ей соотвествуют, то они записываются в строку:
    ```
    CREATE TABLE Customers
    (
        Id SERIAL PRIMARY KEY,
        FirstName VARCHAR(20),
        LastName VARCHAR(20),
        Age INTEGER DEFAULT 18 CHECK(Age >0 AND Age < 100),
        Email VARCHAR(30) UNIQUE CHECK(Email !=''),
        Phone VARCHAR(20) UNIQUE CHECK(Phone !='')
    );
    ```
    Или так
    ```
    CREATE TABLE Customers
    (
        Id SERIAL PRIMARY KEY,
        Age INTEGER DEFAULT 18,
        FirstName VARCHAR(20),
        LastName VARCHAR(20),
        Email VARCHAR(30) UNIQUE,
        Phone VARCHAR(20) UNIQUE,
        CHECK((Age >0 AND Age<100) AND (Email !='') AND (Phone !=''))
    );
    ```
+ **`CONSTRAINT`** - задает имена для ограничений. Потом по данным именам можно данные ограниыения изменять или удалять:
    ```
    CREATE TABLE Customers
    (
        Id SERIAL CONSTRAINT customer_Id PRIMARY KEY,
        Age INTEGER CONSTRAINT customers_age_check CHECK(Age >0 AND Age < 100),
        FirstName VARCHAR(20) NOT NULL,
        LastName VARCHAR(20) NOT NULL,
        Email VARCHAR(30) CONSTRAINT customers_email_key UNIQUE,
        Phone VARCHAR(20) CONSTRAINT customers_phone_key UNIQUE
    );
    ```
    Или так:
    ```
    CREATE TABLE Customers
    (
        Id SERIAL,
        Age INTEGER,
        FirstName VARCHAR(20) NOT NULL,
        LastName VARCHAR(20) NOT NULL,
        Email VARCHAR(30),
        Phone VARCHAR(20),
        CONSTRAINT customer_Id PRIMARY KEY(Id),
        CONSTRAINT customers_age_check CHECK(Age >0 AND Age < 100),
        CONSTRAINT customers_email_key UNIQUE(Email),
        CONSTRAINT customers_phone_key UNIQUE(Phone)
    );
    ```

### <a name="forign"></a> Внешний ключ
+ Для связи между таблицами применяются **внешние ключи**. Внешний ключ устанавливается для столбца из зависимой, **подчиненной таблицы** (referencing table), и указывает на один из столбцов **главной таблицы** (referenced table). Как правило, внешний ключ указывает на **первичный ключ** из связанной **главной таблицы**;
+ **`REFERENCES`** - слово для создания внешнего ключа. Customers является главной и представляет клиента. Orders является зависимой и представляет заказ, сделанный клиентом. Эта таблица через столбец CustomerId связана с таблицей Customers и ее столбцом Id. То есть столбец CustomerId является внешним ключом, который указывает на столбец Id из таблицы Customers.:
    ```
    CREATE TABLE Customers
    (
        Id SERIAL PRIMARY KEY,
        Age INTEGER, 
        FirstName VARCHAR(20) NOT NULL
    );
    
    CREATE TABLE Orders
    (
        Id SERIAL PRIMARY KEY,
        CustomerId INTEGER REFERENCES Customers (Id),
        Quantity INTEGER
    );
    ```
    Или так:
    ```
    CREATE TABLE Customers
    (
        Id SERIAL PRIMARY KEY,
        Age INTEGER, 
        FirstName VARCHAR(20) NOT NULL
    );
    
    CREATE TABLE Orders
    (
        Id SERIAL PRIMARY KEY,
        CustomerId INTEGER,
        Quantity INTEGER,
        FOREIGN KEY (CustomerId) REFERENCES Customers (Id)
    );
    ```
+ **`ON DELETE`** и **`ON UPDATE`** - можно установить действия, которые выполняются соответственно при удалении и изменении связанной строки из главной таблицы. Существуют следующие опции:
    Опция         | Описание
    ------------- | -------------
    **`CASCADE`** | **автоматически** удаляет или изменяет строки из зависимой таблицы при удалении или изменении связанных строк в главной таблице.
    **`RESTRICT`**| **предотвращает** какие-либо действия в зависимой таблице при удалении или изменении связанных строк в главной таблице. **То есть фактически какие-либо действия отсутствуют.**
    **`NO ACTION`** | действие по умолчанию, предотвращает какие-либо действия в зависимой таблице при удалении или изменении связанных строк в главной таблице. И генерирует ошибку. **В отличие от RESTRICT выполняет отложенную проверку на связанность между таблицами.**
    **`SET NULL`** | при удалении связанной строки из главной таблицы **устанавливает для столбца внешнего ключа значение NULL**.
    **`SET DEFAULT`** | при удалении связанной строки из главной таблицы устанавливает для столбца внешнего ключа **значение по умолчанию**, которое задается с помощью атрибуты DEFAULT. Если для столбца не задано значение по умолчанию, то в качестве него применяется значение **NULL**.
+ **Каскаднеое удаление** - по умолчанию, если на строку из главной таблицы по внешнему ключу ссылается какая-либо строка из зависимой таблицы, **то мы не сможем удалить эту строку из главной таблицы**. Вначале нам **необходимо удалить все связанные строки из зависимой таблицы**. И если при удалении строки из главной таблицы необходимо, чтобы были удалены все связанные строки из зависимой таблицы, то **применяется каскадное удаление, то есть опция CASCADE**:
    ```
    CREATE TABLE Orders
    (
        Id SERIAL PRIMARY KEY,
        CustomerId INTEGER,
        Quantity INTEGER,
        FOREIGN KEY (CustomerId) REFERENCES Customers (Id) ON DELETE CASCADE
    );
    ```
+ **Установка NULL** - при установки для внешнего ключа опции SET NULL необходимо, чтобы столбец внешнего ключа допускал значение NULL:
    ```
    CREATE TABLE Orders
    (
        Id SERIAL PRIMARY KEY,
        CustomerId INTEGER,
        Quantity INTEGER,
        FOREIGN KEY (CustomerId) REFERENCES Customers (Id) ON DELETE SET NULL
    );
    ```
+ **Установка значения по умолчанию**
    ```
    CREATE TABLE Orders
    (
        Id SERIAL PRIMARY KEY,
        CustomerId INTEGER DEFAULT 1,
        Quantity INTEGER,
        FOREIGN KEY (CustomerId) REFERENCES Customers (Id) ON DELETE SET DEFAULT
    );
    ```
### <a name="changetable"> </a> Изменение таблицы
+ **Добавление нового столбца**:
    ```
    ALTER TABLE Customers
    ADD Phone CHARACTER VARYING(20) NULL;
    ```
    В случае, если необходимо установить значение помимо NULL, необходимо задать его через NULL:
    ```
    ALTER TABLE Customers
    ADD Address CHARACTER VARYING(30) NOT NULL DEFAULT 'Неизвестно';
    ```
+ **Удаление столбца**:
    ```
    ALTER TABLE Customers
    DROP COLUMN Address;
    ```
+ **Изменение типа столбца**:
    ```
    ALTER TABLE Customers
    ALTER COLUMN FirstName TYPE VARCHAR(50);
    ```
+ **Изменение ограничений столбца**:
    ```
    ALTER TABLE Customers 
    ALTER COLUMN FirstName 
    SET NOT NULL;
    ```
    удаление:
    ```
    ALTER TABLE Customers 
    ALTER COLUMN FirstName 
    SET NOT NULL;
    ```
+ **Изменение ограничений таблицы**:
    ```
    ALTER TABLE Customers
    ADD CHECK (Age > 0);
    ```
+ **Переименование столбца и таблицы**:
    ```
    ALTER TABLE Customers
    RENAME COLUMN Address TO City;
    ```
    Таблицы:
    ```
    ALTER TABLE Customers
    RENAME TO Users;
    ```
### <a name='addeddata'> </a> Добавление данных
+ **`INSERT INTO ... VALUES (...) ;`** - добавление данных. Значения для столбцов в скобках после ключевого слова VALUES передаются по порядку их объявления:
    ```
    INSERT INTO Products VALUES (1, 'Galaxy S9', 'Samsung', 4, 63000);
    ```
    или так
    ```
    INSERT INTO Products (ProductName, Price, Manufacturer) 
    VALUES ('Galaxy S9', 63000, 'Samsung');
    ```
    можно добавить сразу несколько строк
    ```
    INSERT INTO Products  (ProductName, Manufacturer, Price, ProductCount)
    VALUES
    ('iPhone 6', 'Apple', 3, 36000),
    ('Galaxy S8', 'Samsung', 2, 46000),
    ('Galaxy S8 Plus', 'Samsung', 1, 56000);
    ```
+ **`RETURNING`** - возвращает значение выбранного столбца при добавлении в таблицу нового элемента:
    ```
    INSERT INTO Products 
    (ProductName, Manufacturer, Price, ProductCount) 
    VALUES('Desire 12', 'HTC', 8, 21000) RETURNING id;
    ```
### <a name="select"></a> Получение данных
+ **`SELECT ... FROM ... ;`** - конструкция для получения данных из таблицы:
    ```
    SELECT * FROM Products;
    ```
    получени выбранных столбцов из бд
    ```
    SELECT ProductName, Price FROM Products;
    ```
+ при выборе данных, **можно производить операции над данными**, например умножение `Price` на `ProductCount`:
    ```
    SELECT ProductCount, Manufacturer, Price * ProductCount
    FROM Products;
    ```
+ **`AS`** - определение псефдонима для столбцов:
    ```
    SELECT ProductCount AS Title, 
    Manufacturer, 
    Price * ProductCount  AS TotalSum
    FROM Products;
    ```

### <a name="where"> </a> Фильтрация
+ **`SELECT ... FROM ... WHERE ... ;`** - конструкция для фильтрации данных, выводимх в запросе:
    ```
    SELECT * FROM Products
    WHERE Manufacturer = 'Apple';
    ```
+ **Операторы сравнения**:
    + **`=`** - сравнение на равенство;
    + **`<>`** - сравнение на неравенство
    + **`<`** - меньше чем
    + **`>`** - больше чем
    + **`!<`** - не меньше
    + **`!>`** - не больше
    + **`<=`** - меньше че
    + **`>=`** - больше чем или равно
    ```
    SELECT * FROM Products
    WHERE Price < 39000;
    ```
    Для сравнения могут исплользоваться конструкции посложнее
    ```
    SELECT * FROM Products
    WHERE Price * ProductCount > 90000;
    ```
+ **Логические операторы**:
    + **`AND`** - логическое "И":
        ```выражение1 AND выражение2```
    + **`OR`** - логическое "ИЛИ"
        ```выражение1 OR выражение2```
    + **`NOT`** - логическое "НЕ";
        ```NOT выражение```
    ```
    SELECT * FROM Products
    WHERE Manufacturer = 'Samsung' AND Price > 50000;
    ```
    ```
    SELECT * FROM Products
    WHERE NOT Manufacturer = 'Samsung';
    ```
    ```
    SELECT * FROM Products
    WHERE Manufacturer = 'Samsung' OR Price > 30000 AND ProductCount > 2;
    ```
    **Так как оператор AND имеет более высокий приоритет, то сначала будет выполняться подвыражение Price > 30000 AND ProductCount > 2, и только потом оператор OR. То есть здесь выбираются товары, которыех на складе больше 2 и у которых одновременно цена больше 30000, либо те товары, производителем которых является Samsung.**
+ **`IS NULL`** - используется для проверки на отсуствие значения, **не эквивалентен `""`**:
    ```
    SELECT * FROM Products
    WHERE ProductCount IS NULL;
    ```
    наоборот
    ```
    SELECT * FROM Products
    WHERE ProductCount IS NOT NULL;
    ```

### <a name="update"> </a> Обновление данных
+ **`UPDATE ... SET ... (WHERE ...);`** - команда для обновления данных. Обновление всего столбца:
    ```
    UPDATE Products
    SET Price = Price + 3000;
    ```
    С помощью **`WHERE`** можно указать, какие конкретно строки обновлять:
    ```
    UPDATE Products
    SET Manufacturer = 'Samsung Inc.'
    WHERE Manufacturer = 'Samsung';
    ```
    Так же можно обновлять сразу несколько столбцов:
    ```
    UPDATE Products
    SET Manufacturer = 'Samsung',
        ProductCount = ProductCount + 3
    WHERE Manufacturer = 'Samsung Inc.';
    ```
### <a name="delete"> </a> Удаление данных
+ **`DELETE FROM ... WHERE ...;`** - удаление строк из таблицы:
    ```
    DELETE FROM Products
    WHERE Manufacturer='Apple';
    ```
    Такое тоже можно
    ```
    DELETE FROM Products
    WHERE Manufacturer='HTC' AND Price < 15000;
    ```
    Удаление всех строк
    ```
    DELETE FROM Products;
    ```
### <a name="requests"> </a> Запросы
+ **`SELECT DISTINCT ... FROM ...;`** - позволяет вывести все уникальные строки:
    ```
    SELECT DISTINCT Manufacturer FROM Products;
    /*
    выведет только уникальных производтелей
    */
    ```
+ **`SELECT ... FROM ... ORDER BY ...;`** - упорядочить строки по определенному столбцу, упорядочивание происходит по возрастанию:
    ```
    SELECT * FROM Products
    ORDER BY ProductCount;
    ```
    или так
    ```
    SELECT ProductName, ProductCount * Price AS TotalSum
    FROM Products
    ORDER BY TotalSum;
    ```
    + Чтобы сортировка происходила по убыванию можно использовать параметр **`DESC`**:
        ```
        SELECT ProductName, Manufacturer
        FROM Products
        ORDER BY Manufacturer DESC;
        ```
    + По умолчанию сортировка происходит по возрастанию, однако это можно задать явно, используя параметр **`ASC`**:
        ```
        SELECT ProductName, Manufacturer
        FROM Products
        ORDER BY Manufacturer ASC;
        ```
    + Сортировку можно производить по нескольким параметрам:
        ```
        SELECT ProductName, Manufacturer
        FROM Products
        ORDER BY Manufacturer ASC;
        ```
        можно использовать `ASC` и `DESC`:
        ```
        SELECT ProductName, Price, Manufacturer
        FROM Products
        ORDER BY Manufacturer ASC, ProductName DESC;
        ```
+ **`SELECT ... FROM ... ORDER BY ... LIMIT n;`** - команда для запроса определенного количества строк из базы. n - количество строк:
    ```
    SELECT * FROM Products
    ORDER BY ProductName
    LIMIT 4;
    ```
+ **`SELECT ... FROM ... ORDER BY ... OFFSET n;`** - команда для запроса с определенной строки:
    ```
    SELECT * FROM Products
    ORDER BY ProductName
    LIMIT 3 OFFSET 2;
    ```
+  **`(NOT) IN`** - оператор для указания принадлежности элемента к списку:
    ```
    SELECT * FROM Products
    WHERE Manufacturer IN ('Samsung', 'HTC', 'Huawei');
    ```
+ **`(NOT) BETWEEN`** - оператор для указания границ принадлежности:
    ```
    SELECT * FROM Products
    WHERE Price BETWEEN 20000 AND 50000;
    ```
    можно так
    ```
    SELECT * FROM Products
    WHERE Price * ProductCount BETWEEN 90000 AND 150000;
    ```
+ **`LIKE`** - принимает шаблон строки, котрому должны соотвествовать запросы:
    + **`%`** - любое количество символов;
    + **`_`** - один символ;
    ```
    SELECT * FROM Products
    WHERE ProductName LIKE 'iPhone%';
    ```
### <a name="agregators"> </a> Агрегатные функции
+ **Агрегатные фунции** - вычисляют одно значение над некоторым набором строк:

Функция      | Описание
-------------| -------------
**`AVG`**    | Среднее значение
**`BIT_AND`**| Побитовое умножение для чисел (логич И)
**`BIT_OR`** | Побитовое сложение для чисел (логич ИЛИ)
**`BOOL_AND`**| Логич И для булевых типов
**`BOOL_OR`**| Логич ИЛИ для булевых типов
**`COUNT(*)`**| Количество строк в запросе
**`COUNT(expression)`**| находит количество строк в запросе, для которых expression не содержит значение NULL.
**`SUM`**    | Вычисление суммы всех выбранных строк
**`MIN`**    | Находит наименьшее значение
**`MAX`**    | Находит наибольшее значение
**`STRING_AGG(expression, delimiter)`** |  соединяет с помощью delimiter(разделитель между строками) все текстовые значения из expression в одну строку.

+ **Примеры**:   
    ```
    SELECT AVG(Price) AS Average_Price FROM Products;
    ```
    ```
    SELECT AVG(Price) FROM Products
    WHERE Company='Apple';
    ```
    ```
    SELECT COUNT(*) FROM Products;
    ```
    ```
    SELECT MIN(Price) FROM Products;
    ```
    ```
    SELECT SUM(ProductCount) FROM Products;
    ```
    ```
    SELECT BOOL_AND(IsDiscounted) FROM Products;
    ```
    ```
    SELECT STRING_AGG(ProductName, ', ') FROM Products;
    ```
    можно комбинировать
    ```
    SELECT COUNT(*) AS ProdCount,
       SUM(ProductCount) AS TotalCount,
       MIN(Price) AS MinPrice,
       MAX(Price) AS MaxPrice,
       AVG(Price) AS AvgPrice
    FROM Products;
    ```