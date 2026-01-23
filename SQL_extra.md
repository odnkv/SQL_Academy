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

<img width="278" height="242" alt="image" src="https://github.com/user-attachments/assets/4976628b-b1d1-4b1d-b2ad-7d61a02bda5b" />

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

<img width="299" height="204" alt="image" src="https://github.com/user-attachments/assets/e0568f9d-aaf3-42da-8f7d-8ae2d4bd6a88" />

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

<img width="330" height="194" alt="image" src="https://github.com/user-attachments/assets/0b095089-7ebe-4223-9f2e-e7ab5ce84132" />

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

#### Задание #114
##### Вторые пилоты в Нью-Йорк
##### Т-Банк
Напишите запрос, который выведет имена пилотов, которые в качестве второго пилота (second_pilot_id) в августе 2023 года летали в New York
Поля в результирующей таблице:
`name`

<img width="480" height="144" alt="image" src="https://github.com/user-attachments/assets/fdf5d154-b33e-4087-8b21-bb54ecf94022" />

```sql

SELECT name
FROM Flights AS f
	JOIN Pilots AS p ON f.second_pilot_id = p.pilot_id
WHERE destination = 'New York'
	AND flight_date BETWEEN '2023-08-01 00:00:00' AND '2023-09-01 00:00:00'
```

#### Задание #123
##### Количество задач у сотрудника
##### Finstar Financial Group
Необходимо написать SQL-запрос, который покажет всех сотрудников, у кого в работе менее трех задач. Результат предоставить в виде: имя сотрудника, количество задач в работе.
Поля в результирующей таблице:
`emp_name`
`task_count`

<img width="468" height="140" alt="image" src="https://github.com/user-attachments/assets/e3f21236-6a74-45f3-98ac-9e247ec54c03" />

```sql

SELECT e.emp_name,
	COUNT(t.id) AS task_count
FROM Employee AS e
	LEFT JOIN Tasks AS t ON e.id = t.assignee_id
GROUP BY e.id,
	emp_name
HAVING COUNT(t.id) < 3

```

#### Задание #125
##### Произведения, издававшиеся более 5 раз
##### Cian
Дана база данных автоматизирующая работу библиотеки. В таблице Books хранится информация о произведениях, в таблице BookEditions - информация об изданиях этих произведений (одно произведение может издаваться много раз в разные годы).

Найти произведения, которые издавались более 5 раз. В качестве результата вывести название произведения (title).
Поля в результирующей таблице:
`title`

<img width="526" height="425" alt="image" src="https://github.com/user-attachments/assets/f02e20d7-9352-48f0-83c0-d7356d9d0c1c" />

```sql

SELECT b.title
FROM Books AS b
	JOIN BookEditions AS be ON b.id = be.book_id
GROUP BY b.title
HAVING COUNT(*) > 5

```

#### Задание #129
##### Номера счетов компаний с SBER в названии
##### GlowByte
Дана база данных с информацией о компаниях и их счетах. В таблице Company хранится информация о компаниях, в таблице Contract - информация о счетах, в таблице Company_contract - связи между компаниями и их счетами.

Найти номера счетов (contract_number) по компаниям, в названии которых (company_name) встречается 'SBER'.
Поля в результирующей таблице:
`contract_number`

<img width="474" height="262" alt="image" src="https://github.com/user-attachments/assets/d1c13b52-2e66-4c53-8035-12b4b4a0114d" />

```sql

SELECT contract_number
FROM Company AS c
	JOIN Company_contract AS cc ON c.company_id = cc.company_id
	JOIN Contract AS ctc ON cc.contract_id = ctc.contract_id
WHERE company_name LIKE 'SBER%'

```

#### Задание #131
##### Количество заказов каждого зарегистрированного клиента
##### Facebook
Дана база данных с информацией о покупках и зарегистрированных клиентах. В таблице purchases хранится информация о покупках, в таблице `registered_customers` - информация о зарегистрированных клиентах.

Напишите запрос для определения количества заказов, размещенных каждым зарегистрированным клиентом. Выведите customer_id и общее количество заказов, размещенных каждым клиентом.
Поля в результирующей таблице:
`customer_id`
`total_orders`

<img width="488" height="208" alt="image" src="https://github.com/user-attachments/assets/e8f1149b-5283-49e3-9ea3-a13406ea74cd" />

```sql

SELECT rc.customer_id,
	COUNT(purchase_id) AS total_orders
FROM registered_customers AS rc
	LEFT JOIN purchases AS p ON rc.customer_id = p.customer_id
GROUP BY rc.customer_id

```

#### Задание #133
#### Проекты, которые никогда не брались в работу
##### Rostelecom
Дана база данных с информацией о разработчиках и проектах. В таблице `Developers` хранится информация о разработчиках, в таблице `Projects` - информация о проектах, в таблице `ProjectHistory` - история работы разработчиков над проектами.

Вывести все проекты (name), которые никогда не брались в работу разработчиками.
Поля в результирующей таблице:
`name`

<img width="477" height="377" alt="image" src="https://github.com/user-attachments/assets/bad1454a-96ff-4e06-bde4-f5aead763834" />

```sql

SELECT name
FROM Projects AS p
	LEFT JOIN ProjectHistory AS ph ON p.id = ph.project_id
WHERE project_id IS NULL

```

#### Задание #139
##### Большие страны
##### Amazon
Страна считается большой, если:
её площадь составляет не менее 3 миллионов км² (3000000), или
её население составляет не менее 25 миллионов человек (25000000).
Напишите запрос для поиска названия (name), населения (population) и площади (area) больших стран.

Поля в результирующей таблице:
`name`
`population`
`area`

<img width="296" height="236" alt="image" src="https://github.com/user-attachments/assets/2e3c39ad-8709-4143-a34b-493aaf9b56b3" />

```sql

SELECT name,
	population,
	area
FROM World AS w
WHERE population >= '25000000'
	OR area >= '3000000'

```
