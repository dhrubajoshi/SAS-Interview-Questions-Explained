# SQL Interview Questions and Answers Part 5

This is a collection of SQL interview question answers. Most of the questions are commonly asked in the SQL Developer/Programmer/Analyst position interview. Every post has covered 10 related questions and answers.

Calculation of nth highest value from data set/Department..

## 01: How do you find max salary from employees table?
There are different ways to find max salary from employees data set.
1. Simply selecting Salary and sorting descinding then  first row gives max salary.

```
SELECT SALARY FROM EMPLOYEES
ORDER BY SALARY DESC;

```
2. Using max() finction.

```
SELECT MAX(SALARY) FROM EMPLOYEES;

```
## 02: Find the second highest salary from the given employees table.

**1.** By using sub query and Max() function we can claculate second highest salary from the given employees table.

```
SELECT MAX(SALARY) FROM EMPLOYEES
WHERE SALARY NOT IN (SELECT MAX(SALARY) FROM EMPLOYEES);

```

**2.** Using Dense_Rank function. This method is useful to calculate nth salary as well.

```
SELECT * FROM
(SELECT SALARY, DENSE_RANK() OVER (SALARY ORDER BY SALARY) AS R_NUM
FROM EMPLOYEES)
WHERE R_NUM=2;

```

## 03: How do you find the id and name of employee who receives 5th highest salary?

```
SELECT * FROM
(SELECT EMPLOYEE_ID, FIRST_NAME ,SALARY, 
DENSE_RANK() OVER (ORDER BY SALARY DESC) AS R_NUM
FROM EMPLOYEES)
WHERE R_NUM=5;

```
Output result:

```
Employee_id First_Name Salary R_Num
201			Michael		13000	5

```


## 04: How do you find the second highest salary from each department.

We can get resuly by using dense_rank function with partition by department_id and left join with table departments. 

Sample query:

```
SELECT A.EMPLOYEE_ID, A.FIRST_NAME, A.SALARY, R_NUM, B. DEPARTMENT_NAME 
FROM (SELECT * FROM(SELECT EMPLOYEE_ID, FIRST_NAME, DEPARTMENT_ID,SALARY, 
DENSE_RANK() OVER (PARTITION BY DEPARTMENT_ID ORDER BY SALARY DESC) AS R_NUM
FROM EMPLOYEES)WHERE R_NUM=2) A
LEFT JOIN DEPARTMENTS B
ON A.DEPARTMENT_ID = B.DEPARTMENT_ID;

```
Output result: It has displayed second highest salary received by employee from each department.

```
Employee_id First_Name Salary R_Num Department_Name
202			Pat			6000	2	Marketing
115			Alexander	3100	2	Purchasing
120			Matthew		8000	2	Shipping
104			Bruce		6000	2	IT
146			Karen		13500	2	Sales
101			Neena		17000	2	Executive
102			Lex			17000	2	Executive
109			Daniel		9000	2	Finance
206			William		8300	2	Accounting

```

## 05: How do  you find number of employees in each department?

```
SELECT B.DEPARTMENT_NAME, CNT 
FROM 
	(SELECT DEPARTMENT_ID, COUNT(*) AS CNT
FROM EMPLOYEES GROUP BY DEPARTMENT_ID) A
LEFT JOIN DEPARTMENTS B
ON A.DEPARTMENT_ID = B.DEPARTMENT_ID;

```
Output result:

```
DEPARTMENT_NAME 	CNT 
Administration		1
Marketing			2
Purchasing			6
Human Resources		1
Shipping			45
IT					5
Public Relations	1
Sales				34
Executive			3
Finance				6
Accounting			2
NULL 				1

```
## 06: Calculate the total salary paid by the company in each department.

```
select B.Department_Name, Total_Salary from 
(select department_id, Sum(Salary) as Total_Salary
from employees group by department_id) A
left join Departments B
on A.Department_Id = B.Department_Id;

```
Output result:

```
Department_Name  	Total_Salary
Administration		4400
Marketing			19000
Purchasing			24900
Human Resources		6500
Shipping			156400
IT					28800
Public Relations	10000
Sales				304500
Executive			58000
Finance				51600
Accounting			20300
NULL 				7000

```
## 07: 

## 08: How do you convert row into column in oracle?
The Oracle PIVOT clause allows us to write a cross-tabulation query which means that we can aggregate our results and rotate rows into columns.

Lets take sales data and for simplicity select only there column:

```
create table Sales_Transpose1 as
select Sales_Date, Product_Id, Sales_Amount
from sales;

Checking rows and columns for sales_Transpose1 Table.
Select * from Sales_Transpose1;

Output result: 10 records are coppied here:
Sales_Date Product_Id Sales_Amount
12-JAN-15	101		400
15-JAN-15	101		400
16-JAN-15	101		400
12-JAN-15	101		400
12-JAN-15	101		400
01-FEB-15	200		1600
01-FEB-15	200		1600
01-JAN-15	100		40
01-JAN-15	101		60
02-JAN-15	100		300


```
Use above Sales_Transpose1 table: Lets create report:

```
select * 
		from 
			(select Product_id as Product
       	 ,to_char(sales_date, 'Mon-yy') as sales_date
        	 ,Sales_amount 
       	 from sales_transpose1
       	 )
 		 pivot
        	(
            sum(Sales_amount)
            for product in (100,101,105,106,200)
           )

```
Above query results output as below:

```
sales_date 100 		101 	105 	106 	200
Jan-15		340		2860	NULL	NULL	NULL
May-15		1140	NULL	NULL 	NULL 	NULL
Mar-15		15320	14720	16140	16880	NULL
Apr-15		26620	30480	800		1260	NULL
Feb-15		4560	4960	7260	4660	4000

## 09: If you need further modified report that contains total value by column and row then what will be your approach?
