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

#### Задание #26
##### Группы товаров, не купленные в 2005 году
Определить группы товаров, которые не приобретались в 2005 году
Поля в результирующей таблице:
`good_type_name`


```sql

SELECT good_type_name
FROM GoodTypes
WHERE good_type_id NOT IN (
		SELECT type
		FROM Goods AS g
			JOIN Payments AS p ON g.good_id = p.good
		WHERE DATE_PART('year', date) = 2005
	)

```

#### Задание #27
##### Траты по группам товаров в 2005 году
Узнайте, сколько было потрачено на каждую из групп товаров в 2005 году. Выведите название группы и потраченную на неё сумму. Если потраченная сумма равна нулю, т.е. товары из этой группы не покупались в 2005 году, то не выводите её.
Используйте конструкцию "as costs" для отображения затраченной суммы на конкретную группу товаров. Это необходимо для корректной проверки.
Поля в результирующей таблице:
`good_type_name`
`costs`


```sql

SELECT good_type_name,
	SUM(amount * unit_price) AS costs
FROM Payments AS p
	JOIN Goods AS g ON p.good = g.good_id
	JOIN GoodTypes AS gt ON g.type = gt.good_type_id
WHERE DATE_PART('year', date) = 2005
GROUP BY good_type_name

```

#### Задание #28
##### Рейсы из Ростова в Москву
Сколько рейсов совершили авиакомпании из Ростова (Rostov) в Москву (Moscow) ?
Используйте конструкцию "as count" для агрегатной функции подсчета количества рейсов. Это необходимо для корректной проверки.
Поля в результирующей таблице:
`count`


```sql

SELECT count(id)
FROM Trip AS t
WHERE town_from = 'Rostov'
	AND town_to = 'Moscow'

```

#### Задание #29
##### Имена пассажиров, летящих в Москву
Выведите имена пассажиров, улетевших в Москву (Moscow) на самолете TU-134. В ответе не должно быть дубликатов.
Поля в результирующей таблице:
`name`


```sql

SELECT DISTINCT name
FROM Trip AS t
	JOIN Pass_in_trip AS pt ON t.id = pt.trip
	JOIN Passenger AS p ON pt.passenger = p.id
WHERE town_to = 'Moscow'
	AND plane = 'TU-134'

```

#### Задание #30
##### Нагруженность рейсов
Вывести количество занятых мест по каждому рейсу из таблицы Pass_in_trip, отсортировав результат по убыванию количества занятых мест.
Используйте конструкцию "as count" для агрегатной функции подсчета числа пассажиров на рейсе. Это необходимо для корректной проверки.
Поля в результирующей таблице:
`trip`
`count`

```sql

SELECT trip,
	COUNT(place) as count
FROM Pass_in_trip AS pt
GROUP BY trip
ORDER BY COUNT(place) DESC

```

#### Задание #31
##### Члены семьи Quincey
Вывести всех членов семьи с фамилией Quincey.
Поля в результирующей таблице:
`*`

```sql

SELECT *
FROM FamilyMembers
WHERE member_name LIKE '%Quincey'

```

#### Задание #32
##### Средний возраст людей
Вывести средний возраст людей (в годах), хранящихся в базе данных. Результат округлите до целого в меньшую сторону.
Используйте конструкцию "as age" для агрегатной функции подсчета среднего возраста. Это необходимо для корректной проверки.
Поля в результирующей таблице:
`age`

```sql

SELECT FLOOR(
		AVG(
			EXTRACT(
				YEAR
				FROM AGE(CURRENT_DATE, birthday)
			)
		)
	) AS age
FROM FamilyMembers

```

#### Задание #33
##### Средняя цена икры
Найдите среднюю цену икры на основе данных, хранящихся в таблице Payments. В базе данных хранятся данные о покупках красной (red caviar) и черной икры (black caviar). В ответе должна быть одна строка со средней ценой всей купленной когда-либо икры.
Используйте конструкцию "as cost" для агрегатной функции подсчета средней цены икры. Это необходимо для корректной проверки.
Поля в результирующей таблице:
`cost`

```sql

SELECT AVG(unit_price) AS cost
FROM Goods AS g
	JOIN Payments AS p ON g.good_id = p.good
WHERE good_name = 'red caviar'
	OR good_name = 'black caviar'

```

#### Задание #34
##### Количество 10-х классов
Сколько всего 10-ых классов
Используйте конструкцию "as count" для агрегатной функции подсчета количества классов. Это необходимо для корректной проверки.
Поля в результирующей таблице:
`count`

```sql

SELECT COUNT(name) AS count
FROM Class
WHERE name LIKE '10%'

```

#### Задание #35
##### Кабинеты, использованные 2 сентября 2019
Сколько различных кабинетов школы использовались 2 сентября 2019 года для проведения занятий?
Используйте конструкцию "as count" для агрегатной функции подсчета количества различных кабинетов. Это необходимо для корректной проверки.
Поля в результирующей таблице:
`count`

```sql

SELECT COUNT(DISTINCT classroom) AS count
FROM Schedule
WHERE date = '2019-09-02'

```

#### Задание #36
##### Обучающиеся, живущие на улице Пушкина
Выведите информацию об обучающихся, живущих на улице Пушкина (ul. Pushkina)?
Поля в результирующей таблице:
`*`

```sql

SELECT *
FROM Student
WHERE address LIKE 'ul. Pushkina%'

```

#### Задание #37
##### Возраст самого молодого обучающегося
Сколько лет самому молодому обучающемуся ?
Используйте конструкцию "as year" для указания возраста учащегося. Это необходимо для корректной проверки.
Поля в результирующей таблице:
`year`

```sql

SELECT DATE_PART('year', AGE(CURRENT_DATE, birthday)) AS year
FROM Student
ORDER BY birthday DESC
LIMIT 1

```

#### Задание #38
##### Количество учениц с именем Анна
Сколько учениц с именем Анна (Anna) учится в школе?
Используйте конструкцию "as count" для агрегатной функции подсчета количества учащихся. Это необходимо для корректной проверки.
Поля в результирующей таблице:
`count`

```sql

SELECT COUNT(*)
FROM Student
WHERE first_name = 'Anna'

```

#### Задание #39
##### Количество обучающихся в 10 B классе
Сколько обучающихся в 10 B классе ?
Используйте конструкцию "as count" для агрегатной функции подсчета количества учащихся. Это необходимо для корректной проверки.
Поля в результирующей таблице:
`count`

```sql

SELECT COUNT(*) AS count
FROM Class AS c
	JOIN Student_in_class AS sc ON c.id = sc.class
WHERE name = '10 B'

```

#### Задание #40
##### Предметы Ромашкина П.П.
Выведите название предметов, которые преподает Ромашкин П.П. (Romashkin P.P.). Обратите внимание, что в базе данных есть несколько учителей с такой фамилией.
Используйте конструкцию "as subjects" для указания учебных предметов. Это необходимо для корректной проверки.
Поля в результирующей таблице:
`subjects`

```sql

SELECT sb.name AS subjects
FROM Teacher AS t
	JOIN Schedule AS s ON t.id = s.teacher
	JOIN Subject AS sb ON s.subject = sb.id
WHERE last_name = 'Romashkin'
	AND first_name LIKE 'P%'
	AND middle_name LIKE 'P%'

```
