# SQL_-
SQL_СКУД(Системе Контроля и Управления Доступом)

**Диаграмма:**

![image](https://user-images.githubusercontent.com/85709710/180169370-9eaca222-9d7a-4733-a7a1-9cf1feb93b18.png)


**Создание и наполнение таблиц:**
```
create schema manufactory

create table workers(
	worker_id serial primary key, 
	last_name varchar(50) not null,
	first_name varchar(50) not null,
	branches_id integer not null,
	email varchar(100) unique,
	phone text[],
	address_id integer not null,
	foreign key (address_id) references addresses(address_id),
	foreign key (branches_id) references branches(branches_id)
)

	insert into workers(last_name, first_name, branches_id, address_id)
	values ('Петров', 'Пётр', '1', '3')
		
create table branches(
	branches_id serial primary key,
	manager_staff_id int2 not null,
	address_id int2 not null,
	foreign key (address_id) references addresses(address_id)
	)
	
	insert into branches(manager_staff_id, address_id)
	values ('1', '4')
	
	create table birthday(
	birthday_id serial primary key,
	worker_id int2 not null,
	foreign key (worker_id) references workers(worker_id)
	)

	create table cities(
	city_id serial primary key,
	city varchar(20) unique
	)
	
	insert into cities(city)
	values ('Воронеж')

create table addresses (
	address_id serial primary key,
	city_id integer not null,
	street_id integer not null,
	building_appartament varchar(30),
	foreign key (city_id) references cities(city_id),
	foreign key (street_id) references street(street_id)
	)
	
	insert into addresses(city_id, street_id, building_appartament)
	values	(1, 1, 12/5),
			(1, 2, 13/7)
	
	create table street(
	street_id serial primary key,
	street_name varchar(20) unique
	)
	
	insert into street(street_name)
	values ('Плехановская'),
	('Кирова'),
		
	update street
	set street_name = 'Плехановская'
	where street_id = 1
		
	create table timesheet(
	timesheet_id serial primary key,
	worker_id integer not null,
	branches_id integer not null,
	start_time timestamp default now(),
	end_time timestamp default now(),
	working_hours_fact interval,
	foreign key (worker_id) references workers(worker_id),
	foreign key (branches_id) references branches(branches_id)
	)
	
	create table salary(
	worker_id integer not null,
	salary decimal(10, 2) not null,
	date timestamp default now(),
	foreign key (worker_id) references workers(worker_id)
	)
		
	insert into timesheet(worker_id, branches_id, start_time)
	values (3, 1, now())
	```
	**Обновление данных:**
  ```
	update timesheet
	set end_time = now(), working_hours_fact = (end_time - start_time)
	where timesheet_id = 1
	```
	
  **Результат:**
  ```
	select t.branches_id, worker_id, last_name, first_name, start_time, end_time, working_hours_fact
	from timesheet t
	left join workers using(worker_id)
	where (start_time):: date = '2021.09.07' and working_hours_fact is NULL
  ```
  ![image](https://user-images.githubusercontent.com/85709710/180170160-481899ae-1f00-464d-9c21-3843206a7ddc.png)
