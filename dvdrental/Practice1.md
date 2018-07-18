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








