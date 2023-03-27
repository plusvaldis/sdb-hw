 # Домашнее задание к занятию "12.2. «SQL. Часть 1» - Черепанов Владислав"





Задание можно выполнить как в любом IDE, так и в командной строке.

Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1

Получите уникальные названия районов из таблицы с адресами, которые начинаются на “K” и заканчиваются на “a” и не содержат пробелов.  
```
select distinct district from address where district like 'K%a' and district not like '% %'; 
```  
![Скриншот-1](https://github.com/plusvaldis/sdb-hw/blob/main/12.03-hw/img/Screenshot_1.png)
---


### Задание 2

Получите из таблицы платежей за прокат фильмов информацию по платежам, которые выполнялись в промежуток с 15 июня 2005 года по 18 июня 2005 года **включительно** и стоимость которых превышает 10.00.  
```
select * from payment where payment_date between '2005-06-15 00:00:00' and '2005-06-18 23:59:59' and amount > 10 order by amount desc; 
```  
![Скриншот-2](https://github.com/plusvaldis/sdb-hw/blob/main/12.03-hw/img/Screenshot_2.png)
---

### Задание 3

Получите последние пять аренд фильмов.  `
```
SELECT * FROM rental ORDER by rental_date DESC LIMIT 5;  
```  
![Скриншот-3](https://github.com/plusvaldis/sdb-hw/blob/main/12.03-hw/img/Screenshot_3.png)
---

### Задание 4

Одним запросом получите активных покупателей, имена которых Kelly или Willie. 

Сформируйте вывод в результат таким образом:
- все буквы в фамилии и имени из верхнего регистра переведите в нижний регистр,
- замените буквы 'll' в именах на 'pp'.  
```
SELECT LOWER(REPLACE(first_name, 'L', 'p')), LOWER(last_name) FROM customer WHERE first_name LIKE 'Willie' OR first_name  LIKE 'Kelly';  
```  
![Скриншот-4](https://github.com/plusvaldis/sdb-hw/blob/main/12.03-hw/img/Screenshot_4.png)
---


## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 5*

Выведите Email каждого покупателя, разделив значение Email на две отдельных колонки: в первой колонке должно быть значение, указанное до @, во второй — значение, указанное после @.  
```
SELECT email, SUBSTRING_INDEX(email , '@', 1), SUBSTRING_INDEX(email , '@', -1) FROM customer;  
```  
![Скриншот-5](https://github.com/plusvaldis/sdb-hw/blob/main/12.03-hw/img/Screenshot_5.png)
---

### Задание 6*

Доработайте запрос из предыдущего задания, скорректируйте значения в новых колонках: первая буква должна быть заглавной, остальные — строчными.  
```
SELECT email  , SUBSTRING_INDEX(email  , '@', 1), CONCAT ( LEFT(UPPER(SUBSTRING_INDEX(email  , '@', 1)), 1), LOWER(SUBSTR((SUBSTRING_INDEX(email , '@',1)),2))) as '1' , SUBSTRING_INDEX(email  , '@', -1) , CONCAT(LEFT(UPPER(SUBSTRING_INDEX(email  , '@', -1)), 1), LOWER(SUBSTR((SUBSTRING_INDEX(email , '@',-1)),2))) as '2' FROM customer c;  
```  
![Скриншот-6](https://github.com/plusvaldis/sdb-hw/blob/main/12.03-hw/img/Screenshot_6.png)
---
