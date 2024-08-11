
## Database Information
This project use the database provided by MySQL called [The World Database](https://dev.mysql.com/doc/world-setup/en/). This project mostly tackles some of the exploratory data analysis that can be done in MySQL.


### 1. Query to count the number of distinct regions in the `country` table.
  ```
  select count(distinct(region)) as jumlah_region
  from country;
  ```

### 2. Query to count the number of distinct countries in Africa in the `country` table.
  ```
  select count(distinct(name)) as jumlah_negara_di_afrika
  from country
  where continent = 'Africa';
  ```

### 3. Query to list the top 5 countries with the highest population in the `country` table.
  ```
  select name, population
  from country
  order by population desc
  limit 5;
  ```

### 4. Query to find the average life expectancy per continent in the `country` table.
  ```
  select continent, avg(lifeexpectancy) as rerata_harapan_hidup
  from country
  group by continent
  order by rerata_harapan_hidup;
  ```

### 5. Query to find the number of regions per continent in the `country` table, filtered by continents having more than 3 regions.
  ```
  select continent, count(distinct(region)) as jumlah_region
  from country
  group by continent
  having jumlah_region > 3;
  ```

### 6. Query to find the average GNP per region in Africa in the `country` table.
  ```
  select region, avg(gnp) as rerata_gnp
  from country
  where continent = 'Africa'
  group by region
  order by rerata_gnp desc;
  ```

### 7. Query to list the countries in Europe that start with the letter 'I' in the `country` table.
  ```
  select name, continent
  from country
  where name like 'I%'
  and continent = 'Europe';
  ```

### 8. Query to count the number of distinct languages spoken per country in the `countrylanguage` table.
  ```
  select countrycode, count(distinct language) as jumlah_bahasa
  from countrylanguage
  group by countrycode
  order by jumlah_bahasa desc;
  ```

### 9. Query to list the countries with names that have 'o' as the sixth letter in the `country` table.
  ```
  select name
  from country
  where name like '_____o';
  ```

### 10. Query to find the percentage of each language spoken in Indonesia in the `countrylanguage` table.
  ```
  select language as bahasa, round(percentage) as persentase
  from countrylanguage
  where countrycode = 'IDN'
  order by persentase DESC;
  ```

### 11. Query to list the first 10 countries that gained independence in the `country` table.
  ```
  select name, indepyear
  from country
  where indepyear is not null
  order by indepyear asc
  limit 10;
  ```

### 12. Query to count the number of distinct forms of government per continent in the `country` table, filtered by continents having fewer than 10 forms.
  ```
  select continent as benua, count(distinct governmentform) as banyaknya_bentuk_pemerintahan
  from country
  group by benua
  having banyaknya_bentuk_pemerintahan < 10;
  ```

## 13. Query to calculate the difference in GNP (GNP - GNPOLD) per region in the `country` table, filtering regions with a positive difference.
  ```
  select region, sum(gnp - gnpold) as selisih
  from country
  group by region 
  having selisih > 0
  order by selisih desc;
  ```


## 14. Query to calculate the percentage of the population for the top 10 most populated cities.
This query calculates the percentage of the population for each of the top 10 most populated cities relative to the total population of all cities.
The subquery inside the SELECT clause divides the population of each city by the total population and multiplies by 100 to get the percentage.
The result includes the city name, the country it belongs to, and the population percentage, ordered by population in descending order.

```
select 
	cty.name as nama_kota, 
	ctr.name as asal_negara,
    (select round(cty.population/sum(population)*100,2) from city) as presentase
from city cty
join country ctr 
on countrycode = code
order by cty.population desc
limit 10;
```

## 15. Query to find the top 10 countries in Asia with a higher life expectancy than the average in Europe.
This query compares the life expectancy of countries in Asia against the average life expectancy in Europe.
The subquery calculates the average life expectancy for European countries, and the main query selects countries in Asia with a life expectancy higher than that average.
The result includes the country name, continent, life expectancy, and GNP, ordered by life expectancy in descending order.

```
select name, continent, lifeexpectancy, gnp
from country
where continent = 'Asia' and lifeexpectancy > (select avg(lifeexpectancy) from country where continent = 'Europe')
order by lifeexpectancy desc
limit 10;
```

## 16. Query to list the top 10 countries where English is the official language and is spoken by the highest percentage of the population.
This query identifies countries where English is an official language and orders them by the percentage of the population that speaks English.
The query joins the `country` and `countrylanguage` tables to fetch the relevant information, ensuring only countries with English as an official language (isofficial = 'T') are included.
The result is ordered by the percentage of English speakers in descending order.

```
select code, c.name, cl.language, isofficial, percentage
from country c
join countrylanguage cl
on code = countrycode
where language = 'English' and isofficial = 'T'
order by percentage desc
limit 10;
```

## 17. Query to list countries in Southeast Asia with their capitals and population details.
This query fetches the country names, their population, GNP, and capital city details (name and population) for countries in the Southeast Asia region.
The query joins the `country` and `city` tables based on the capital city ID and orders the results by country name.

```
select country.name, country.population, gnp, city.name as ibukota, city.population as populasi_ibukota
from country
join city
on capital = id
where region = 'Southeast Asia'
order by country.name;
```

## 18. Query to list selected G20 countries with their capital and population details.
This query fetches similar information as the previous one but is limited to specific G20 countries.
It retrieves the country names, population, GNP, and capital city details (name and population) and orders the results by country name.

```
select country.name ,country.population, gnp, city.name as ibukota, city.population as populasi_ibukota
from country
join city
on capital = id
where country.name in ('Argentina', 'Australia', 'Brazil', 'Canada', 'China', 'France', 'Germany', 'India', 'Indonesia', 'Italy', 'Japan', 'South Korea', 'Mexico', 'Russian Federation', 'Saudi Arabia', 'South Africa', 'Turkey', 'United Kingdom', 'United States');
```
