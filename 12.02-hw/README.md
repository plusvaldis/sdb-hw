 # Домашнее задание к занятию "12.2. «Работа с данными (DDL/DML)» - Черепанов Владислав"





Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1
1.1. Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.  
Сначала устанавлваем gnupg , затем скачиваем с оф.сайта репозиторий, устанавливаем его, обновляем информацию об репо, устанавливаем сервер mysql и проверяем его запуск.  
- sudo apt install gnupg
- wget https://dev.mysql.com/get/mysql-apt-config_0.8.22-1_all.deb  
- sudo dpkg -i mysql-apt-config*  
- sudo apt update  
- apt install mysql-server  
- sudo systemctl status mysql  

1.2. Создайте учётную запись sys_temp.  
- CREATE USER 'sys_temp'@'localhost' IDENTIFIED BY 'password';  


1.3. Выполните запрос на получение списка пользователей в базе данных. (скриншот)  
- SELECT User,Host FROM mysql.user;  

![Скриншот-1](https://github.com/plusvaldis/sdb-hw/blob/main/12.02-hw/img/Screenshot_1.png)
---

1.4. Дайте все права для пользователя sys_temp.  
- GRANT ALL PRIVILEGES ON * . * TO 'sys_temp'@'localhost';  


1.5. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)  
- SHOW GRANTS FOR 'sys_temp'@'localhost';  

![Скриншот-2](https://github.com/plusvaldis/sdb-hw/blob/main/12.02-hw/img/Screenshot_2.png)
---

1.6. Переподключитесь к базе данных от имени sys_temp.
- mysql -u sys_temp -p  

Для смены типа аутентификации с sha2 используйте запрос: 
```sql
ALTER USER 'sys_test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```
1.6. По ссылке https://downloads.mysql.com/docs/sakila-db.zip скачайте дамп базы данных.  
- wget https://downloads.mysql.com/docs/sakila-db.zip  
- apt install unzip
- unzip sakila-db.zip

1.7. Восстановите дамп в базу данных.  
- mysql -u sys_temp -p  
- CREATE DATABASE sakila DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;  
- exit  
- export DBNAME=sakila
- mysql -u sys_temp -p ${sakila} < /tmp/sakila-db/sakila-schema.sql  
- mysql -u sys_temp -p ${sakila} < /tmp/sakila-db/sakila-data.sql  

1.8. При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)  
- mysql -u sys_temp -p  
- use sakila;  
- show tables;  

![Скриншот-3](https://github.com/plusvaldis/sdb-hw/blob/main/12.02-hw/img/Screenshot_3.png)
---

*Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.*


### Задание 2
Составьте таблицу, используя любой текстовый редактор или Excel, в которой должно быть два столбца: в первом должны быть названия таблиц восстановленной базы, во втором названия первичных ключей этих таблиц. Пример: (скриншот/текст)
```
Название таблицы | Название первичного ключа
customer         | customer_id
```  
  
выполним запрос через SQL на получение необходимой информации  
 SELECT  
     KU.table_name as TABLENAME  
    ,column_name as PRIMARYKEYCOLUMN  
 FROM INFORMATION_SCHEMA.TABLE_CONSTRAINTS AS TC  

 INNER JOIN INFORMATION_SCHEMA.KEY_COLUMN_USAGE AS KU  
    ON TC.CONSTRAINT_TYPE = 'PRIMARY KEY'  
    AND TC.CONSTRAINT_NAME = KU.CONSTRAINT_NAME  
    AND KU.table_name='actor'  

 ORDER BY  
     KU.TABLE_NAME  
    ,KU.ORDINAL_POSITION  
;   
```
Название таблицы | Название первичного ключа
| actor                      | actor_id            |
| actor_info                 | -                   |
| address                    | address_id          |
| category                   | category_id         |
| city                       | city_id             |
| country                    | country_id          |
| customer                   | customer_id         |
| customer_list              | -                   |
| film                       | film_id             |
| film_actor                 | film_id             |
| film_category              | categoty_id         |
| film_list                  | -                   |
| film_text                  | film_id             |
| inventory                  | inventory_id        |
| language                   | language_id         |
| nicer_but_slower_film_list | -                   |
| payment                    | payment_id          |
| rental                     | rental_id           |
| sales_by_film_category     | -                   |
| sales_by_store             | -                   |
| staff                      | staff_id            |
| staff_list                 | -                   |
| store                      | store_id            |
```


## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 3*
3.1. Уберите у пользователя sys_temp права на внесение, изменение и удаление данных из базы sakila.  
- REVOKE DROP,CREATE,UPDATE,DELETE ON *. * FROM 'sys_temp'@'localhost';

3.2. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)  
Как можем заметить на скриншоте права для пользователя sys_temp, на удаление, внесение изменений и создание исключены.  
  
![Скриншот-4](https://github.com/plusvaldis/sdb-hw/blob/main/12.02-hw/img/Screenshot_4.png)
---

*Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.*