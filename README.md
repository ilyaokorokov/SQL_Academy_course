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
WHERE LENGTH(name) = (
    SELECT max(LENGTH(name))
    FROM Passenger
)
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
WHERE good_type_id NOT IN (
    SELECT type
    FROM Goods
    JOIN Payments ON Goods.good_id = Payments.good
    WHERE YEAR(date) = 2005
)
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
SELECT DISTINCT Timepair.start_pair
FROM Timepair
JOIN Schedule ON Timepair.id = Schedule.number_pair
WHERE Schedule.number_pair = 4
```

</details>
<details>
<summary><b>Задание №42:</b> Сколько времени обучающийся будет находиться в школе, учась со 2-го по 4-ый уч. предмет?</summary>
  
  ```mysql
SELECT TIMEDIFF(
    (SELECT end_pair FROM Timepair WHERE id = 4),
    (SELECT start_pair FROM Timepair WHERE id = 2)
) AS time

```

</details>
<details>
<summary><b>Задание №43:</b> Выведите фамилии преподавателей, которые ведут физическую культуру (Physical Culture). Отсортируйте преподавателей по фамилии в алфавитном порядке.</summary>
  
  ```mysql
SELECT Teacher.last_name
FROM Teacher
JOIN Schedule ON Teacher.id = Schedule.teacher
JOIN Subject ON Schedule.subject = Subject.id
WHERE Subject.name = 'Physical Culture'
ORDER BY 1 asc
```

</details>
<details>
<summary><b>Задание №44:</b> Найдите максимальный возраст (количество лет) среди обучающихся 10 классов на сегодняшний день. Для получения текущих даты и времени используйте функцию NOW().</summary>
  
  ```mysql
SELECT MAX(TIMESTAMPDIFF(year, Student.birthday, NOW())) as max_year
FROM Student
JOIN Student_in_class ON Student.id = Student_in_class.student
JOIN Class ON Student_in_class.class = Class.id
WHERE Class.name LIKE '10%'
```

</details>
<details>
<summary><b>Задание №45:</b> Какие кабинеты чаще всего использовались для проведения занятий? Выведите те, которые использовались максимальное количество раз.</summary>
  
  ```mysql
SELECT classroom
FROM Schedule
GROUP BY classroom
HAVING COUNT(classroom) = (
    SELECT COUNT(*)
    FROM Schedule
    GROUP BY classroom
    ORDER BY 1 DESC LIMIT 1
)
```

</details>
<details>
<summary><b>Задание №46:</b> В каких классах введет занятия преподаватель "Krauze" ?</summary>
  
  ```mysql
SELECT Class.name
FROM Class
JOIN Schedule ON Class.id = Schedule.class
JOIN Teacher ON Schedule.teacher = Teacher.id
WHERE Teacher.last_name = 'Krauze'
GROUP BY Class.name
```

</details>
<details>
<summary><b>Задание №47:</b> Сколько занятий провел Krauze 30 августа 2019 г.?</summary>
  
  ```mysql
SELECT COUNT(*) as count
FROM Schedule
JOIN Teacher ON Schedule.teacher = Teacher.id
WHERE Teacher.last_name = 'Krauze' AND Schedule.date LIKE '2019-08-30%'
```

</details>
<details>
<summary><b>Задание №48:</b> Выведите заполненность классов в порядке убывания</summary>
  
  ```mysql
SELECT Class.name, COUNT(*) as count
FROM Class
JOIN Student_in_class ON Class.id = Student_in_class.class
JOIN Student ON Student_in_class.student = Student.id
GROUP BY 1
ORDER BY 2 DESC
```

</details>
<details>
<summary><b>Задание №49:</b> Какой процент обучающихся учится в "10 A" классе? Выведите ответ в диапазоне от 0 до 100 с округлением до четырёх знаков после запятой, например, 96.0201.</summary>
  
  ```mysql
SELECT ROUND(((
    SELECT COUNT(*)
    FROM Student_in_class
    JOIN Class ON Student_in_class.class = Class.id
    WHERE Class.name = '10 A')
    /
    (SELECT COUNT(*)
    FROM Student_in_class
    JOIN Class ON Student_in_class.class = Class.id)) * 100, 4) as percent
```

</details>
<details>
<summary><b>Задание №50:</b> Какой процент обучающихся родился в 2000 году? Результат округлить до целого в меньшую сторону.</summary>
  
  ```mysql
SELECT FLOOR(((
    SELECT COUNT(*)
    FROM Student
    WHERE YEAR(birthday) = 2000)
    /
    (SELECT COUNT(*)
    FROM Student)) * 100) as percent
```

</details>
<details>
<summary><b>Задание №51:</b> Добавьте товар с именем "Cheese" и типом "food" в список товаров (Goods).</summary>
  
  ```mysql
INSERT INTO Goods (good_name, type)
VALUES ('Cheese',
    (SELECT good_type_id
    FROM GoodTypes
    WHERE good_type_name = 'food')
    )
```

</details>
<details>
<summary><b>Задание №52:</b> Добавьте в список типов товаров (GoodTypes) новый тип "auto".</summary>
  
  ```mysql
INSERT INTO GoodTypes
SET good_type_id = (
    SELECT COUNT(*) + 1
    FROM GoodTypes as table_name
),
    good_type_name = 'auto'
```

</details>
<details>
<summary><b>Задание №53:</b> Измените имя "Andie Quincey" на новое "Andie Anthony".</summary>
  
  ```mysql
UPDATE FamilyMembers
SET member_name = 'Andie Anthony'
WHERE member_name = 'Andie Quincey'
```

</details>
<details>
<summary><b>Задание №54:</b> Удалить всех членов семьи с фамилией "Quincey".</summary>
  
  ```mysql
DELETE
FROM FamilyMembers
WHERE member_name LIKE '%Quincey'
```

</details>
<details>
<summary><b>Задание №55:</b> Удалить компании, совершившие наименьшее количество рейсов.</summary>
  
  ```mysql
DELETE
FROM company
WHERE id IN (
    SELECT company
    FROM trip
    GROUP BY company
    HAVING COUNT(*) = (
        SELECT COUNT(*) AS c
        FROM trip
        GROUP BY company
        ORDER BY 1
        LIMIT 1
        )
    )
```

</details>
<details>
<summary><b>Задание №56:</b> Удалить все перелеты, совершенные из Москвы (Moscow).</summary>
  
  ```mysql
DELETE
FROM Trip
WHERE town_from = 'Moscow'
```

</details>
<details>
<summary><b>Задание №57:</b> Перенести расписание всех занятий на 30 мин. вперед.</summary>
  
  ```mysql
UPDATE Timepair
SET start_pair = ADDTIME(start_pair, '00:30:00'),
    end_pair = ADDTIME(end_pair, '00:30:00')
```

</details>
<details>
<summary><b>Задание №58:</b> Добавить отзыв с рейтингом 5 на жилье, находящиеся по адресу "11218, Friel Place, New York", от имени "George Clooney".</summary>
  
  ```mysql
INSERT INTO Reviews
SET rating = 5,
    id = (
        SELECT COUNT(*) + 1
        FROM Reviews as rw),
    reservation_id = (
        SELECT rs.id
        FROM Reservations as rs
        JOIN Users as us ON rs.user_id = us.id
        JOIN Rooms as rom ON rs.room_id = rom.id
        WHERE us.name = 'George Clooney' AND rom.address = '11218, Friel Place, New York')
    
```

</details>
<details>
<summary><b>Задание №59:</b> Вывести пользователей,указавших Белорусский номер телефона ? Телефонный код Белоруссии +375.</summary>
  
  ```mysql
SELECT *
FROM Users
WHERE phone_number LIKE '+375%'
```

</details>
<details>
<summary><b>Задание №60:</b> Выведите идентификаторы преподавателей, которые хотя бы один раз за всё время преподавали в каждом из одиннадцатых классов.</summary>
  
  ```mysql
SELECT Schedule.Teacher
FROM Schedule
JOIN Class ON Schedule.class = Class.id
WHERE Class.name LIKE '11%'
GROUP BY 1
HAVING COUNT(DISTINCT Class.name) = 2
```

</details>
<details>
<summary><b>Задание №61:</b> Выведите список комнат, которые были зарезервированы хотя бы на одни сутки в 12-ую неделю 2020 года. В данной задаче в качестве одной недели примите период из семи дней, первый из которых начинается 1 января 2020 года. Например, первая неделя года — 1–7 января, а третья — 15–21 января.</summary>
  
```mysql
SELECT Rooms.*
FROM Rooms
JOIN Reservations ON Rooms.id = Reservations.room_id
WHERE DATE(Reservations.start_date) BETWEEN "2020-03-18" AND "2020-03-24"
```

</details>
<details>
<summary><b>Задание №62:</b> Вывести в порядке убывания популярности доменные имена 2-го уровня, используемые пользователями для электронной почты. Полученный результат необходимо дополнительно отсортировать по возрастанию названий доменных имён.</summary>
  
```mysql
SELECT SUBSTRING_INDEX(email, '@', -1) as domain, COUNT(*) as count
FROM Users
GROUP BY domain
ORDER BY 2 DESC, 1 ASC
```

</details>
<details>
<summary><b>Задание №63:</b> Выведите отсортированный список (по возрастанию) фамилий и имен студентов в виде Фамилия.И.</summary>
  
```mysql
SELECT CONCAT(last_name, '.', LEFT(first_name, 1), '.') as name
FROM Student
ORDER BY 1
```

</details>
<details>
<summary><b>Задание №64:</b> Вывести количество бронирований по каждому месяцу каждого года, в которых было хотя бы 1 бронирование. Результат отсортируйте в порядке возрастания даты бронирования.</summary>
  
```mysql
SELECT YEAR(start_date) as year, MONTH(start_date) as month, COUNT(*) as amount
FROM Reservations
GROUP BY 1, 2
HAVING amount >=1
ORDER BY 1, 2
```

</details>
<details>
<summary><b>Задание №65:</b> Необходимо вывести рейтинг для комнат, которые хоть раз арендовали, как среднее значение рейтинга отзывов округленное до целого вниз.</summary>
  
```mysql
SELECT Reservations.room_id, FLOOR(AVG(Reviews.rating)) as rating
FROM Reservations
JOIN Reviews ON Reservations.id = Reviews.reservation_id
GROUP BY 1
```

</details>
<details>
<summary><b>Задание №66:</b> Вывести список комнат со всеми удобствами (наличие ТВ, интернета, кухни и кондиционера), а также общее количество дней и сумму за все дни аренды каждой из таких комнат.</summary>
  
```mysql
SELECT Rooms.home_type, Rooms.address, IFNULL(SUM(DATEDIFF(Reservations.end_date, Reservations.start_date)), 0) as days, IFNULL(SUM(Reservations.total), 0) as total_fee
FROM Rooms
LEFT JOIN Reservations ON Rooms.id = Reservations.room_id
WHERE has_internet = 1 AND has_kitchen = 1 AND has_air_con = 1 AND has_tv = 1
GROUP BY 1, 2
```

</details>
<details>
<summary><b>Задание №67:</b> Вывести время отлета и время прилета для каждого перелета в формате "ЧЧ:ММ, ДД.ММ - ЧЧ:ММ, ДД.ММ", где часы и минуты с ведущим нулем, а день и месяц без.</summary>
  
```mysql

```

</details>
<details>
<summary><b>Задание №50:</b> Какой.</summary>
  
```mysql

```

</details>
<details>
<summary><b>Задание №50:</b> Какой.</summary>
  
```mysql

```

</details>
<details>
<summary><b>Задание №50:</b> Какой.</summary>
  
```mysql

```

</details>
<details>
<summary><b>Задание №50:</b> Какой.</summary>
  
```mysql

```

</details>
<details>
<summary><b>Задание №50:</b> Какой.</summary>
  
```mysql

```

</details>
<details>
<summary><b>Задание №50:</b> Какой.</summary>
  
```mysql

```

</details>
<details>
<summary><b>Задание №50:</b> Какой.</summary>
  
```mysql

```

</details>
<details>
<summary><b>Задание №50:</b> Какой.</summary>
  
```mysql

```

</details>
<details>
<summary><b>Задание №50:</b> Какой.</summary>
  
```mysql

```

</details>
<details>
<summary><b>Задание №50:</b> Какой.</summary>
  
```mysql

```

</details>
