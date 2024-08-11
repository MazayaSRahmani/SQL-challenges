## Database Information
This project use the Sakila database from the official [MySQL sakila sample database](https://dev.mysql.com/doc/sakila/en/) that can be accessed here.


### 1. Query to list all actors with the first name 'Scarlett' in the `actor` table.
```
select *
from actor
where first_name = 'Scarlett';
```

### 2. Query to count the number of unique last names in the `actor` table.
```
select count(distinct last_name) as nama_belakang_unik
from actor;
```

### 3. Query to find the top 5 least common last names in the `actor` table.
```
select 
	last_name as nama_belakang,
	sum(case WHEN last_name is not null THEN 1 ELSE 0 END) AS jumlah_kemunculan 
from actor
group by nama_belakang
order by jumlah_kemunculan asc, nama_belakang asc
limit 5;
```

### 4. Query to find the top 5 least common last names that appear more than once in the `actor` table.
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

### 5. Query to list all actors with their full names in uppercase in the `actor` table.
```
select ucase(concat(first_name, ' ', last_name)) as nama_aktor
from actor;
```

### 6. Query to find the average length of movies in the `film` table.
```
select round(avg(length),2) as rerata_durasi
from film;
```

### 7. Query to list all customers in Indonesia with odd phone numbers in the `customer_list` table, ordered by the fourth column and ID.
```
select * 
from customer_list
where country = 'Indonesia' and phone % 2 = 1
order by 4, id;
```

### 8. Query to find the average payment amount per month in the `payment` table, ordered by the average payment in descending order.
```
select monthname(payment_date) as bulan, round(avg(amount),3) as rerata_pembayaran 
from payment
group by bulan
order by rerata_pembayaran desc;
```

### 9. Query to find the top 5 shortest movies in the `film` table.
```
select title, length
from film
order by length
limit 5;
```

### 10. Query to count the number of movies per rating in the `film` table.
```
select rating, sum(case when rating is not null then 1 else 0 end) as jumlah_film
from film
group by rating
order by jumlah_film desc;
```

### 11. Query to find the average replacement cost per rental rate for movies with titles starting with 'z' in the `film` table.
```
select rental_rate, avg(replacement_cost) as rerata_replacement_cost
from film
where title like 'z%'
group by rental_rate
order by rerata_replacement_cost;
```

### 12. Query to find the average length of movies per rating, filtered by movies longer than 115 minutes in the `film` table.
```
select rating, avg(length) as rerata_durasi
from film
group by rating
having rerata_durasi > 115;
```

### 13. Query to list the first 10 payment transactions.
This query selects the first 10 records from the `payment` table, retrieving customer ID, rental ID, amount, and payment date.

```
select customer_id, rental_id, amount, payment_date
from payment
limit 10;
```
### 14. Query to list the first 10 films with titles starting with 'S'.
This query selects the first 10 films from the `film` table where the title starts with the letter 'S', retrieving the title, release year, and rental duration.

```
select title, release_year, rental_duration
from film 
where title like "s%"
limit 10;
```

### 15. Query to count the number of films for each rental duration and calculate the average length.
This query groups films by rental duration and calculates the count of titles and the average film length for each group.
The results are ordered by rental duration.

```
select rental_duration, count(title), round(avg(length),2)
from film 
group by rental_duration
order by rental_duration;
```

### 16. Query to list the top 10 longest films with lengths above the average.
This query selects films with lengths greater than the average film length, ordering them by length in descending order.
The result includes the title, length, and rating of each film.
```
select title, length, rating
from film 
where length > (select avg(length) from film)
order by length desc
limit 10;
```

### 17. Query to find the maximum replacement cost, minimum rental rate, and average length of films grouped by rating.
This query groups films by their rating and calculates the maximum replacement cost, minimum rental rate, and average length for each group.

```
select rating, max(replacement_cost), min(rental_rate), avg(length)
from film
group by rating;
```

### 18. Query to list the first 15 films whose titles end with 'K' and their language.
This query joins the `film` and `language` tables to retrieve the title, length, and language name for films whose titles end with the letter 'K'.
The results are ordered by title.

```
select title, length, l.name
from film f
join language l
on f.language_id = l.language_id
where title like '%k' 
order by title
limit 15;
```

### 19. Query to list the first 10 films featuring a specific actor.
This query joins the `film`, `film_actor`, and `actor` tables to retrieve the title, actor's first name, and last name for films featuring the actor with ID 14.
The results are limited to the first 10 films.

```
select f.title, a.first_name, a.last_name
from film f
join film_actor fa using(film_id)
join actor a using(actor_id)
where actor_id = 14
limit 10;
```
