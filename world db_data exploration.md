
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

## Query to calculate the difference in GNP (GNP - GNPOLD) per region in the `country` table, filtering regions with a positive difference.
select region, sum(gnp - gnpold) as selisih
from country
group by region 
having selisih > 0
order by selisih desc;
