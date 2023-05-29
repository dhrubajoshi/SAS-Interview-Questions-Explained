# SAS Interview Questions and Answers Part 5

This is a collection of SAS interview question answers. Most of the questions are commonly asked in the SAS Developer/Programmer position interview. Every post has covered 10 related questions and answers.

Data step Merge and Proc step Joins:
## 01: What are the differences between Join and Merge? Which one is more efficient and why?
1. Both methods are used to combine the two or more than two data sets. Join doesn’t need to sort the data but merge need to sort the data by index or by variable. 
2. Multiple data sets can be joined in one step without having common variables in all the data sets.
3. Proc SQL join can handle many to many relationships whereas Data step merge do not.

## 02: What are the difference types of joins?
There are five different types of joins:

1. Self join: Given table is joined with same table -Example: When using employee table, self join is used to idrntify Manager's name.
2. Inner Join: This jine is used to identify (Extract) all rows which are common to the both table.
3. Left join: Left join is used to Select all the records from left table and records from second table which has common rows with first table.
4. Right Join: Right join is used to Select all the records from right table and records from second table which has common rows with first table.
5. Full outer join: Full outer join is used to get all records from both table.


## 03: Demographic info and account info are saved in different table. How do you pull name of all customers whose id exist in the account table.
We can use either data step merge with certsin condition or proc step left join. 

```
Data Demographic;
input ID FName $ LName $ Department $;
Datalines;
101 Mark 

```


## 04: How do you join daily data set to monthly directory?
While working on joining daily data to monthly directory, we use vertical join. Either we use union or union all operator or data step single set statement or proc append with the base table.


## 05: You need to join two table having same fields. Manager has doubt that some record may have repeated. How do you get the records without duplicating in combined file?
We use vertical join with union operator.

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
UNION 
SELECT * FROM DATA_TABLE2
RUN;

```
Output result:

```

	PRODUCT_ID	STATE	AMOUNT	
1	0023		AZ		90877	
2	0111		KS		22778	
3	0123		AZ		90877	
4	0123		VA		67876	
5	0123		VA		99876	
6	1234		IL		78778	
7	2211		PA		66222	
8	2291		MD		66622

```



## 06: You are validating the data migration from source to target. How do you validate the two-table data? 
We use vertical join with except operatpr between the two table.

```
DATA DATA_TARGET;
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
CREATE TABLE COMBINE AS
SELECT * FROM DATA_SOURCE
EXCEPT
SELECT * FROM DATA_TARGET
RUN;

```
Log output:

```
NOTE: Table WORK.COMBINE created, with 0 rows and 3 columns.

```


## 07: How do you get all records from Table1 and common records exist in both table1 and table2 using sas data step?
SAS Data step merge can be used to get required result with suitable conditional IN=Option.

```
DATA DATA_PRODUCT;
INPUT PRODUCT_ID $ NAME $ 20.;
DATALINES;
0123 GRAPE
2222 ORANGE
0023 BANANA
0111 PAPAYA
1234 GAUVA
2222 STRABERRY
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

PROC SORT DATA =DATA_PRODUCT;
BY PRODUCT_ID;
RUN;
PROC SORT DATA =DATA_SALES;
BY PRODUCT_ID;
RUN;

DATA COMB_DATA_STEP;
MERGE DATA_PRODUCT(IN=A) DATA_SALES(IN=B);
BY PRODUCT_ID;
IF A THEN OUTPUT;
RUN;
```
Output result:

```
	Product_id	Name		State	Amount	
1	0023		BANANA		AZ		90877	
2	0111		PAPAYA		KS		22778	
3	0123		Grape		VA		67876	
4	1234		GAUVA		IL		78778	
5	2222		ORANGE					.	
6	2222		STRABERRY				.	
7	4423		BLACKBERRY  			.

```




## 08: How do you get all common records from Table1 and table2 based on product-id using SAS data step?
common records from both table can be obtained using suitable IN = Option as explained below.

```
DATA DATA_PRODUCT;
INPUT PRODUCT_ID $ NAME $ 20.;
DATALINES;
0123 GRAPE
2222 ORANGE
0023 BANANA
0111 PAPAYA
1234 GAUVA
2222 STRABERRY
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

PROC SORT DATA =DATA_PRODUCT;
BY PRODUCT_ID;
RUN;
PROC SORT DATA =DATA_SALES;
BY PRODUCT_ID;
RUN;

DATA COMB_DATA_STEP;
MERGE DATA_PRODUCT(IN=A) DATA_SALES(IN=B);
BY PRODUCT_ID;
IF A AND B THEN OUTPUT;
RUN;

```
Output result:

```
	Product_id	Name	State	Amount
1	0023		BANANA	AZ		90877
2	0111		PAPAYA	KS		22778
3	0123		Grape	VA		67876
4	1234		GAUVA	IL		78778

```
## 09: How do you get all records from Table2 and common records exist in both table1 and table2 using sas data step?
SAS Data step merge can be used to get required result with suitable conditional IN=Option.

```
DATA DATA_PRODUCT;
INPUT PRODUCT_ID $ NAME $ 20.;
DATALINES;
0123 GRAPE
2222 ORANGE
0023 BANANA
0111 PAPAYA
1234 GAUVA
2222 STRABERRY
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

PROC SORT DATA =DATA_PRODUCT;
BY PRODUCT_ID;
RUN;
PROC SORT DATA =DATA_SALES;
BY PRODUCT_ID;
RUN;

DATA COMB_DATA_STEP;
MERGE DATA_PRODUCT(IN=A) DATA_SALES(IN=B);
BY PRODUCT_ID;
IF B THEN OUTPUT;
RUN;

```
Output result:

```
	Product_id	Name	State	Amount
1	0023		BANANA	AZ		90877
2	0111		PAPAYA	KS		22778
3	0123		Grape	VA		67876
4	1234		GAUVA	IL		78778
5	2233				CA		89098
6	2291				MD		66622

```

## 10: How do you get all records from Table1 and Table2 using sas data step?
Following conditional option is used to get all records from Table1 and Table2.

```
DATA DATA_PRODUCT;
INPUT PRODUCT_ID $ NAME $ 20.;
DATALINES;
0123 GRAPE
2222 ORANGE
0023 BANANA
0111 PAPAYA
1234 GAUVA
2222 STRABERRY
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

PROC SORT DATA =DATA_PRODUCT;
BY PRODUCT_ID;
RUN;
PROC SORT DATA =DATA_SALES;
BY PRODUCT_ID;
RUN;

DATA COMB_DATA_STEP;
MERGE DATA_PRODUCT(IN=A) DATA_SALES(IN=B);
BY PRODUCT_ID;
IF A OR B THEN OUTPUT;
RUN;

```
Output result:

```
	 
Product_id	Name	State	Amount	
1	0023	BANANA		AZ	90877	
2	0111	PAPAYA		KS	22778	
3	0123	Grape		VA	67876	
4	1234	GAUVA		IL	78778	
5	2222	STRABERRY			.	
6	2222	ORANGE				.	
7	2233				CA	89098	
8	2291				MD	66622	
9	4423	BLACKBERRY			.

```





[<img align="center" src="../static/images/left.svg" height="20" width="20"/> Part 2](./Interview_QA_Post2_05_24_2023.md)&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;[Part 4 <img align="center" src="../static/images/right.svg" height="20" width="20"/>](./Interview_QA_Post4_05_26_2023.md)
