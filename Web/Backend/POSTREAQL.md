# postgreSQL
**postgreSQL - объектно-реляционная система управления базами данных**

+ [Основные команды](#basic_commands)

<a name="basic_commands"></a> Основные команды:
+ **`sudo -u postgres psql`** - запуск СУБД;
+ **`\q`** - выход из СУБД;
+ **`CREATE DATABASE test;`** - создать базу данных test. Если не поставить точку с запятой СУБД будет думать, что команда не закончена;
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
