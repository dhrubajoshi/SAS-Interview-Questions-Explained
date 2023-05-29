# SAS Interview Questions and Answers Part 5

This is a collection of SAS interview question answers. Most of the questions are commonly asked in the SAS Developer/Programmer position interview. Every post has covered 10 related questions and answers.

Data step Merge and Proc step Joins (Part-2):

## 01: How do you join two or more than two tables vertically using proc step in sas? Assuming that the result should display all record from the all tables.
We use vertical join with union all operator.

```
DATA DATA_TABLE1;
INPUT PRODUCT_ID $ STATE $ AMOUNT DOLLARS5.2;
DATALINES;
0123 VA 67876
2291 MD 66622
0023 AZ 90877
0111 KS 22778
1234 IL 78778
;
RUN;
DATA DATA_TABLE2;
INPUT PRODUCT_ID $ STATE $ AMOUNT DOLLARS5.2;
DATALINES;
0123 VA 99876
2211 PA 66222
0123 AZ 90877
0111 KS 22778
1234 IL 78778
;
RUN;

PROC SQL;
CREATE TABLE COMBINE AS
SELECT * FROM DATA_TABLE1
UNION ALL
SELECT * FROM DATA_TABLE2
RUN;

```
Output result:

```
	PRODUCT_ID	STATE	AMOUNT	
	0123		VA		67876	
	2291		MD		66622	
	0023		AZ		90877	
	0111		KS		22778	
	1234		IL		78778	
	0123		VA		99876	
	2211		PA		66222	
	0123		AZ		90877	
	0111		KS		22778	
	1234		IL		78778

```
## 02: How do you extract the records which are common in both tables?
To get common record from both table we use intersect operator.

```
DATA DATA_TABLE1;
INPUT PRODUCT_ID $ STATE $ AMOUNT DOLLARS5.2;
DATALINES;
0123 VA 67876
2291 MD 66622
0023 AZ 90877
0111 KS 22778
1234 IL 78778
;
RUN;
DATA DATA_TABLE2;
INPUT PRODUCT_ID $ STATE $ AMOUNT DOLLARS5.2;
DATALINES;
0123 VA 99876
2211 PA 66222
0123 AZ 90877
0111 KS 22778
1234 IL 78778
;
RUN;

PROC SQL;
CREATE TABLE COMMON_REC AS
SELECT * FROM DATA_TABLE1
INTERSECT
SELECT * FROM DATA_TABLE2
RUN;

```
Output result:

```
	PRODUCT_ID	STATE	AMOUNT
	0111		KS		22778
	1234		IL		78778

```

## 03: How do you extract the records which are in Table1 but not in Table2?
To get record which are in Table1 but not in Table2 we use except operator.

```
DATA DATA_TABLE1;
INPUT PRODUCT_ID $ STATE $ AMOUNT DOLLARS5.2;
DATALINES;
0123 VA 67876
2291 MD 66622
0023 AZ 90877
0111 KS 22778
1234 IL 78778
;
RUN;
DATA DATA_TABLE2;
INPUT PRODUCT_ID $ STATE $ AMOUNT DOLLARS5.2;
DATALINES;
0123 VA 99876
2211 PA 66222
0123 AZ 90877
0111 KS 22778
1234 IL 78778
;
RUN;

PROC SQL;
CREATE TABLE COMMON_REC AS
SELECT * FROM DATA_TABLE1
EXCEPT
SELECT * FROM DATA_TABLE2
RUN;

```
Output result:

```
	PRODUCT_ID	STATE	AMOUNT
	0023		AZ		90877
	0123		VA		67876
	2291		MD		66622

```
## 04: How do you extract the records which are in Table2 but not in Table1?
To get record which are in Table2 but not in Table1 we use except operator.

```
DATA DATA_TABLE1;
INPUT PRODUCT_ID $ STATE $ AMOUNT DOLLARS5.2;
DATALINES;
0123 VA 67876
2291 MD 66622
0023 AZ 90877
0111 KS 22778
1234 IL 78778
;
RUN;
DATA DATA_TABLE2;
INPUT PRODUCT_ID $ STATE $ AMOUNT DOLLARS5.2;
DATALINES;
0123 VA 99876
2211 PA 66222
0123 AZ 90877
0111 KS 22778
1234 IL 78778
;
RUN;

PROC SQL;
CREATE TABLE COMMON_REC AS
SELECT * FROM DATA_TABLE2
EXCEPT
SELECT * FROM DATA_TABLE1
RUN;

```
Output result:

```
	PRODUCT_ID	STATE	AMOUNT
	0123		AZ		90877
	0123		VA		99876
	2211		PA		66222

```
## 05: How do you get all records from Table1 and common records exist in both table1 and table2 using sas proc step?
SAS proc step left join can be used to get required result.

```
DATA DATA_PRODUCT;
INPUT PRODUCT_ID $ NAME $ 20.;
DATALINES;
0123 GRAPE
2222 ORANGE
0023 BANANA
0111 PAPAYA
1234 GUAVA
2222 STRAWBERRY
4423 BLACKBERRY
;
RUN;

DATA DATA_SALES;
INPUT PRODUCT_ID $ STATE $ AMOUNT DOLLARS5.2;
DATALINES;
0123 VA 67876
2291 MD 66622
0023 AZ 90877
0111 KS 22778
1234 IL 78778
2233 CA 89098
;
RUN;

proc sql;
create table Left_Join as
select * from Data_Product as p left join Data_Sales as s
on p.product_id=s.product_id;
quit;

```
Output result:

```
	PRODUCT_ID	NAME		STATE	AMOUNT
	0023		BANANA		AZ		90877
	0111		PAPAYA		KS		22778
	0123		GRAPE		VA		67876
	1234		GUAVA		IL		78778
	2222		STRAWBERRY				.
	2222		ORANGE					.
	4423		BLACKBERRY				.

```
## 06: How do you get all records from Table2 and common records exist in both table1 and table2 using sas proc step?
Following SAS proc step right join is used to get desired result.

```
DATA DATA_PRODUCT;
INPUT PRODUCT_ID $ NAME $ 20.;
DATALINES;
0123 GRAPE
2222 ORANGE
0023 BANANA
0111 PAPAYA
1234 GUAVA
2222 STRAWBERRY
4423 BLACKBERRY
;
RUN;

DATA DATA_SALES;
INPUT PRODUCT_ID $ STATE $ AMOUNT DOLLARS5.2;
DATALINES;
0123 VA 67876
2291 MD 66622
0023 AZ 90877
0111 KS 22778
1234 IL 78778
2233 CA 89098
;
RUN;

proc sql;
create table Left_Join as
select * from Data_Product as p right join Data_Sales as s
on p.product_id=s.product_id;
quit;

```
Output result:
Product_Id which are exist only on DataSales are missing.

```
	PRODUCT_ID	NAME	STATE	AMOUNT
	0023		BANANA	AZ		90877
	0111		PAPAYA	KS		22778
	0123		GRAPE	VA		67876
	1234		GUAVA	IL		78778
						CA		89098
						MD		66622

```
## 07: Refering question no. 6, how do you solve the missing Product_Id issue?
SQL COALESCE function can be used to solve the issue. The COALESCE function returns the first non-missing argument.

```
DATA DATA_PRODUCT;
INPUT PRODUCT_ID $ NAME $ 20.;
DATALINES;
0123 GRAPE
2222 ORANGE
0023 BANANA
0111 PAPAYA
1234 GUAVA
2222 STRAWBERRY
4423 BLACKBERRY
;
RUN;

DATA DATA_SALES;
INPUT PRODUCT_ID $ STATE $ AMOUNT DOLLARS5.2;
DATALINES;
0123 VA 67876
2291 MD 66622
0023 AZ 90877
0111 KS 22778
1234 IL 78778
2233 CA 89098
;
RUN;

PROC SQL;
CREATE TABLE LEFT_JOIN AS
SELECT COALESCE(P.PRODUCT_ID, S.PRODUCT_ID) AS PRODUCT_ID, NAME,
STATE, AMOUNT
FROM DATA_PRODUCT AS P RIGHT JOIN DATA_SALES AS S
ON P.PRODUCT_ID=S.PRODUCT_ID;
QUIT;

```
Output result:

```
	Product_Id	NAME	STATE	AMOUNT
	0023		BANANA	AZ		90877
	0111		PAPAYA	KS		22778
	0123		GRAPE	VA		67876
	1234		GUAVA	IL		78778
	2233				CA		89098
	2291				MD		66622

```
## 08: How do you get all records from Table1 and Table2 using sas proc step?
SAS proc step full join can be used to get required result.

```
DATA DATA_PRODUCT;
INPUT PRODUCT_ID $ NAME $ 20.;
DATALINES;
0123 GRAPE
2222 ORANGE
0023 BANANA
0111 PAPAYA
1234 GUAVA
2222 STRAWBERRY
4423 BLACKBERRY
;
RUN;

DATA DATA_SALES;
INPUT PRODUCT_ID $ STATE $ AMOUNT DOLLARS5.2;
DATALINES;
0123 VA 67876
2291 MD 66622
0023 AZ 90877
0111 KS 22778
1234 IL 78778
2233 CA 89098
;
RUN;

PROC SQL;
CREATE TABLE FULL_JOIN AS
SELECT * FROM DATA_PRODUCT AS P FULL JOIN DATA_SALES AS S
ON P.PRODUCT_ID=S.PRODUCT_ID;
QUIT;

```
Output result:
Product_Id which are exist only on DataSales are missing.

```

	PRODUCT_ID	NAME	STATE	AMOUNT
	0023		BANANA		AZ	90877
	0111		PAPAYA		KS	22778
	0123		GRAPE		VA	67876
	1234		GUAVA		IL	78778
	2222		STRAWBERRY		.
	2222		ORANGE				.
							CA	89098
							MD	66622
	4423		BLACKBERRY			.

```
SQL COALESCE function can be used to solve the issue. The COALESCE function returns the first non-missing argument.

```
PROC SQL;
CREATE TABLE FULL_JOIN AS
SELECT COALESCE(P.PRODUCT_ID, S.PRODUCT_ID) AS PRODUCT_ID, NAME,
STATE, AMOUNT
FROM DATA_PRODUCT AS P FULL JOIN DATA_SALES AS S
ON P.PRODUCT_ID=S.PRODUCT_ID;
QUIT;

```
Output result:

```
	Product_Id	NAME	STATE	AMOUNT
	0023		BANANA		AZ	90877
	0111		PAPAYA		KS	22778
	0123		GRAPE		VA	67876
	1234		GUAVA		IL	78778
	2222		STRAWBERRY			.
	2222		ORANGE				.
	2233					CA	89098
	2291					MD	66622
	4423		BLACKBERRY			.

```
## 09: How do you get all common records exist in both table1 and table2 using sas proc step?
SAS proc step inner join can be used to get required result.

```
DATA DATA_PRODUCT;
INPUT PRODUCT_ID $ NAME $ 20.;
DATALINES;
0123 GRAPE
2222 ORANGE
0023 BANANA
0111 PAPAYA
1234 GUAVA
2222 STRAWBERRY
4423 BLACKBERRY
;
RUN;

DATA DATA_SALES;
INPUT PRODUCT_ID $ STATE $ AMOUNT DOLLARS5.2;
DATALINES;
0123 VA 67876
2291 MD 66622
0023 AZ 90877
0111 KS 22778
1234 IL 78778
2233 CA 89098
;
RUN;

PROC SQL;
CREATE TABLE FULL_JOIN AS
SELECT * FROM DATA_PRODUCT AS P INNER JOIN DATA_SALES AS S
ON P.PRODUCT_ID=S.PRODUCT_ID;
QUIT;

```
Output result:

```
	PRODUCT_ID	NAME	STATE	AMOUNT
	0123		GRAPE	VA		67876
	0023		BANANA	AZ		90877
	0111		PAPAYA	KS		22778
	1234		GUAVA	IL		78778

```
## 10: What do you mean self join and where do you use it?
A self join is a type of join where a table is joined with itself. It allows you to retrieve related information within the same table by creating a temporary view of the table. 
The self join is commonly used in processing a hierarchy table.
 
 ```
DATA EMPLOYEES;
INPUT ID NAME $  SALARY  MANAGERID;
DATALINES;
1 TOM 10000 3
2 ANNE 12000 3
3 JESS 15000 4
4 JEFF 20000 .  
5 MIKE 9000 1
6 BOB 15000 2
;
RUN;

PROC SQL;
CRDATE TABLE SELF_JOIN AS
SELECT 
EMPLOYEE.ID
,EMPLOYEE.NAME
,EMPLOYEE.MANAGERID
,MANAGER.NAME AS MANAGERNAME
FROM EMPLOYEES EMPLOYEE
LEFT JOIN EMPLOYEES MANAGER
ON EMPLOYEE.MANAGERID = MANAGER.ID
ORDER BY ID;
QUIT;
 ```
Output result:

   ```
	Id	Name	ManagerId	ManagerName
	1	Tom		3			Jess
	2	Anne	3			Jess
	3	Jess	4			Jeff
	4	Jeff	.	
	5	Mike	1			Tom
	6	Bob		2			Anne

   ```
   
   

   [<img align="center" src="../static/images/arrow_left.svg" height="20" width="20"/> Part 4](./Interview_QA_Post4_05_26_2023.md)&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;[Part 6 <img align="center" src="../static/images/arrow_right.svg" height="20" width="20"/>](./Interview_QA_Post6_05_27_2023.md)