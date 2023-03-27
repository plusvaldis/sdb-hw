 # Домашнее задание к занятию "Реляционные базы данных: SQL. Часть 2» - Черепанов Владислав"

### Задание 1.

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина,
- город нахождения магазина,
- количество пользователей, закрепленных в этом магазине.
### Ответ
```
SELECT concat(s.first_name  , ' ', s.last_name) as Имя , c.city,  count(c2.customer_id) as Количество FROM staff s JOIN address a  ON s.address_id = a.address_id JOIN city c  ON a.city_id = c.city_id JOIN store s2 ON s2.store_id = s.store_id JOIN customer c2 ON s2.store_id = c2.store_id GROUP BY s.first_name , s.last_name , c.city HAVING Количество > 300;
```
![Скриншот-1](https://github.com/plusvaldis/sdb-hw/blob/main/12.04-hw/img/Screenshot_1.png)
---

### Задание 2.

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.
### Ответ
```
SELECT  count(`length`) FROM film WHERE `length` > (SELECT avg(`length`)FROM film );
```  

![Скриншот-2](https://github.com/plusvaldis/sdb-hw/blob/main/12.04-hw/img/Screenshot_2.png)
---

### Задание 3.

Получите информацию, за какой месяц была получена наибольшая сумма платежей и добавьте информацию по количеству аренд за этот месяц.
### Ответ
```
SELECT DATE_FORMAT(p.payment_date, '%Y-%M') AS Дата , (sum(p.amount )) AS Сумма , count((p.rental_id )) AS Аренд FROM payment p GROUP BY Дата ORDER BY Сумма DESC LIMIT 1;
```  

![Скриншот-3](https://github.com/plusvaldis/sdb-hw/blob/main/12.04-hw/img/Screenshot_3.png)
---