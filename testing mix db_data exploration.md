## Database Information
In this project the database we're using is a mix of a view synthetic tables.

---

### Query to sum the views by device type (laptop and mobile) in the `viewership` table.
This query uses the SUM function combined with CASE WHEN to count the number of views for specific device types.
The CASE WHEN clause acts like an if-else condition inside the SUM function. For example:
1. When the `device_type` is 'laptop', it counts 1, otherwise 0. 
2. The same logic applies for 'tablet' and 'phone', where the counts for these devices are summed together as `mobile_views`.
The result provides the total number of views split between laptops and mobile devices (tablets and phones combined).

```
select 
	sum(case device_type when 'laptop' then 1 else 0 end) as laptop_views,
	sum(case device_type when 'tablet' then 1 else 0 end) 
    	+ sum(case device_type when 'phone' then 1 else 0 end) as mobile_views
from viewership;
```


### Query to identify Subject Matter Experts (SMEs) in Accenture based on their work experience in specific domains.
This query identifies employees who qualify as SMEs (Subject Matter Experts) based on two criteria:
1. The first CASE WHEN condition checks if an employee has 8 or more years of experience in a single domain.
2. The second CASE WHEN condition checks if an employee has 12 or more years of experience across two distinct domains.
The SUM function accumulates the total years of experience, and the COUNT function ensures the correct number of domains are considered.
The HAVING clause filters the results to include only those who meet either of the SME criteria.

```
select 
	employee_id,
    	(case when sum(years_of_experience) >= 8 and count(distinct domain) = 1 then True else False end) as req_1,
	(case when sum(years_of_experience) >= 12 and count(distinct domain) = 2 then True else False end) as req_2
from employee_expertise
group by employee_id
having req_1 = 1 or req_2 = 1;
```


### Query to calculate the total transaction volume per merchant for transactions made via Apple Pay, including merchants with zero Apple Pay transactions.
This query sums up the transaction amounts for each merchant, specifically focusing on transactions made via Apple Pay.
The CASE WHEN clause is used to check if the payment method is 'Apple Pay'. If true, it sums the transaction amount; otherwise, it sums 0.
The result includes all merchants, showing 0 for those without Apple Pay transactions, and orders them by the total transaction volume in descending order.

```
select 
	merchant_id, 
    	sum(case when payment_method = 'Apple Pay' then transaction_amount else 0 end) as total_transaction
from applepay_transactions
group by merchant_id
order by total_transaction desc;
```


### Query to add a grade column (`indeks_nilai`) to the `tabel_nilai` table based on the final score.
This query uses the CASE WHEN clause to assign grades (`indeks_nilai`) based on the `nilai_akhir` (final score).
The grading scale is:
1. 'A' for scores greater than or equal to 80
2. 'B' for scores between 60 and 79
3. 'C' for scores between 40 and 59
4. 'D' for scores between 21 and 39
5. 'E' for scores 20 or below.
The query appends a new column `indeks_nilai` to the existing table, categorizing each student's score accordingly.

```
select *,
		case
        when nilai_akhir >= 80.00 then 'A' 
        when nilai_akhir >= 60.00 then 'B'
        when nilai_akhir >= 40.00 then 'C'
        when nilai_akhir > 20.00 then 'D'
        when nilai_akhir <= 20.00 then 'E'
        end as indeks_nilai
from tabel_nilai;
```
