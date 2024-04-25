USER202204101508
***
# Запуск MySQL
```
mysql -u root -p
```

# База данных
###### Показать список баз данных в MySQL 

```sql
SHOW DATABASES; 
```

###### Создать новую базу данных в MySQL 

```sql
CREATE DATABASE название-бд; 
```

###### Удалить базу данных

```sql
DROP DATABASE название-бд;
```

###### Выйти из MySQL

```sql
QUIT;
```

###### Удалить таблицу из базы данных

```sql
DROP TABLE название-таблицы; 
```

###### Выбрать базу, в которой нужно что-то сделать/использовать

```sql 
USE название-бд; 
```

###### Показать список таблиц в базе 

```sql
SHOW TABLES;
``` 

###### Блокировка таблицы в базе данных
```sql
USE DATABASE название-бд; - исп. бд
FLUSH TABLES WITH READ LOCK; - блокировать бд
```

###### Разблокировать таблицы в бд

```sql
UNLOCK TABLES;
```

###### Статус мастер-сервера

```sql
SHOW MASTER STATUS;
```

***
# Таблица
###### Создать макет таблицы
[The CHAR and VARCHAR Types](https://dev.mysql.com/doc/refman/8.0/en/char.html)
[ALTER TABLE Statement](https://dev.mysql.com/doc/refman/8.0/en/alter-table.html) - увеличить длинну VRCHAR
[The DATE, DATETIME, and TIMESTAMP Types](https://dev.mysql.com/doc/refman/8.0/en/datetime.html)
```sql
CREATE TABLE pet (name VARCHAR(20), owner VARCHAR(20), species VARCHAR(20), sex CHAR(1), birth DATE, death DATE);

VARCHAR(число) - макс. доступная длинна символов текста
CHAR(1)
DATE - дата
INT - число

INT NOT NULL AUTO_INCREMENT PRIMARY KEY
NOT NULL - не пустая ячейка
AUTO_INCREMENT - увеличение числа на 1 в след. ячейке
PRIMARY KEY - привязка ключа
```

###### Показать структуру таблицы
[DESCRIBE Statement](https://dev.mysql.com/doc/refman/8.0/en/describe.html)

```sql
DESCRIBE название-табл;
```

###### Показать список колонок в таблице
```sql
SHOW COLUMNS FROM test_tables FROM somedb; 
SHOW COLUMNS FROM somedb.test_tables; 
```

###### Записать данные, если такие данные есть, то просто их обновить, не создавая таких же новых данных
```sql
NSERT INTO table (id, name, age) VALUES(1, "A", 19) ON DUPLICATE KEY UPDATE name="A", age=19 
```

###### Отобразить данные из таблицы

```sql
pager less -SFX 
SELECT * FROM sometable; 
```

###### Отобразить данные из таблицы в вертикальнои виде 

```sql
SELECT * FROM sometable \G (в конце НЕ нужно ставить ;)
```

###### Создание таблицы с пустой колонкой

```sql
CREATE TABLE имяТаблицы (название колонки INT NOT NULL);
```

***

# *ссылки:*
[MySQL Data Manipulation](https://www.mysqltutorial.org/mysql-drop-database/)
список команд MySQL
![[Pasted image 20220410162050.png]]
# настройка master-slave
master: show master status;
![[Pasted image 20220412175246.png]]

- посмотреть список юзеров, которые могут работать с БД, вид подключения и IP с которого они могут заходить:
`SELECT user,authentication_string,plugin,host FROM mysql.user;`

- отчистка подключения к MASTER
`RESET SLAVE;`

- остановить SLAVE
`STOP SLAVE;`

- запустить SLAVE
`START SLAVE`

- статус SLAVE
`SHOW SLAVE STATUS \G`

[решения ошибок настройки MASTER-SLAVE](https://www.sqlpac.com/en/documents/mysql-8-replication-binary-log-file-position-quick-setup.html)
[Mysql error 1236 from master when reading data from binary log](https://stackoverflow.com/questions/28838040/mysql-error-1236-from-master-when-reading-data-from-binary-log)