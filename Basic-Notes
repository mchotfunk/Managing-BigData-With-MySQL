
• SELECT - extracts data from a database
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
    
• SQL ALIASES
	• Functions: Give column or table a temporary name.
	• Column: Select Product_name as product
	• Table: 
	SELECT o.OrderID, o.OrderDate, c.CustomerName
	FROM Customers AS c, Orders AS o
	WHERE c.CustomerName="Around the Horn" AND c.CustomerID=o.CustomerID;
  
  
• SQL JOINS
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
	
• Subqueries
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


• INSERT INTO - inserts new data into a database
• CREATE DATABASE - creates a new database
• ALTER DATABASE - modifies a database
• CREATE TABLE - creates a new table
• ALTER TABLE - modifies a table
• DROP TABLE - deletes a table
• CREATE INDEX - creates an index (search key)
• DROP INDEX - deletes an index

