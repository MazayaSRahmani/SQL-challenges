## Database Information
This project use the Sakila database from the official [MySQL sakila sample database](https://dev.mysql.com/doc/sakila/en/) that can be accessed here.
---

### Query to list all actors with the first name 'Scarlett' in the `actor` table.
```
select *
from actor
where first_name = 'Scarlett';
```

### Query to count the number of unique last names in the `actor` table.
```
select count(distinct last_name) as nama_belakang_unik
from actor;
```

### Query to find the top 5 least common last names in the `actor` table.
```
select 
	last_name as nama_belakang,
	sum(case WHEN last_name is not null THEN 1 ELSE 0 END) AS jumlah_kemunculan 
from actor
group by nama_belakang
order by jumlah_kemunculan asc, nama_belakang asc
limit 5;
```

### Query to find the top 5 least common last names that appear more than once in the `actor` table.
```
select 
	last_name as nama_belakang,
	sum(case WHEN last_name is not null THEN 1 ELSE 0 END) AS jumlah_kemunculan 
from actor
group by nama_belakang
having jumlah_kemunculan > 1
order by nama_belakang asc
limit 5;
```

### Query to list all actors with their full names in uppercase in the `actor` table.
```
select ucase(concat(first_name, ' ', last_name)) as nama_aktor
from actor;
```

### Query to find the average length of movies in the `film` table.
```
select round(avg(length),2) as rerata_durasi
from film;
```

### Query to list all customers in Indonesia with odd phone numbers in the `customer_list` table, ordered by the fourth column and ID.
```
select * 
from customer_list
where country = 'Indonesia' and phone % 2 = 1
order by 4, id;
```

### Query to find the average payment amount per month in the `payment` table, ordered by the average payment in descending order.
```
select monthname(payment_date) as bulan, round(avg(amount),3) as rerata_pembayaran 
from payment
group by bulan
order by rerata_pembayaran desc;
```

### Query to find the top 5 shortest movies in the `film` table.
```
select title, length
from film
order by length
limit 5;
```

### Query to count the number of movies per rating in the `film` table.
```
select rating, sum(case when rating is not null then 1 else 0 end) as jumlah_film
from film
group by rating
order by jumlah_film desc;
```

### Query to find the average replacement cost per rental rate for movies with titles starting with 'z' in the `film` table.
```
select rental_rate, avg(replacement_cost) as rerata_replacement_cost
from film
where title like 'z%'
group by rental_rate
order by rerata_replacement_cost;
```

### Query to find the average length of movies per rating, filtered by movies longer than 115 minutes in the `film` table.
```
select rating, avg(length) as rerata_durasi
from film
group by rating
having rerata_durasi > 115;
```
