# MySQL

### Требования:  
1. Скачать дампы базы данных [Hogwarts](<https://drive.google.com/drive/u/0/folders/1MC0AttnmlAmugifFlX3hG6pssYZDqpPB>)
2. Осуществить импорт таблиц, используя Workbench
3. Выполнить задания 

### Примеры запросов:

#### Task 1

1. Выведите имя, фамилию, патронуса всех персонажей, у которых есть patronus и он известен

```sql
select fname, lname, patronus
from characters
where patronus is not null and not patronus = 'Unknown';
```
2. Выведите фамилию персонажей, у которых последняя буква в фамилии ‘e’ 

```sql
select lname
from characters
where lname like '%e';
```

3. Посчитайте общий возраст всех персонажей и выведите это на экран

```sql
select sum(age)
from characters;
```

4. Выведите имя, фамилию и возраст персонажей по убыванию их возраста

```sql
select fname, lname, age
from characters
order by age desc;
```

5. Выведите имя персонажа и возраст, у которых последний находится в диапазоне от 50 до 100 лет

```sql
select fname, age
from characters
where age between 50 and 100;
```

6. Выведите возраст всех персонажей так, чтобы среди них не было тех, у кого он одинаковый

```sql
select distinct age
from characters;
```

7. Выведите всю информацию о персонажах, у которых faculty = Gryffindor и чей возраст больше 30 лет

```sql
select *
from characters
where faculty = 'Gryffindor' and age > 30;
```

8. Выведите имена первых трех факультетов из таблицы, так чтобы факультеты не повторялись

```sql
select distinct faculty 
from characters
limit 3;
```

9. Выведите имена всех персонажей, у которых имя начинается с ‘H’ и состоит из 5 букв, или чье имя начинается с ‘L’ 

```sql
select * 
from characters
where fname like 'H____' or fname like 'L%';
```

10. Посчитайте средний возраст всех персонажей

```sql
select avg(age)
from characters;
```

11. Удалите персонажа с ID = 11

```sql
delete from characters 
where char_id = 11;
```

12. Выведите фамилию всех персонажей, которые содержат в ней букву ‘a’

```sql
select fname
from characters
where fname like '%a%';
```

13. Используйте псевдоним для того, чтобы временно замените название столбца fname на Half-Blood Prince для реального принца-полукровки

```sql
select fname as Half_Blood_Prince
from characters
where fname = 'Severus';
```

14. Выведите id и имена всех патронусов в алфавитном порядки, при условии что они есть или известны

```sql
select char_id, patronus 
from characters
where patronus is not null and not 'Unknown'
order by patronus asc;
```

15. Используя оператор IN, выведите имя и фамилию тех персонажей, у которых фамилия Crabbe, Granger или Diggory

```sql
select fname, lname 
from characters
where lname in ('Crabbe', 'Granger', 'Diggory');
```

16. Выведите минимальный возраст персонажа

```sql
select min(age)
from characters;
```

17. Используя оператор UNION выберите имена из таблицы characters и названия книг из таблицы library

```sql
select fname from characters
union 
select book_name from library;
```

18. Используя оператор HAVING посчитайте количество персонажей на каждом факультете, оставив только те факультеты, где количество студентов больше 1

```sql
select count(chair_id), faculty from characters
group by faculty
having count(chair_id) > 1;
```

19. Используя оператор CASE опишите следующую логику:

Выведите имя и фамилию персонажа, а также следующий текстовое сообщение:

   Если факультет Gryffindor, то в консоли должно вывестись Godric
   Если факультет Slytherin, то в консоли должно вывестись Salazar
   Если факультет Ravenclaw, то в консоли должно вывестись Rowena
   Если факультет Hufflepuff, то в консоли должно вывестись Helga
   Если другая информация, то выводится Muggle

Для сообщения используйте псевдоним Founders

```sql
select fname, lname
case 
 when faculty = 'Gryffindor' then 'Godric'
 when faculty = 'Slytherin' then 'Salazar'
 when faculty = 'Ravenclaw' then 'Rowena'
 when faculty = 'Hufflepuff' then 'Helga'
 else 'Muggle'
end Founders
from characters;
```

20. Используя регулярное выражение найдите фамилии персонажей, которые не начинаются с букв H, L или S и выведите их

```sql
select lname from characters
where  not lname regexp '^[HLS]';
```

#### Task 2

1. Выведите имя, фамилию персонажей и название книги, которая на них числится

```sql
select characters.fname, characters.lname, library.book_name
from characters
join library on characters.char_id = library.char_id;
```

2. Выведите имя, фамилию персонажей и название книги, вне зависимости от того, есть ли у них книги или нет

```sql
select lname, fname, book_name
from characters
left join library on characters.char_id = library.char_id;
```

3. Выведите название книги и имя патронуса, вне зависимости от того, есть ли информация о держателе книги в таблице или нет

```sql
select book_name, patronus
from characters
right join library on characters.char_id = library.char_id;
```

4. Выведите имя, фамилию, возраст персонажей и название книги, которая на них числится, при условии, что все владельцы книг должны быть старше 15 лет

```sql
select characters.fname, characters.lname, characters.age, library.book_name
from characters
join library on characters.char_id = library.char_id
where age > 15 and patronus = 'Unknown';
```

5. Выведите имя персонажа, название книги, дату выдачи и дату завершения, при условии, что он младше 15 лет и его патронус неизвестен

```sql
select characters.fname, library.book_name, library.start_date, library.end_date   
from characters
join library on characters.char_id = library.char_id
where age > 15 and patronus = 'Unknown';
```

6. Используя вложенный запрос количество книг, у которых end_date больше, чем end_date у Hermione

```sql
select count(book_id)
from  library 
where end_date > (select library.end_date 
                  from library 
                  join characters on characters.char_id = library.char_id
                  where fname = 'Hermione');
```

7. С помощью вложенного запроса выведите имена всех патронусов, у которых владельцы старше возраста персонажа, у которого патронус Unknown

```sql
select patronus
from characters
where age > (select age 
             from characters 
             where patronus = 'Unknown')
```
