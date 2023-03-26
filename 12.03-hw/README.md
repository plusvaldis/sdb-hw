 # Домашнее задание к занятию "12.2. «SQL. Часть 1» - Черепанов Владислав"





Задание можно выполнить как в любом IDE, так и в командной строке.

Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1

Получите уникальные названия районов из таблицы с адресами, которые начинаются на “K” и заканчиваются на “a” и не содержат пробелов.  
```
select distinct district from address where district like 'K%a' and district not like '% %'; 
``` 

+-----------+  
| district  |  
+-----------+  
| Kanagawa  |  
| Kalmykia  |  
| Kaduna    |  
| Karnataka |  
| Kütahya   |  
| Kerala    |  
| Kitaa     |  
+-----------+  
---

### Задание 2

Получите из таблицы платежей за прокат фильмов информацию по платежам, которые выполнялись в промежуток с 15 июня 2005 года по 18 июня 2005 года **включительно** и стоимость которых превышает 10.00.  

SELECT * FROM payment WHERE payment_date BETWEEN  CAST('2005-06-15' AS DATE) AND CAST('2005-06-19' AS DATE) AND amount > 10;  

+------------+-------------+----------+-----------+--------+---------------------+---------------------+  
| payment_id | customer_id | staff_id | rental_id | amount | payment_date        | last_update         |  
+------------+-------------+----------+-----------+--------+---------------------+---------------------+  
|        908 |          33 |        1 |      1301 |  10.99 | 2005-06-15 09:46:33 | 2006-02-15 22:12:36 |  
|       7017 |         260 |        1 |      2091 |  10.99 | 2005-06-17 18:09:04 | 2006-02-15 22:14:58 |  
|       8272 |         305 |        1 |      2166 |  11.99 | 2005-06-17 23:51:21 | 2006-02-15 22:15:47 |  
|      12888 |         477 |        1 |      2306 |  10.99 | 2005-06-18 08:33:23 | 2006-02-15 22:19:46 |  
|      13892 |         516 |        1 |      1718 |  10.99 | 2005-06-16 14:52:02 | 2006-02-15 22:20:47 |  
|      14620 |         544 |        2 |      1434 |  10.99 | 2005-06-15 18:30:46 | 2006-02-15 22:21:35 |  
|      15313 |         572 |        2 |      1889 |  10.99 | 2005-06-17 04:05:12 | 2006-02-15 22:22:22 |  
+------------+-------------+----------+-----------+--------+---------------------+---------------------+  


### Задание 3

Получите последние пять аренд фильмов.  
SELECT * FROM rental ORDER by rental_date DESC LIMIT 5;  
+-----------+---------------------+--------------+-------------+-------------+----------+---------------------+  
| rental_id | rental_date         | inventory_id | customer_id | return_date | staff_id | last_update         |  
+-----------+---------------------+--------------+-------------+-------------+----------+---------------------+  
|     11739 | 2006-02-14 15:16:03 |         4568 |         373 | NULL        |        2 | 2006-02-15 21:30:53 |  
|     14616 | 2006-02-14 15:16:03 |         4537 |         532 | NULL        |        1 | 2006-02-15 21:30:53 |  
|     11676 | 2006-02-14 15:16:03 |         4496 |         216 | NULL        |        2 | 2006-02-15 21:30:53 |  
|     15966 | 2006-02-14 15:16:03 |         4472 |         374 | NULL        |        1 | 2006-02-15 21:30:53 |  
|     13486 | 2006-02-14 15:16:03 |         4460 |         274 | NULL        |        1 | 2006-02-15 21:30:53 |  
+-----------+---------------------+--------------+-------------+-------------+----------+---------------------+  


### Задание 4

Одним запросом получите активных покупателей, имена которых Kelly или Willie. 

Сформируйте вывод в результат таким образом:
- все буквы в фамилии и имени из верхнего регистра переведите в нижний регистр,
- замените буквы 'll' в именах на 'pp'.  

SELECT LOWER(REPLACE(first_name, 'L', 'p')), LOWER(last_name) FROM customer WHERE first_name LIKE 'Willie' OR first_name  LIKE 'Kelly';  

+--------------------------------------+------------------+  
| LOWER(REPLACE(first_name, 'L', 'p')) | LOWER(last_name) |  
+--------------------------------------+------------------+  
| keppy                                | torres           |  
| wippie                               | howell           |  
| wippie                               | markham          |  
| keppy                                | knott            |  
+--------------------------------------+------------------+  


## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 5*

Выведите Email каждого покупателя, разделив значение Email на две отдельных колонки: в первой колонке должно быть значение, указанное до @, во второй — значение, указанное после @.  

SELECT email, SUBSTRING_INDEX(email , '@', 1), SUBSTRING_INDEX(email , '@', -1) FROM customer;  

+------------------------------------------+---------------------------------+----------------------------------+  
| email                                    | SUBSTRING_INDEX(email , '@', 1) | SUBSTRING_INDEX(email , '@', -1) |  
+------------------------------------------+---------------------------------+----------------------------------+  
| MARY.SMITH@sakilacustomer.org            | MARY.SMITH                      | sakilacustomer.org               |  
| PATRICIA.JOHNSON@sakilacustomer.org      | PATRICIA.JOHNSON                | sakilacustomer.org               |  
| LINDA.WILLIAMS@sakilacustomer.org        | LINDA.WILLIAMS                  | sakilacustomer.org               |  
| BARBARA.JONES@sakilacustomer.org         | BARBARA.JONES                   | sakilacustomer.org               |  
| ELIZABETH.BROWN@sakilacustomer.org       | ELIZABETH.BROWN                 | sakilacustomer.org               |  


### Задание 6*

Доработайте запрос из предыдущего задания, скорректируйте значения в новых колонках: первая буква должна быть заглавной, остальные — строчными.  

SELECT email  , SUBSTRING_INDEX(email  , '@', 1), CONCAT ( LEFT(UPPER(SUBSTRING_INDEX(email  , '@', 1)), 1), LOWER(SUBSTR((SUBSTRING_INDEX(email , '@',1)),2))) as '1' , SUBSTRING_INDEX(email  , '@', -1) , CONCAT(LEFT(UPPER(SUBSTRING_INDEX(email  , '@', -1)), 1), LOWER(SUBSTR((SUBSTRING_INDEX(email , '@',-1)),2))) as '2' FROM customer c;  

+------------------------------------------+----------------------------------+-----------------------+-----------------------------------+--------------------+  
| email                                    | SUBSTRING_INDEX(email  , '@', 1) | 1                     | SUBSTRING_INDEX(email  , '@', -1) | 2                  |  
+------------------------------------------+----------------------------------+-----------------------+-----------------------------------+--------------------+  
| MARY.SMITH@sakilacustomer.org            | MARY.SMITH                       | Mary.smith            | sakilacustomer.org                | Sakilacustomer.org |  
| PATRICIA.JOHNSON@sakilacustomer.org      | PATRICIA.JOHNSON                 | Patricia.johnson      | sakilacustomer.org                | Sakilacustomer.org |  
| LINDA.WILLIAMS@sakilacustomer.org        | LINDA.WILLIAMS                   | Linda.williams        | sakilacustomer.org                | Sakilacustomer.org |  
| BARBARA.JONES@sakilacustomer.org         | BARBARA.JONES                    | Barbara.jones         | sakilacustomer.org                | Sakilacustomer.org |  
| ELIZABETH.BROWN@sakilacustomer.org       | ELIZABETH.BROWN                  | Elizabeth.brown       | sakilacustomer.org                | Sakilacustomer.org |  
