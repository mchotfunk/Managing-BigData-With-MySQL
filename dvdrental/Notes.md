# Logical Operators

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

# Subqueries

Subqueries will execute first, and then the result is used the broader query.

```
select title, rental_rate from film  
where rental_rate < (select avg(rental_rate) from film)  
order by rental_rate desc;
```
```
select * from customer
where address_id in 
(select address_id from address where postal_code = '52137')
```

Note: Subquery must have alias 

```
select count(customer_id) as num_customer from
(
SELECT customer_id, count(*) 
 FROM rental GROUP BY customer_id
 HAVING count(*) > 30 
	) as table1 ;
 ```
 
# Joining

**Inner JOIN**

Inner join returns the results in both tables

```
select * from customer inner join address
on customer.address_id = address.address_id;
```
equals to:

```
select * from customer, address
where customer.address_id = address.address_id;`
```

with conditions:

```
select * from customer, address
where customer.address_id = address.address_id
and postal_code= '52137';
```

**Table names and aliases**

Note: you need to specify the table.address_id to avoid ambiguous
```
select first_name, last_name, customer.address_id, postal_code 
from customer, address
where customer.address_id = address.address_id;
```

Makes the reference easier: make alias for tables
Note: the "as" can be dropped

```
select first_name, last_name, c.address_id, postal_code
from customer as c, address as a
where c.address_id= a.address_id;
```

**Joining more than 2 tables**

```
select title, first_name, last_name
from film f, film_actor fa, actor a
where f.film_id=fa.film_id and fa.actor_id = a.actor_id;
```


**Left Join**

```
SELECT f.film_id, title, inventory_id, store_id 
FROM film f LEFT JOIN inventory i
ON f.film_id=i.film_id
where i.film_id is null;
```
