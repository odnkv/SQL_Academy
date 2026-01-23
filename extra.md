#### Задание #93
##### Средний возраст покупателей Smartwatch
##### Альфа-Банк
Какой средний возраст клиентов, купивших Smartwatch (использовать наименование товара `product.name`) в 2024 году?
Поля в результирующей таблице:
`average_age`

<img width="503" height="217" alt="image" src="https://github.com/user-attachments/assets/bff73827-3cec-427e-9936-98fc04260b47" />

```sql

SELECT AVG(DISTINCT c.age) AS average_age
FROM Product AS p
	JOIN Purchase AS ps ON p.product_key = ps.product_key
	JOIN Customer AS c ON ps.customer_key = c.customer_key
WHERE p.name = 'Smartwatch'
	AND DATE_PART('year', ps.date) = 2024

```

#### Задание #94
##### Покупатели Laptop и Monitor в марте
##### Альфа-Банк
Вывести имена покупателей, каждый из которых приобрёл Laptop и Monitor (использовать наименование товара product.name) в марте 2024 года?
Поля в результирующей таблице:
`name`

<img width="503" height="217" alt="image" src="https://github.com/user-attachments/assets/bff73827-3cec-427e-9936-98fc04260b47" />

```sql

SELECT DISTINCT c.name
FROM Product AS p
	JOIN Purchase AS ps ON p.product_key = ps.product_key
	JOIN Customer AS c ON ps.customer_key = c.customer_key
WHERE DATE_PART('year', date) = 2024
	AND DATE_PART('month', date) = 03
	AND p.name IN ('Laptop', 'Monitor')
GROUP BY c.name
HAVING COUNT(DISTINCT p.name) = 2;

```

#### Задание #97
##### Работающие склады по городам
##### Самокат
Посчитать количество работающих складов на текущую дату по каждому городу. Вывести только те города, у которых количество складов более 80. Данные на выходе - город, количество складов.
Поля в результирующей таблице:
`city`
`warehouse_count`

<img width="556" height="484" alt="image" src="https://github.com/user-attachments/assets/4976628b-b1d1-4b1d-b2ad-7d61a02bda5b" />

```sql

SELECT city,
	COUNT(warehouse_id) AS warehouse_count
FROM Warehouses
WHERE date_close IS NULL
GROUP BY city
HAVING COUNT(warehouse_id) > 80

```

#### Задание #99
##### Доход с женской аудитории
##### VK
Посчитай доход с женской аудитории (доход = сумма(price * items)). Обратите внимание, что в таблице женская аудитория имеет поле user_gender «female» или «f».
Поля в результирующей таблице:
`income_from_female`

<img width="598" height="408" alt="image" src="https://github.com/user-attachments/assets/e0568f9d-aaf3-42da-8f7d-8ae2d4bd6a88" />

```sql

SELECT SUM(items * price) AS income_from_female
FROM Purchases
WHERE user_gender = 'f'
	or user_gender = 'female'

```

#### Задание #101
##### Первый заказ пользователя
##### VK
Выведи для каждого пользователя первое наименование, которое он заказал (первое по времени транзакции).
Поля в результирующей таблице:
`user_id`
`item`

<img width="660" height="388" alt="image" src="https://github.com/user-attachments/assets/0b095089-7ebe-4223-9f2e-e7ab5ce84132" />

```sql

SELECT user_id,
	item
FROM Transactions
WHERE transaction_ts IN (
		SELECT min(transaction_ts)
		FROM Transactions
		GROUP BY user_id
	)

```

#### Задание #103
##### Зарплата выше руководителя
##### Сбербанк
Вывести список имён сотрудников, получающих большую заработную плату, чем у непосредственного руководителя.
Поля в результирующей таблице:
`name`

<img width="509" height="225" alt="image" src="https://github.com/user-attachments/assets/cf956f06-c4eb-4d4f-b73f-4131571c71a8" />

```sql

SELECT Employee.name
FROM Employee
	INNER JOIN Employee AS chief_name ON Employee.chief_id = chief_name.id
WHERE Employee.salary > chief_name.salary;

```

#### Задание #109
##### Страна города Зальцбург
##### ДомКлик
Выведите название страны, где находится город «Salzburg»
Поля в результирующей таблице:
`country_name`

<img width="501" height="239" alt="image" src="https://github.com/user-attachments/assets/b296a6f6-a1a6-4559-a0d6-ac9ea00bd4f1" />


```sql

SELECT cs.name AS country_name
FROM Cities AS c
	JOIN Regions AS r ON c.regionid = r.id
	JOIN Countries AS cs ON r.countryid = cs.id
WHERE c.name = 'Salzburg'

```

#### Задание #111
##### Население каждого региона
##### ДомКлик
Посчитайте население каждого региона. В качестве результата выведите название региона и его численность населения.
Поля в результирующей таблице:
`region_name`
`total_population`


<img width="501" height="239" alt="image" src="https://github.com/user-attachments/assets/b296a6f6-a1a6-4559-a0d6-ac9ea00bd4f1" />


```sql

SELECT r.name AS region_name,
	SUM(population) AS total_population
FROM Cities AS c
	JOIN Regions AS r ON c.regionid = r.id
GROUP BY region_name

```
