# Прохождение тренажера от SQL Academy
<details>
<summary>Задание №1: Вывести имена всех людей, которые есть в базе данных авиакомпаний.</summary>
  
  ```mysql
SELECT name
FROM passenger
```

</details>

<details>
<summary>Задание №2: Вывести названия всеx авиакомпаний.</summary>
  
  ```mysql
SELECT name
FROM company
```

</details>

<details>
<summary>Задание №3: Вывести все рейсы, совершенные из Москвы.</summary>
  
  ```mysql
SELECT *
FROM Trip
WHERE town_from = 'Moscow'
```

</details>
