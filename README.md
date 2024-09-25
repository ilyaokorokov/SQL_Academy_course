# Прохождение тренажера от SQL Academy
<details>
<summary><b>Задание №1:</b> Вывести имена всех людей, которые есть в базе данных авиакомпаний.</summary>
  
  ```mysql
SELECT name
FROM passenger
```

</details>

<details>
<summary><b>Задание №2:</b> Вывести названия всеx авиакомпаний.</summary>
  
  ```mysql
SELECT name
FROM company
```

</details>

<details>
<summary><b>Задание №3:</b> Вывести все рейсы, совершенные из Москвы.</summary>
  
  ```mysql
SELECT *
FROM Trip
WHERE town_from = 'Moscow'
```

</details>
<details>
<summary><b>Задание №4:</b> Вывести имена людей, которые заканчиваются на "man".</summary>
  
  ```mysql
SELECT name
FROM passenger
WHERE name LIKE '%man'
```

</details>
<details>
<summary><b>Задание №5:</b> Вывести количество рейсов, совершенных на TU-134.</summary>
  
  ```mysql
SELECT count(*) as count
FROM Trip
WHERE plane = 'TU-134'
```

</details>
<details>
<summary><b>Задание №6:</b> Какие компании совершали перелеты на Boeing.</summary>
  
  ```mysql
SELECT DISTINCT name
FROM Company
JOIN Trip on Company.id = Trip.company
WHERE plane = 'Boeing'
```

</details>
<details>
<summary><b>Задание №7:</b> Вывести все названия самолётов, на которых можно улететь в Москву (Moscow).</summary>
  
  ```mysql
SELECT DISTINCT plane
FROM Trip
WHERE town_to = 'Moscow'
```

</details>
<details>
<summary><b>Задание №8:</b> В какие города можно улететь из Парижа (Paris) и сколько времени это займёт?</summary>
  
  ```mysql
SELECT town_to,	TIMEDIFF(time_in, time_out) as flight_time
FROM Trip
WHERE town_from = 'Paris'
```

</details>
<details>
<summary><b>Задание №9:</b> Какие компании организуют перелеты из Владивостока (Vladivostok)?</summary>
  
  ```mysql
SELECT DISTINCT Company.name
FROM Trip
JOIN Company ON Trip.company = Company.id
WHERE Trip.town_from = 'Vladivostok'
```

</details>
<details>
<summary><b>Задание №10:</b> Вывести вылеты, совершенные с 10 ч. по 14 ч. 1 января 1900 г.</summary>
  
  ```mysql
SELECT *
FROM Trip
WHERE time_out BETWEEN '1900-01-01 10:00:00' AND '1900-01-01 14:00:00'
```

</details>
<details>
<summary><b>Задание №11:</b> Выведите пассажиров с самым длинным ФИО. Пробелы, дефисы и точки считаются частью имени.</summary>
  
  ```mysql
SELECT name
FROM Passenger
WHERE LENGTH(name)=(SELECT max(LENGTH(name))
  FROM Passenger)
```

</details>
<details>
<summary><b>Задание №12:</b> Выведите идентификаторы всех рейсов и количество пассажиров на них. Обратите внимание, что на каких-то рейсах пассажиров может не быть. В этом случае выведите число "0".</summary>
  
  ```mysql
SELECT Trip.id AS id, COUNT(Pass_in_trip.id) AS count
FROM Trip
JOIN Pass_in_trip ON Trip.id = Pass_in_trip.trip
GROUP BY Trip.id;
```

</details>
<details>
<summary><b>Задание №13:</b> Вывести имена людей, у которых есть полный тёзка среди пассажиров.</summary>
  
  ```mysql
SELECT name
FROM Passenger
GROUP BY 1
HAVING count(name) = 2
```

</details>
<details>
<summary><b>Задание №14:</b> В какие города летал Bruce Willis.</summary>
  
  ```mysql
SELECT Trip.town_to
FROM Trip
JOIN Pass_in_trip ON Trip.id = Pass_in_trip.trip
JOIN Passenger ON Pass_in_trip.passenger = Passenger.id
WHERE Passenger.name = 'Bruce Willis'
```

</details>
<details>
<summary><b>Задание №15:</b> Выведите дату и время прилёта пассажира Стив Мартин (Steve Martin) в Лондон (London).</summary>
  
  ```mysql
SELECT Trip.time_in
FROM Trip
JOIN Pass_in_trip ON Trip.id = Pass_in_trip.trip
JOIN Passenger ON Pass_in_trip.passenger = Passenger.id
WHERE Passenger.name = 'Steve Martin' AND Trip.town_to = 'London'
```

</details>
<details>
<summary><b>Задание №16:</b> Вывести отсортированный по количеству перелетов (по убыванию) и имени (по возрастанию) список пассажиров, совершивших хотя бы 1 полет.</summary>
  
  ```mysql
SELECT Passenger.name, COUNT(*) as count
FROM Pass_in_trip
JOIN Passenger ON Pass_in_trip.passenger = Passenger.id
GROUP BY Passenger.name
HAVING count >= 1
ORDER BY 2 DESC, 1 ASC
```

</details>
<details>
<summary><b>Задание №17:</b> Определить, сколько потратил в 2005 году каждый из членов семьи. В результирующей выборке не выводите тех членов семьи, которые ничего не потратили.</summary>
  
  ```mysql
SELECT FamilyMembers.member_name, FamilyMembers.status, SUM(Payments.unit_price*Payments.amount) as costs
FROM FamilyMembers
JOIN Payments ON FamilyMembers.member_id = Payments.family_member
WHERE YEAR(Payments.date) = 2005
GROUP BY FamilyMembers.member_name, FamilyMembers.status
```

</details>
<details>
<summary><b>Задание №18:</b> Выведите имя самого старшего человека. Если таких несколько, то выведите их всех.</summary>
  
  ```mysql
SELECT member_name
FROM FamilyMembers
ORDER BY birthday LIMIT 1
```

</details>
<details>
<summary><b>Задание №19:</b> Определить, кто из членов семьи покупал картошку (potato).</summary>
  
  ```mysql
SELECT FamilyMembers.status
FROM FamilyMembers
JOIN Payments ON FamilyMembers.member_id = Payments.family_member
JOIN Goods ON Payments.good = Goods.good_id
WHERE Goods.good_name = 'potato'
GROUP BY 1
```

</details>
<details>
<summary><b>Задание №20:</b> Сколько и кто из семьи потратил на развлечения (entertainment). Вывести статус в семье, имя, сумму.</summary>
  
  ```mysql
SELECT FamilyMembers.status, FamilyMembers.member_name, SUM (Payments.unit_price*Payments.amount) as costs
FROM FamilyMembers
JOIN Payments ON FamilyMembers.member_id = Payments.family_member
JOIN Goods ON Payments.good = Goods.good_id
WHERE Goods.type = 4
GROUP BY 1, 2
```

</details>
<details>
<summary><b>Задание №21:</b> Определить товары, которые покупали более 1 раза.</summary>
  
  ```mysql
SELECT Goods.good_name
FROM Goods
JOIN Payments ON Goods.good_id = Payments.good
GROUP BY 1
HAVING Count(*) > 1
```

</details>
<details>
<summary><b>Задание №22:</b> Найти имена всех матерей (mother).</summary>
  
  ```mysql
SELECT member_name
FROM FamilyMembers
WHERE status = 'mother'
```

</details>
<details>
<summary><b>Задание №23:</b> Найдите самый дорогой деликатес (delicacies) и выведите его цену.</summary>
  
  ```mysql
SELECT Goods.good_name, Payments.unit_price
FROM Payments
JOIN Goods ON Payments.good = Goods.good_id
JOIN GoodTypes ON Goods.good_id = GoodTypes.good_type_id
WHERE Goods.type = 3
ORDER BY Payments.unit_price DESC LIMIT 1
```

</details>
<details>
<summary><b>Задание №24:</b> Определить кто и сколько потратил в июне 2005.</summary>
  
  ```mysql
SELECT FamilyMembers.member_name, SUM(Payments.unit_price*Payments.amount) as costs
FROM FamilyMembers
JOIN Payments ON FamilyMembers.member_id = Payments.family_member
WHERE YEAR(Payments.date) = 2005 AND MONTH(Payments.date) = 06
GROUP BY 1
```

</details>
<details>
<summary><b>Задание №25:</b> Определить, какие товары не покупались в 2005 году.</summary>
  
  ```mysql
SELECT good_name
FROM Goods
WHERE good_id NOT IN (
    SELECT good
    FROM Payments
    WHERE YEAR(date) = 2005
)
```

</details>
<details>
<summary><b>Задание №26:</b> Определить группы товаров, которые не приобретались в 2005 году.</summary>
  
  ```mysql
SELECT good_type_name
FROM GoodTypes
WHERE good_type_id NOT IN (SELECT type
  FROM Goods
  JOIN Payments ON Goods.good_id = Payments.good
  WHERE YEAR(date) = 2005)
```

</details>
<details>
<summary><b>Задание №27:</b> Узнайте, сколько было потрачено на каждую из групп товаров в 2005 году. Выведите название группы и потраченную на неё сумму. Если потраченная сумма равна нулю, т.е. товары из этой группы не покупались в 2005 году, то не выводите её.</summary>
  
  ```mysql
SELECT GoodTypes.good_type_name, SUM(Payments.amount * Payments.unit_price) as costs
FROM GoodTypes
JOIN Goods ON GoodTypes.good_type_id = Goods.type
JOIN Payments ON Goods.good_id = Payments.good
WHERE YEAR(Payments.date) = 2005
GROUP BY GoodTypes.good_type_name
```

</details>
<details>
<summary><b>Задание №28:</b> Сколько рейсов совершили авиакомпании из Ростова (Rostov) в Москву (Moscow) ?</summary>
  
  ```mysql
SELECT COUNT(*) as count
FROM Trip
WHERE town_from = 'Rostov' and town_to = 'Moscow'
```

</details>
<details>
<summary><b>Задание №29:</b> Выведите имена пассажиров улетевших в Москву (Moscow) на самолете TU-134.</summary>
  
  ```mysql
SELECT Passenger.name
FROM Passenger
JOIN Pass_in_trip ON Passenger.id = Pass_in_trip.passenger
JOIN Trip ON Pass_in_trip.trip = Trip.id
WHERE Trip.town_to = 'Moscow' AND Trip.plane = 'TU-134'
GROUP BY Passenger.name
```

</details>
<details>
<summary><b>Задание №30:</b> Выведите нагруженность (число пассажиров) каждого рейса (trip). Результат вывести в отсортированном виде по убыванию нагруженности.</summary>
  
  ```mysql
SELECT Pass_in_trip.trip, COUNT(*) as count
FROM Pass_in_trip
GROUP BY 1
ORDER BY 2 DESC 
```

</details>
<details>
<summary><b>Задание №31:</b> Вывести всех членов семьи с фамилией Quincey.</summary>
  
  ```mysql
SELECT *
FROM FamilyMembers
WHERE member_name LIKE '%Quincey'
```

</details>
<details>
<summary><b>Задание №32:</b> Вывести средний возраст людей (в годах), хранящихся в базе данных. Результат округлите до целого в меньшую сторону.</summary>
  
  ```mysql
SELECT FLOOR(AVG(YEAR(CURDATE()) - YEAR(birthday))) as age
FROM FamilyMembers
```

</details>
<details>
<summary><b>Задание №33:</b> Найдите среднюю цену икры на основе данных, хранящихся в таблице Payments. В базе данных хранятся данные о покупках красной (red caviar) и черной икры (black caviar). В ответе должна быть одна строка со средней ценой всей купленной когда-либо икры.</summary>
  
  ```mysql
SELECT AVG(Payments.unit_price) as cost
FROM Payments
JOIN Goods ON Payments.good = Goods.good_id
WHERE Goods.good_name = 'red caviar' OR Goods.good_name = 'black caviar'
```

</details>
<details>
<summary><b>Задание №34:</b> Сколько всего 10-ых классов.</summary>
  
  ```mysql
SELECT COUNT(*) as count
FROM Class
WHERE name LIKE '10%'
```

</details>
<details>
<summary><b>Задание №35:</b> Сколько различных кабинетов школы использовались 2 сентября 2019 года для проведения занятий?</summary>
  
  ```mysql
SELECT COUNT(DISTINCT classroom) as count
FROM Schedule
WHERE date LIKE '2019-09-02'
```

</details>
<details>
<summary><b>Задание №36:</b> Выведите информацию об обучающихся живущих на улице Пушкина (ul. Pushkina)?</summary>
  
  ```mysql
SELECT *
FROM Student
WHERE address LIKE 'ul. Pushkina%'
```

</details>
<details>
<summary><b>Задание №37:</b> Сколько лет самому молодому обучающемуся?</summary>
  
  ```mysql
SELECT MIN(TIMESTAMPDIFF(year, birthday, CURDATE())) as year 
FROM Student
```

</details>
<details>
<summary><b>Задание №38:</b> Сколько Анн (Anna) учится в школе?</summary>
  
  ```mysql
SELECT COUNT(*) as count
FROM Student
WHERE first_name = 'Anna'
```

</details>
<details>
<summary><b>Задание №39:</b> Сколько обучающихся в 10 B классе?</summary>
  
  ```mysql
SELECT COUNT(*) as count
FROM Class
JOIN Student_in_class ON Class.id = Student_in_class.class
WHERE Class.name = '10 B'
```

</details>
<details>
<summary><b>Задание №40:</b> Выведите название предметов, которые преподает Ромашкин П.П. (Romashkin P.P.). Обратите внимание, что в базе данных есть несколько учителей с такими фамилией и инициалами.</summary>
  
  ```mysql
SELECT Subject.name as subjects
FROM Subject
JOIN Schedule ON Subject.id = Schedule.subject
JOIN Teacher ON Schedule.teacher = Teacher.id
WHERE Teacher.last_name = 'Romashkin' AND Teacher.first_name LIKE 'P%' AND Teacher.middle_name LIKE 'P%'
```

</details>
<details>
<summary><b>Задание №41:</b> Выясните, во сколько по расписанию начинается четвёртое занятие.</summary>
  
  ```mysql

```

</details>
