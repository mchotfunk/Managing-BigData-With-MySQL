**> note:**

* `IN` lets you specify a lot of values that you would otherwise join together with an `OR` statement
* We can test for `NULL` with `IS NULL`. If we want to filter out '<NA>' rows, we can use `IS NOT NULL` in `WHERE`.
* `WHERE` operators include:

| Operator | Description |
|:---:|:---|
| = | Equal |
| > | Greater than |
| < | Less than |
| >= | Greater than or equal |
| <= | Less than or equal |
| <> or != | Not equal |
| AND | Logical operator AND |
| OR | Logical operator OR |


**> note:**

* All columns except the columns applied with some calculation in the `SELECT` part of the statement have to be in the GROUP BY part, or you'll get an error.
* The [aggregate function](https://www.postgresql.org/docs/9.5/static/functions-aggregate.html) that can be used with `GROUP BY` include:

| Name | Description |
|:---:|:---|
| avg() | Return the average value of the argument |
| count() | Return a count of the number of rows returned |
| count(distinct) | Return the count of a number of different values |
| max() | Return the maximum value |
| min() | Return the minimum value |
| sum() | Return the sum |
| stddev_pop | Return the population standard deviation |
| stddev(), stddev_samp() | Return the sample standard deviation |
| var_pop() | Return the population standard variance |
| variance(), var_samp() | Return the sample variance |
| array_agg() | input arrays concatenated into array of one higher dimension  |
| json_agg | aggregates values as a JSON array | 





# Exercise: Select

1. Get a list of actors with the first name Julia.*

`select * from actor where first_name = 'Julia';`

2. Get a list of actors with the first name Chris, Cameron, or Cuba.

`select * from actor where first_name in ('Chris', 'Cameron', 'Cuba');`

3. Select the row from customer for customer named Jamie Rice.

`select * from customer where first_name = 'Jamie' and last_name= 'Rice';`

4. Select amount and payment_date from payment where the amount paid was less than $1.

`select amount, payment_date from payment where amount<1;`

5. What are the different rental durations that the store allows?

`select max(return_date - rental_date) from rental;`

# Exercise: Counting


1. How many films are rated NC-17? How many are rated PG or PG-13?


`select count (* ) from film where rating='NC-17'`


`select count (* ) from film where rating in ('PG', 'PG-13')`



2. Challenge: How many different customers have entries in the rental table?

`select count(distinct customer_id) from rental;`

# Exercise: Order By

1. What are the IDs of the last 3 customers to return a rental?

`select customer_id from rental order by  rental_date desc  limit 3;`

# Exercise: Like

1. Select film title that have "Dragon" in them.

`select * from  film where title like '%Dragon%';`

2. Challenge: only select titles that have just the word "Dragon" (not "Dragonfly") in them.

`select * from  film where title like '% Dragon' or title like 'Dragon %' ;`

# Exercise: Group By

1. Does the average replacement cost of a film differ by rating?

`select avg(replacement_cost) from film group by rating;`

2. Which store (store_id) has the most customers whose first name starts with M?

`select store_id, count(distinct customer_id) from customer where first_name like 'M%' group by store_id`

3. Challenge: Are there any customers with the same last name?

`SELECT last_name, count(*) FROM customer GROUP BY last_name HAVING count(*) > 1;`

# Exercise: Functions

1. What is the average rental rate of films? Can you round the result to 2 decimal places?

`select avg(rental_rate) from film;`

2. Challenge: What is the average time that people have rentals before returning? Hint: the output you'll get may include a number of hours > 24. You can use the function justify_interval on the result to get output that looks more like you might expect.

`select justify_interval(avg(return_date - rental_date)) from rental ;`

3. Challenge 2: Select the 10 actors who have the longest names (first and last name combined).

`select concat(first_name, last_name), length(concat(first_name, last_name)) from actor 
order by length(concat(first_name, last_name)) desc;`

# Exercise: Count, Group, and Order

1. Which film (id) has the most actors? Which actor (id) is in the most films?


`select film_id, (count(actor_id)) as count from film_actor group by film_id order by count desc limit 1;`

`select actor_id, count(film_id) from film_actor group by actor_id order by count(film_id) desc limit 1 ;`










