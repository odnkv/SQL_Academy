<img width="502" height="146" alt="image" src="https://github.com/user-attachments/assets/6886b29a-d8ce-4371-8900-a442979f7495" />

#### Задание #1
##### Имена всех людей
Вывести имена всех людей, которые есть в базе данных авиакомпаний
Поля в результирующей таблице:
`name`

```sql

SELECT name
FROM Passenger

```

#### Задание #2
##### Названия всех авиакомпаний
Вывести названия всеx авиакомпаний
Поля в результирующей таблице:
`name`

```sql

SELECT name
FROM Company

```

#### Задание #3
##### Рейсы из Москвы
Вывести все рейсы, совершенные из Москвы
Поля в результирующей таблице:
`*`

```sql

SELECT *
FROM Trip
WHERE town_from = 'Moscow'

```

#### Задание #4
##### Имена, заканчивающиеся на "man"
Вывести имена людей, которые заканчиваются на "man"
Поля в результирующей таблице:
`name`

```sql

SELECT name
FROM Passenger
WHERE name LIKE '%man'

```

#### Задание #5
##### Количество рейсов на TU-134
Вывести количество рейсов, совершенных на TU-134
Используйте конструкцию "as count" для агрегатной функции подсчета количества рейсов. Это необходимо для корректной проверки.
Поля в результирующей таблице:
`count`

```sql

SELECT COUNT(plane)
FROM Trip
WHERE plane = 'TU-134'

```

#### Задание #6
##### Компании, летавшие на Boeing
Какие компании совершали перелеты на Boeing
Поля в результирующей таблице:
`name`

```sql

SELECT DISTINCT name
FROM Trip
	JOIN Company ON trip.company = company.id
WHERE plane = 'Boeing'

```

#### Задание #7
##### Самолеты, летящие в Москву
Вывести все названия самолётов, на которых можно улететь в Москву (Moscow)
Поля в результирующей таблице:
plane

```sql

SELECT DISTINCT plane
FROM Trip
WHERE town_to = 'Moscow'

```

#### Задание #8
##### Полёты из Парижа
В какие города можно улететь из Парижа (Paris) и сколько времени это займёт?
Используйте конструкцию "as flight_time" для вывода необходимого времени. Это необходимо для корректной проверки.
Формат для вывода времени: HH:MM:SS
Поля в результирующей таблице:
`town_to
flight_time`

```sql

SELECT town_to,
	(time_in - time_out) AS flight_time
FROM Trip
WHERE town_from = 'Paris'

```

#### Задание #9
##### Компании с рейсами из Владивостока
Какие компании организуют перелеты из Владивостока (Vladivostok)?
Поля в результирующей таблице:
`name`

```sql

SELECT name
FROM Trip AS t
	JOIN Company AS C ON c.id = t.company
WHERE town_from = 'Vladivostok'

```

#### Задание #10
##### Вылеты в определенное время
Вывести вылеты, совершенные с 10 ч. по 14 ч. 1 января 1900 г.
Поля в результирующей таблице:
`*`

```sql

SELECT *
FROM Trip
WHERE time_out BETWEEN '1900-01-01 10:00:00' AND '1900-01-01 14:00:00'

```

#### Задание #11
##### Пассажиры с самым длинным ФИО
Выведите пассажиров с самым длинным ФИО. Пробелы, дефисы и точки считаются частью имени.
Поля в результирующей таблице:
`name`

```sql

SELECT name
FROM Passenger
WHERE LENGTH(name) = (
		SELECT MAX(LENGTH(name))
		FROM passenger
	)

```

#### Задание #12
##### Количество пассажиров на рейсах
Выведите идентификаторы всех рейсов и количество пассажиров на них. Обратите внимание, что на каких-то рейсах пассажиров может не быть. В этом случае выведите число "0".
Используйте конструкцию "as count" для агрегатной функции подсчета количества пассажиров. Это необходимо для корректной проверки.
Поля в результирующей таблице:
`id`
`count`

```sql

SELECT t.id,
	COUNT(pt.passenger) AS COUNT
FROM Trip AS t
	LEFT JOIN Pass_in_trip AS pt ON t.id = pt.trip
GROUP BY t.id

```

#### Задание #13
##### Полные тёзки
Вывести имена людей, у которых есть полный тёзка среди пассажиров
Поля в результирующей таблице:
`name`

```sql

SELECT name
FROM Passenger
GROUP BY name
HAVING COUNT(name) >= 2

```

#### Задание #14
##### Города, которые посетил Bruce Willis
В какие города летал Bruce Willis
Поля в результирующей таблице:
`town_to`

```sql

SELECT town_to
FROM Passenger AS p
	JOIN Pass_in_trip AS pt ON p.id = pt.passenger
	JOIN Trip AS t ON pt.trip = t.id
WHERE name = 'Bruce Willis'

```

#### Задание #15
##### Прибытие Steve Martin в Лондон
Выведите идентификатор пассажира Стив Мартин (Steve Martin) и дату и время его прилёта в Лондон (London)
Поля в результирующей таблице:
`id`
`time_in`

```sql

SELECT p.id,
	t.time_in
FROM Passenger AS p
	JOIN Pass_in_trip AS pt ON p.id = pt.passenger
	JOIN Trip AS t ON pt.trip = t.id
WHERE p.name = 'Steve Martin'
	AND t.town_to = 'London'

```

#### Задание #16
##### Сортировка пассажиров по количеству полетов
Вывести отсортированный по количеству перелетов (по убыванию) и имени (по возрастанию) список пассажиров, совершивших хотя бы 1 полет.
Используйте конструкцию "as count" для агрегатной функции подсчета количества перелетов. Это необходимо для корректной проверки.
Поля в результирующей таблице:
`name`
`count`

```sql

SELECT name,
	COUNT(trip) AS count
FROM Passenger AS p
	JOIN Pass_in_trip AS pt ON p.id = pt.passenger
GROUP BY name
ORDER BY count DESC,
	name ASC

```


<img width="488" height="151" alt="image" src="https://github.com/user-attachments/assets/a39ce52c-b335-4530-9fda-4e1e3515e908" />

#### Задание #17
##### Траты членов семьи в 2005 году
Определить, сколько потратил в 2005 году каждый из членов семьи. В результирующей выборке не выводите тех членов семьи, которые ничего не потратили.
Используйте конструкцию "as costs" для отображения затраченной суммы членом семьи. Это необходимо для корректной проверки.
Поля в результирующей таблице:
`member_name`
`status`
`costs`

```sql

SELECT member_name,
	status,
	SUM(p.amount * p.unit_price) AS costs
FROM FamilyMembers AS f
	JOIN Payments AS p ON f.member_id = p.family_member
WHERE DATE_PART('year', date) = '2005'
GROUP BY member_name,
	status

```

#### Задание #18
##### Самый старший человек
Выведите имя самого старшего человека. Если таких несколько, то выведите их всех.
Поля в результирующей таблице:
`member_name`

```sql

SELECT member_name
FROM FamilyMembers
ORDER BY birthday
LIMIT 1

```

#### Задание #19
##### Кто покупал картошку
Определить, кто из членов семьи покупал картошку (potato)
Поля в результирующей таблице:
`status`

```sql

SELECT status
FROM FamilyMembers AS fm
	JOIN Payments AS p ON fm.member_id = p.family_member
	JOIN Goods AS g ON p.good = g.good_id
WHERE good_name = 'potato'
GROUP BY status

```

#### Задание #20
##### Траты на развлечения
Сколько и кто из семьи потратил на развлечения (entertainment). Вывести статус в семье, имя, сумму
Используйте конструкцию "as costs" для отображения затраченной суммы членом семьи. Это необходимо для корректной проверки.
Поля в результирующей таблице:
`status`
`member_name`
`costs`

```sql

SELECT status,
	member_name,
	SUM(amount * unit_price) AS costs
FROM FamilyMembers AS fm
	JOIN Payments AS p ON fm.member_id = p.family_member
	JOIN Goods AS g ON p.good = g.good_id
	JOIN GoodTypes AS gt ON g.type = gt.good_type_id
WHERE good_type_name = 'entertainment'
GROUP BY status,
	member_name

```

#### Задание #21
##### Товары, купленные более одного раза
Определить товары, которые покупали более 1 раза
Поля в результирующей таблице:
`good_name`

```sql

SELECT g.good_name
FROM Goods AS g
	JOIN Payments AS p ON g.good_id = p.good
GROUP BY g.good_name
HAVING COUNT(p.amount) > 1

```

#### Задание #22
##### Имена всех матерей
Найти имена всех матерей (mother)
Поля в результирующей таблице:
`member_name`


```sql

SELECT member_name
FROM FamilyMembers
WHERE status = 'mother'

```

#### Задание #23
##### Самый дорогой деликатес
Найдите самый дорогой деликатес (delicacies) и выведите его цену
Поля в результирующей таблице:
`good_name`
`unit_price`


```sql

SELECT g.good_name,
	p.unit_price
FROM Goods AS g
	JOIN GoodTypes AS gt ON g.type = gt.good_type_id
	JOIN Payments AS p ON g.good_id = p.good
WHERE good_type_name = 'delicacies'
ORDER BY unit_price DESC
LIMIT 1

```

#### Задание #24
##### Кто и сколько потратил в июне 2005 года
Определить, кто и сколько потратил в июне 2005
Используйте конструкцию "as costs" для отображения затраченной суммы членом семьи. Это необходимо для корректной проверки.
Поля в результирующей таблице:
`member_name`
`costs`


```sql

SELECT member_name,
	SUM(amount * unit_price) AS costs
FROM FamilyMembers AS fm
	JOIN Payments AS p ON fm.member_id = p.family_member
WHERE DATE_PART('year', date) = 2005
	AND DATE_PART('month', date) = 06
GROUP by member_name

```

#### Задание #25
##### Товары, не купленные в 2005 году
Определить, какие товары не покупались в 2005 году
Все доступные к покупке продукты находятся в таблице Goods
Поля в результирующей таблице:
`good_name`


```sql

SELECT good_name
FROM Goods
WHERE good_id NOT IN (
		SELECT DISTINCT good
		FROM Payments
		WHERE DATE_PART('year', date) = 2005
	)

```
