Exercise: Subqueries
What films are actors with ids 129 and 195 in together?

'''
select film_id from 

(

select * from film_actor where actor_id in('129','195')

) as table1;
;;;

Challenge: How many actors are in more films than actor id 47? Hint: this takes 2 subqueries (one nested in the other). Work inside out: 1) how many films is actor 47 in; 2) which actors are in more films than this? 3) Count those actors.


```
Select count(*) from

(

Select actor_id , count(*) from film_actor 
group by actor_id
having count(*)>
(
select count(*) from film_actor 
where actor_id='47')
	
	) foo
;

```

Exercise: Joining Customers, Payments, and Staff
Join the customer and payment tables together with an inner join; select customer id, name, amount, and date and order by customer id. Then join the staff table to them as well to add the staff's name.

```
select c.customer_id, c.first_name, c.last_name, amount, payment_date
from customer c, payment p 
where c.customer_id = p.customer_id
order by c.customer_id;
```

```
select c.customer_id, c.first_name, c.last_name, s.first_name, s.last_name, amount, payment_date
from customer c, payment p, staff s
where c.customer_id=p.customer_id and p.staff_id=s.staff_id
order by customer_id;
```


Exercise: Joining for Better Addresses
Create a list of addresses that includes the name of the city instead of an ID number and the name of the country as well.

```
Select address, city, country from 
address a, city ci, country co
where
a.city_id=ci.city_id
and
ci.country_id=co.country_id;
```


Exercise: Joining and Grouping
Repeating an exercise from Part 1, but adding in information from additional tables: Which film (by title) has the most actors? Which actor (by name) is in the most films?

```
Select first_name, count(title)as count
from 

(
	
Select title, first_name, last_name, fa.actor_id
from film f, film_actor fa, actor a
where f.film_id= fa.film_id 
and fa.actor_id= a.actor_id
	
) foo
group by 
order by count desc;
```


```
Select title, count(actor_id)as count
from 
(
Select title, first_name, last_name, fa.actor_id
from film f, film_actor fa, actor a
where f.film_id= fa.film_id 
and fa.actor_id= a.actor_id

) foo
group by title
order by count desc;
```
Challenge: Which two actors have been in the most films together? Hint: You can join a table to itself by including it twice with different aliases. Hint 2: Try writing the query first to find the answer in terms of actor ids (not names); then for a super challenge (it takes a complicated query), rewrite it to get the actor names instead of the IDs. Hint 3: make sure not to count pairs twice (a in the movie with b and b in the movie with a) and avoid counting cases of an actor being in a movie with themselves.

Exercise: Joining and Grouping 2
Get a list of the names of customers who have spent more than $150, along with their total spending.

Who is the customer with the highest average payment amount?
