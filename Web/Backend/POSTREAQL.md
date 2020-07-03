# postgreSQL
**postgreSQL - объектно-реляционная система управления базами данных**

+[Основные команды](#basic_commands)

<a name="basic_commands"></a> Основные команды:
+ **`sudo -u postgres psql`** - запуск СУБД;
+ **`\q`** - выход из СУБД;
+ **`CREATE DATABASE test;`** - создать базу данных test. Если не поставить точку с запятой СУБД будет думать, что команда не закончена;
+ **```CREATE TABLE users (
        Id serial primary key, 
        Name character varying(30), 
        Age integer
    );```** - создание новой таблицы users с 3-мя полями: *Id, Name, Age* ;
+ **`INSERT INTO users (Name, Age) VALUES ('Tom', 33);`** - добавление нового элемента в таблицу, Id добавится автоматически, так как является первичным ключем;
+ **`SELECT * FROM users;`** - вывести все записи в таблице users;