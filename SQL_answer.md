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

#### Задание #41
##### Начало четвёртого занятия
Выясните, во сколько по расписанию начинается четвёртое занятие.
Поля в результирующей таблице:
`start_pair`

```sql

SELECT start_pair
FROM Timepair
WHERE id = 4

```

#### Задание #42
##### Время, проведённое в школе
Сколько времени обучающийся будет находиться в школе, учась со 2-го по 4-ый уч. предмет?
Используйте конструкцию "as time" для указания разницы во времени. Это необходимо для корректной проверки.
Результат должен быть в формате HH:MM:SS
Поля в результирующей таблице:
`time`

```sql

SELECT DISTINCT (
		(
			SELECT end_pair
			FROM Timepair
			WHERE id = 4
		) - (
			SELECT start_pair
			FROM Timepair
			WHERE id = 2
		)
	) AS time
FROM Timepair

```

#### Задание #43
##### Преподаватели физкультуры
Выведите фамилии преподавателей, которые ведут физическую культуру (Physical Culture). Отсортируйте преподавателей по фамилии в алфавитном порядке.
Поля в результирующей таблице:
`last_name`

```sql

SELECT last_name
FROM Subject AS s
	JOIN Schedule AS se ON s.id = se.subject
	JOIN Teacher AS t ON se.teacher = t.id
WHERE name = 'Physical Culture'
ORDER BY last_name ASC

```

#### Задание #44
##### Максимальный возраст в 10 классах
Найдите максимальный возраст (количество лет) среди обучающихся 10 классов на сегодняшний день. Для получения текущих даты и времени используйте функцию NOW().
Используйте конструкцию "as max_year" для указания максимального возраста в годах. Это необходимо для корректной проверки.
Поля в результирующей таблице:
`max_year`

```sql

SELECT EXTRACT(
		YEAR
		FROM AGE(NOW(), birthday)
	) AS max_year
FROM Student AS s
	JOIN Student_in_class AS sc ON s.id = sc.student
	JOIN Class AS c ON sc.class = c.id
WHERE c.name LIKE '10%'
ORDER BY EXTRACT(
		YEAR
		FROM AGE(NOW(), birthday)
	) DESC
LIMIT 1

```

#### Задание #45
##### Самые используемые кабинеты
Какие кабинеты чаще всего использовались для проведения занятий? Выведите те, которые использовались максимальное количество раз.
Поля в результирующей таблице:
`classroom`

```sql

SELECT classroom
FROM Schedule
GROUP BY classroom
HAVING COUNT(classroom) = (
		SELECT COUNT(*) AS count
		FROM Schedule
		GROUP BY classroom
		ORDER BY count DESC
		LIMIT 1
	)

```

#### Задание #46
##### Классы преподавателя Krauze
В каких классах введет занятия преподаватель "Krauze" ?
Поля в результирующей таблице:
`name`

```sql

SELECT DISTINCT c.name
FROM Teacher AS t
	JOIN Schedule AS s ON t.id = s.teacher
	JOIN Class AS c ON s.class = c.id
WHERE last_name = 'Krauze'

```

#### Задание #47
##### Занятия Krauze 30 августа 2019
Сколько занятий провел Krauze 30 августа 2019 г.?
Используйте конструкцию "as count" для агрегатной функции подсчета числа занятий. Это необходимо для корректной проверки.
Поля в результирующей таблице:
`count`

```sql

SELECT COUNT(number_pair)
FROM Teacher AS t
	JOIN Schedule AS s ON t.id = s.teacher
WHERE last_name = 'Krauze'
	AND date = '2019-08-30'

```

#### Задание #48
##### Заполненность классов
Выведите заполненность классов в порядке убывания
Используйте конструкцию "as count" для агрегатной функции подсчета числа учащихся в классах. Это необходимо для корректной проверки.
Поля в результирующей таблице:
`name`
`count`

```sql

SELECT name,
	COUNT(Student) AS count
FROM Class AS c
	JOIN Student_in_class AS sc ON c.id = sc.class
GROUP BY name
ORDER BY count DESC

```

#### Задание #49
##### Процент обучающихся в 10 A классе
Какой процент обучающихся учится в "10 A" классе? Выведите ответ в диапазоне от 0 до 100 с округлением до четырёх знаков после запятой, например, 96.0201.
Используйте конструкцию "as percent" для представления результата вычисления. Это необходимо для корректной проверки.
Поля в результирующей таблице:
`percent`

```sql

SELECT ROUND(
		COUNT(*) * 100.0 / (
			SELECT Count(*)
			FROM Student_in_class
		),
		4
	) AS percent
FROM Student_in_class AS sc
	JOIN Class AS c ON sc.class = c.id
WHERE c.name = '10 A'

```

#### Задание #50
##### Процент родившихся в 2000 году
Какой процент обучающихся родился в 2000 году? Результат округлить до целого в меньшую сторону.
Используйте конструкцию "as percent" для указания процента. Это необходимо для корректной проверки.
Поля в результирующей таблице:
`percent`

```sql

SELECT FLOOR(
		COUNT(*) * 100 / (
			SELECT Count(*)
			FROM Student
		)
	) AS percent
FROM Student
WHERE (DATE_PART('year', birthday) = 2000)

```

#### Задание #51
##### Добавить товар "Cheese"
Добавьте товар с именем "Cheese" и типом "food" в список товаров (Goods).
В качестве первичного ключа (good_id) укажите количество записей в таблице + 1.

```sql

INSERT INTO Goods (good_id, good_name, type)
VALUES (19, 'Cheese', 2)

```

#### Задание #52
##### Добавить тип товара "auto"
Добавьте в список типов товаров (GoodTypes) новый тип "auto".
В качестве первичного ключа (good_type_id) укажите количество записей в таблице + 1.

```sql

INSERT INTO GoodTypes (good_type_id, good_type_name)
VALUES (
		(
			SELECT COUNT(*) + 1
			FROM GoodTypes
		),
		'auto'
	)

```

#### Задание #53
##### Изменить имя на "Andie Anthony"
Измените имя "Andie Quincey" на новое "Andie Anthony".

```sql

UPDATE FamilyMembers
SET member_name = 'Andie Anthony'
WHERE member_name = 'Andie Quincey'

```

#### Задание #54
##### Удалить членов семьи Quincey
Удалить всех членов семьи с фамилией "Quincey".

```sql

DELETE FROM FamilyMembers
WHERE member_name LIKE '%Quincey'

```

#### Задание #55
##### Удалить компании с наименьшим числом рейсов
Удалить компании, совершившие наименьшее количество рейсов.

```sql

DELETE FROM Company
WHERE id IN (
		SELECT company
		FROM Trip
		GROUP BY company
		HAVING COUNT(*) = (
				SELECT COUNT(*) AS count
				FROM Trip
				GROUP BY company
				ORDER BY count
				LIMIT 1
			)
	);

```

#### Задание #56
##### Удалить перелеты из Москвы
Удалить все перелеты, совершенные из Москвы (Moscow).

```sql

DELETE FROM Trip
WHERE town_from = 'Moscow'

```

#### Задание #57
##### Перенести расписание на 30 мин
Перенести расписание всех занятий на 30 мин. вперед.

```sql

UPDATE Timepair
SET start_pair = start_pair + '00:30:00',
	end_pair = end_pair + '00:30:00'

```

#### Задание #58
##### Добавить отзыв от George Clooney
Добавить отзыв с рейтингом 5 на жилье, находящиеся по адресу "11218, Friel Place, New York", от имени "George Clooney"
В качестве первичного ключа (id) укажите количество записей в таблице + 1.
Резервация комнаты, на которую вам нужно оставить отзыв, уже была сделана, нужно лишь ее найти.

```sql

INSERT INTO Reviews (id, reservation_id, rating)
VALUES (
		(
			SELECT COUNT(*) + 1
			FROM Reviews
		),
		(
			SELECT r.id
			FROM Reservations AS r
				JOIN Rooms AS rs ON r.room_id = rs.id
				JOIN Users AS u ON r.user_id = u.id
			WHERE address = '11218, Friel Place, New York'
				AND name = 'George Clooney'
		),
		5
	)

```

#### Задание #59
##### Пользователи с белорусским номером
Вывести пользователей,указавших Белорусский номер телефона ? Телефонный код Белоруссии +375.
Поля в результирующей таблице:
`*`

```sql

SELECT *
FROM Users
WHERE phone_number LIKE '+375%'

```

#### Задание #60
##### Преподаватели в 11-ых классах
Выведите идентификаторы преподавателей, которые хотя бы один раз за всё время преподавали в каждом из одиннадцатых классов.
Поля в результирующей таблице:
`teacher`

```sql

SELECT teacher
FROM Schedule AS s
	JOIN Class AS c ON s.class = c.id
WHERE c.name LIKE '11%'
GROUP BY teacher
HAVING COUNT(DISTINCT name) > 1

```

#### Задание #61
##### Комнаты, зарезервированные на 12-й неделе 2020 года
Выведите список комнат, которые были зарезервированы хотя бы на одни сутки в 12-ую неделю 2020 года. В данной задаче в качестве одной недели примите период из семи дней, первый из которых начинается 1 января 2020 года. Например, первая неделя года — 1–7 января, а третья — 15–21 января.
Поля в результирующей таблице:
`Rooms.*`

```sql

SELECT r.*
FROM Rooms AS r
	JOIN Reservations AS rs ON rs.room_id = r.id
WHERE DATE_PART('year', start_date) = 2020
	AND DATE_PART('week', start_date) = 12
	OR DATE_PART('week', end_date) = 12

```

#### Задание #62
##### Рейтинг доменов 2-го уровня
Вывести в порядке убывания популярности доменные имена 2-го уровня, используемые пользователями для электронной почты. Полученный результат необходимо дополнительно отсортировать по возрастанию названий доменных имён.
Для эл. почты index@gmail.com доменным именем 2-го уровня будет gmail.com.
Поля в результирующей таблице:
`domain`
`count`

```sql

SELECT SPLIT_PART(email, '@', 2) AS domain,
	COUNT(*) AS count
FROM Users
GROUP BY domain
ORDER BY count DESC,
	domain ASC

```

#### Задание #63
##### Сортировка имён обучающихся
Выведите отсортированный список (по возрастанию) фамилий и имен студентов в виде Фамилия.И.
Поля в результирующей таблице:
`name`

```sql

SELECT CONCAT(last_name, '.', LEFT(first_name, 1), '.') AS name
FROM Student
ORDER BY name ASC

```

#### Задание #64
##### Количество бронирований по месяцам
Вывести количество бронирований по каждому месяцу каждого года, в которых было хотя бы 1 бронирование. Результат отсортируйте в порядке возрастания даты бронирования.
Используйте конструкцию "as year", "as month" и "as amount" для вывода года и месяца бронирования, количества таких бронирований соответственно.
Поля в результирующей таблице:
`year`
`month`
`amount`

```sql

SELECT DATE_PART('year', start_date) AS year,
	DATE_PART('month', start_date) AS month,
	COUNT(*) AS amount
FROM Reservations AS r
GROUP BY DATE_PART('year', start_date),
	DATE_PART('month', start_date)
ORDER BY DATE_PART('year', start_date),
	DATE_PART('month', start_date)

```

#### Задание #65
##### Рейтинг арендованных комнат
Необходимо вывести рейтинг для комнат, которые хоть раз арендовали, как среднее значение рейтинга отзывов округленное до целого вниз.
Используйте конструкцию "as rating" для вывода рейтинга.
Поля в результирующей таблице:
`room_id`
`rating`

```sql

SELECT room_id,
	FLOOR(AVG(rating)) AS rating
FROM Reviews AS r
	JOIN Reservations AS rs ON r.reservation_id = rs.id
GROUP BY room_id

```

#### Задание #66
##### Комнаты со всеми удобствами
Вывести список комнат со всеми удобствами (наличие ТВ, интернета, кухни и кондиционера), а также общее количество дней и сумму за все дни аренды каждой из таких комнат.
Используйте конструкции "as days" и "as total_fee" для вывода количества дней и суммы аренды, соответственно.
Если комната не сдавалась, то количество дней и сумму вывести как 0.
Поля в результирующей таблице:
`home_type`
`address`
`days`
`total_fee`

```sql

SELECT home_type,
	address,
	COALESCE(
		SUM(
			DATE_PART('day', rs.end_date - rs.start_date)::integer
		),
		0
	) AS days,
	COALESCE(SUM(total), 0) AS total_fee
FROM Rooms AS r
	LEFT JOIN Reservations AS rs ON r.id = rs.room_id
WHERE has_tv = TRUE
	AND has_internet = TRUE
	AND has_kitchen = TRUE
	AND has_air_con = TRUE
GROUP BY home_type,
	address

```

#### Задание #67
##### Время отлёта и прилёта
Вывести время отлета и время прилета для каждого перелета в формате "ЧЧ:ММ, ДД.ММ - ЧЧ:ММ, ДД.ММ", где часы и минуты с ведущим нулем, а день и месяц без.
Используйте конструкции "as flight_time" для полученной строки с датами отлета и прилета.
Поля в результирующей таблице:
`flight_time`

```sql

SELECT CONCAT(
		TO_CHAR(time_out, 'HH24:MI, FMDD.FMMM'),
		' - ',
		TO_CHAR(time_in, 'HH24:MI, FMDD.FMMM')
	) AS flight_time
FROM Trip

```

#### Задание #68
##### Последний арендатор комнаты
Для каждой комнаты, которую снимали как минимум 1 раз, найдите имя человека, снимавшего ее последний раз, и дату, когда он выехал
Используйте конструкцию "as room_id" для вывода идентификатора комнаты
Поля в результирующей таблице:
`room_id`
`name`
`end_date`

```sql

WITH max_dates AS (
	SELECT room_id,
		MAX(end_date) as end_date
	FROM Reservations AS rs
	GROUP BY room_id
)
SELECT rs.room_id,
	name,
	rs.end_date
FROM Reservations AS rs
	JOIN Users AS u ON rs.user_id = u.id
	JOIN max_dates AS md ON rs.room_id = md.room_id
	AND rs.end_date = md.end_date

```

#### Задание #69
##### Заработок владельцев комнат
Вывести идентификаторы всех владельцев комнат, что размещены на сервисе бронирования жилья и сумму, которую они заработали
Используйте конструкцию "as owner_id" и "as total_earn" для вывода идентификаторов владельцев и заработанной суммы соответственно.
Поля в результирующей таблице:
`owner_id`
`total_earn`

```sql

SELECT owner_id,
	COALESCE(SUM(total), 0) AS total_earn
FROM Rooms AS R
	LEFT JOIN Reservations AS rs ON r.id = rs.room_id
GROUP BY owner_id

```

#### Задание #70
##### Категоризация жилья по цене
Необходимо категоризовать жилье на economy, comfort, premium по цене соответственно <= 100, 100 < цена < 200, >= 200. В качестве результата вывести таблицу с названием категории и количеством жилья, попадающего в данную категорию
Используйте конструкцию "as category" и "as count" для вывода названия категории и количества такого жилья соответственно.
Поля в результирующей таблице:
`category`
`count`

```sql

SELECT CASE
		WHEN price <= 100 THEN 'economy'
		WHEN price > 100
		AND price < 200 THEN 'comfort'
		WHEN price >= 200 THEN 'premium'
	END AS category,
	COUNT(*) AS count
FROM Rooms
GROUP BY category

```

#### Задание #71
##### Процент активных пользователей
Найдите какой процент пользователей, зарегистрированных на сервисе бронирования, хоть раз арендовали или сдавали в аренду жилье. Результат округлите до сотых.
Используйте конструкцию "as percent" для вывода процента активных пользователей. Пример формата ответа: 65.23
Поля в результирующей таблице:
`percent`

```sql

SELECT ROUND(
		COUNT(DISTINCT active_id) * 100.0 / COUNT(DISTINCT u.id),
		2
	) AS percent
FROM Users AS u
	LEFT JOIN (
		SELECT user_id AS active_id
		FROM Reservations
		UNION
		SELECT DISTINCT owner_id AS active_id
		FROM Rooms AS r
			INNER JOIN Reservations AS rs ON r.id = rs.room_id
	) AS active ON u.id = active.active_id;

```

#### Задание #72
##### Средняя цена бронирования
Выведите среднюю цену бронирования за сутки для каждой из комнат, которую бронировали хотя бы один раз. Среднюю цену необходимо округлить до целого значения вверх.
Используйте конструкцию "as avg_price" для вывода средней стоимости бронирования для комнат
Поля в результирующей таблице:
`room_id`
`avg_price`

```sql

SELECT room_id,
	CEIL(AVG(price)) AS avg_price
FROM Reservations
GROUP BY room_id

```

#### Задание #73
##### Комнаты, арендованные нечетное число раз
Выведите id тех комнат, которые арендовали нечетное количество раз
Используйте конструкцию "as count" для вывода количество сколько раз комнату брали в аренду
Поля в результирующей таблице:
`room_id`
`count`

```sql

SELECT room_id,
	count(*) AS count
FROM Rooms AS r
	JOIN Reservations AS rs ON r.id = rs.room_id
GROUP BY room_id
HAVING MOD(count(*), 2) <> 0

```

#### Задание #74
##### Наличие интернета в помещении
Выведите идентификатор и признак наличия интернета в помещении. Если интернет в сдаваемом жилье присутствует, то выведите «YES», иначе «NO».
Используйте конструкцию "AS has_internet" для вывода признака наличия интернета в помещении.
Поля в результирующей таблице:
`id`
`has_internet`

```sql

SELECT id,
	CASE
		WHEN has_internet = 'true' THEN 'YES'
		ELSE 'NO'
	END AS has_internet
FROM Rooms AS r

```

#### Задание #75
##### Студенты, рожденные в мае
Выведите фамилию, имя и дату рождения студентов, кто был рожден в мае.
Поля в результирующей таблице:
`last_name`
`first_name`
`birthday`

```sql

SELECT last_name,
	first_name,
	birthday
FROM Student
WHERE DATE_PART('month', birthday) = 5

```

#### Задание #76
##### Статус пользователя: собственник/арендатор
Вывести имена всех пользователей сервиса бронирования жилья, а также два признака: является ли пользователь собственником какого-либо жилья (is_owner) и является ли пользователь арендатором (is_tenant). В случае наличия у пользователя признака необходимо вывести в соответствующее поле 1, иначе 0.
Используйте конструкцию "AS is_owner" для отображения признака собственника жилья.
Используйте конструкцию "AS is_tenant" для отображения признака арендатора
Поля в результирующей таблице:
`name`
`is_owner`
`is_tenant`

```sql

SELECT name,
	CASE
		WHEN id IN (
			SELECT owner_id
			FROM Rooms
		) THEN 1
		ELSE 0
	END AS is_owner,
	CASE
		WHEN id IN (
			SELECT user_id
			FROM Reservations
		) THEN 1
		ELSE 0
	END AS is_tenant
FROM Users;

```

#### Задание #77
##### Создать представление "People"
Создайте представление с именем "People", которое будет содержать список имен (first_name) и фамилий (last_name) всех студентов (Student) и преподавателей(Teacher)

```sql

CREATE VIEW People AS
SELECT first_name,
	last_name
FROM Student
UNION ALL
SELECT first_name,
	last_name
FROM Teacher;

```

#### Задание #78
##### Пользователи с почтой hotmail.com
Выведите всех пользователей с электронной почтой в «hotmail.com»
Поля в результирующей таблице:
`*`

```sql

SELECT *
FROM Users
WHERE email LIKE '%@hotmail.com'

```

#### Задание #79
##### Цена со скидкой 10%
Выведите поля id, home_type, price у всего жилья из таблицы Rooms. Если комната имеет телевизор и интернет одновременно, то в качестве цены в поле price выведите цену, применив скидку 10%.
Поля в результирующей таблице:
`id`
`home_type`
`price`

```sql

SELECT id,
	home_type,
	CASE
		WHEN has_tv = TRUE
		AND has_internet = TRUE THEN price * 0.9
		ELSE price
	END AS price
FROM Rooms

```

#### Задание #80
##### Создать представление "Verified_Users"
Создайте представление «Verified_Users» с полями id, name и email, которое будет показывает только тех пользователей, у которых подтвержден адрес электронной почты.

```sql

CREATE VIEW Verified_Users AS
SELECT id,
	name,
	email
FROM Users
WHERE email_verified_at IS NOT NULL

```
