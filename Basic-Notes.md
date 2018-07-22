# SELECT - extracts data from a database
	• * = select all
	• Distinct = no duplicate
	• Select 
	•  (distinct xxx) from yyy--> count the numbers of that variable.
	• Where= conditional  
		○ (e.g.  Where Country = 'USA')
		○ If want to use calculated results, need to specify 'calculated' 
			§ E.g. Select Salary*0.5 as Bonus, where calculated bonus >3000
	• Where not xxx=yyy
	• And and OR
		○ Select… from… Where xxx = yyy or/ and..
	• Having= conditional (but is different from where. Where can't include functions.) 
		○ E.g.  Having count(Customer_Id >5) 
		○ E.g.  Having bonus le 3000 (Note: could be an already calculated results.)
		○ Note: determine which to show. Should put after the group by statement
	• Order by xxx (DESC)
	• STOP x or limit x
		○ Select … from …. Limit 5 -->shows only top 5 results
	• Select Max() or Min() as yyy (naming as yyy)
		○ Select Max(Revenue) as MaxRev
	• Select count() from… =counting numbers
		○ Select count(Customer_ID) from …
	• Case = define a case by yourself
	• Scan = go to the nth word
		○ Select Case scan(Job_Title, -1, ' ') 
			§ When 'I' then Salary*.05
			§ else…..
			§ End as bonus
	• Group by  --> aggregate the group  (should put it after the from statement)
	• In --> specify multiple values in a where clause
		○ Select * from xxx where Country in ( 'Germany', 'USA', 'Japan')
	• Between --> select value from a specific raconge
		○ Select *from Product where price between 100 and 200;
		○ Range could also be Text. Where Product_Name between 'Cheese'  and 'Steak'. 
		○ Not Between: the opposite of between.
		○ Range could also be Dates. Where Order_Date between 07/01/2000 and 07/01/2010
	• Select top xxx rows
		○ Select top 3 * from xxx
		○ Select * from xxx limit 3
		○ Select * from xxx where rownum=3
	• Select top 50% * from xx
	• Concat (like paste in r): use ||
		○ E.g. 
		SELECT first_name || last_name, length(first_name || last_name) 
		FROM actor 
		ORDER BY length(first_name || last_name)  DESC
		LIMIT 10;
		
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


    
 # SQL ALIASES
	• Functions: Give column or table a temporary name.
	• Column: Select Product_name as product
	• Table: 
	SELECT o.OrderID, o.OrderDate, c.CustomerName
	FROM Customers AS c, Orders AS o
	WHERE c.CustomerName="Around the Horn" AND c.CustomerID=o.CustomerID;


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

  
  
 # SQL JOINS
	• Functions: Combining row for two or more table, on a related column between them.
	• Inner Join: (Join the two overlap area: matching values in both tables)
	SELECT o.OrderID, c.CustomerName, o.OrderDate
	FROM Orders (table 1) as o
	INNER JOIN Customers (table 2) as c
	ON o.CustomerID=c.CustomerID (your condition);
	• Left Join: Preserve all the records from the left table, and match the records from the right 
	• Right Join: Preserve all the records from the right
	• Full Join: Either left or right
	
	• (INNER) JOIN: Returns records that have matching values in both tables
	• LEFT (OUTER) JOIN: Return all records from the left table, and the matched records from the right table
	• RIGHT (OUTER) JOIN: Return all records from the right table, and the matched records from the left table
	• FULL (OUTER) JOIN: Return all records when there is a match in either left or right table
	
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
	

# Subqueries
	• Non-correlated: Value are passed from inner to the outer. It's a stand-alone query. 
		○ E.g. 
		select Job_Title, avg(Salary) as MeanSalary
		   from orion.Staff
		   group by Job_Title
		   having avg(Salary) >
		(select avg(salary) as MeanSalary from imcdata.Staff)
		Procedure: From main statement --> outer (select function)
	• Correlated: the outer query must provide information to the subquery before it can be resolved. It's not Stand-alone. In other words, the outer query controls the result set.
		○ E.g. 
		proc sql;
		   select Employee_ID,
		          catx(' ',scan(Employee_Name,2),
		          scan(Employee_Name,1) as Manager_Name
		          length=25
		      from orion.Employee_Addresses
		      where 'AU'=
		        (select Country
		            from Work.Supervisors
		            where Employee_Addresses.Employee_ID=
		                  Supervisors.Employee_ID) ;
		quit;
		○ Procedure: From the outer (select) --> to the main query 
		○ Where exists/ Where not exists:
			§ Exists: if returns at least one row, the results will be 1.
			§ Not exists: the opposite of exists
			§ E.g. 
		proc sql;
		   select Employee_ID, Job_Title
		      from imcdata.Sales 
			 where not exists (select * from imcdata.Order_Fact 
			 where Sales.Employee_ID=Order_Fact.Employee_ID);
			--> Find sales employees at Sales but not in Order_Fact

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

