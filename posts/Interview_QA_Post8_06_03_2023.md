> # SAS Interview Questions and Answers Part 8

This is a collection of SAS interview question answers. Most of the questions are commonly asked in the SAS Developer/Programmer position interview. Every post has covered 10 related questions and answers.

SAS Functions continue:

## 01: Due to unexpected circumstances like COVID-19, senior policymakers have granted a 2-year extension for the ending period of all student loan accounts. How can you apply this rule to all student loan accounts?

SAS Date function INTNX() can be used in sas data step to apply extension period for all loan account.
Loan_Ending_Date=intnx('year ', Loan_ending_Date , 2,'s');

```
DATA SAMPLE_ACCOUNT;
INPUT ID NAME $ LOAN_ENDING_DATE;
INFORMAT LOAN_ENDING_DATE MMDDYY10.;
FORMAT LOAN_ENDING_DATE DATE9.;
DATALINES;
101 JACK 12/11/2031
102 MIKE 09/21/2040
103 SHIV 03/08/2034
104 JESS 07/22/2033
105 GEORGE 11/04/2029
;
RUN;

DATA DATE_CHANGED;
SET SAMPLE_ACCOUNT;
LOAN_ENDING_DATE=INTNX('YEAR', LOAN_ENDING_DATE, 2,'S');
RUN;

```

## 02: How can you add leading zeros in sas variable?

Adding leading zeros in a numeric variable.

```
Data Sample1;
input ID Name$;
Datalines;
12 Jaffery
105 Robert 
1030 Jany
;
run;

Data Leading_zero_added(drop=ID);
set Sample1;
ID_1 =put(ID, z5.);
rename ID_1=ID;
run;

output:

```

Adding leading zeros in a character variable.

```
Data Sample2;
input ID$  Name$;
Datalines;
B12 Jaffery
C105 Robert 
B1030 Jany
A103 Mary
;
run;
Data Leading_zero_added(drop=ID);
set Sample2;
ID_1 =cats(repeat('0',5-length(ID)-1), ID);
rename ID_1=ID;
run;
 
```

## 03: What is the difference between SCAN and SUBSTR charactefr function?

SCAN extracts words within a value that is marked by delimiters. 
SUBSTR extracts a portion of the value by stating the specific location. It is best used when we know the exact position of the sub string to extract from a character value.

Sample program:

```
Data Scan_Substr;
Name1=“Washington DC is Capital of United States of America”;
Name1_Scan=scan(Name1,2,’ ’);
Name1_Substr=substr(Name1,1,4);
Run;

```
Output result:

```
73   put Name1_Scan;
74   put Name1_Substr;
75   Run;
 
 DC
 Wash

```
Scan function extract second word which is delimited by space ' '(If there are other delimiter option then just keep them inside the quote.

Substr function extracts letter starting from first and go upto 4.

## 04: How do you remove leading and trailing space of a character variable using single function?
STRIP function is used to remove leading and trailing blanks. If the argument is blank, STRIP returns a string with a length of zero.

Sample Program:

```
data Blank;
input Name $char14.;
datalines;
Mary Hasmann 
 John Park 
Krish Xu 
 ;
data LT_Removed;
set Blank;
 Name1 = '*' || Name || '*';
 Name2 = '*' || strip(Name) || '*';
run;
proc print noobs data=LT_Removed;
title2 'Output of Strip';
run;
```
  
 Output result:
 
 ````
Output of Strip
Name			Name1			Name2
Mary Hasmann	*Mary Hasmann *	*Mary Hasmann*
John Park		* John Park *	*John Park*
Krish Xu		*Krish Xu *		*Krish Xu*

```

## 05: How do you check if there are duplicate records present in a sas dataset?

First count each record and only print those records whose count is greater than 1. If it gives output result then there are duplicate records.

Sample dataset.

```
DATA EMPLOYEE;
INPUT E_ID NAME $ SALARY;
DATALINES;
1 JEFF 4500
1 ROBERT 8750
2 SAMI 8000
2 SAMI 8000
3 JESS 8700
1 MARK 8000
3 MARY 6200
3 JESS 8700
4 BOB 6500
5 JENNY 7000
7 JENNY 7050
5 KEN 8700
6 DAN 6500
;
RUN;

Proc sql;
select *, count(*) as rec_Count
from Employee
group by E_ID, Name, Salary
having rec_count > 1;
quit;

```
Output result:

```
E_ID	NAME	SALARY	rec_Count
2		SAMI	8000	2
3		JESS	8700	2

```
Result shows there are two records (E_ID=2 &2) are duplicated.

## 06: How do you remove duplicate records from a sas dataset?
There are different ways to remove duplicate records from sas dataset. 

1. proc sort with nodup and nodupkey options.
2. first.var and last.var 

**Proc sort with nodup option:** This method sorts the data by all variable and displays output without duplication(Useing Employee table from 05).

```
proc sort data = employee nodup;
by _all_;
run;

```
Output result:

```
	E_ID	NAME	SALARY
	1		JEFF	4500
	1		MARK	8000
	1		ROBERT	8750
	2		SAMI	8000
	3		JESS	8700
	3		MARY	6200
	4		BOB		6500
	5		JENNY	7000
	5		KEN		8700
	6		DAN		6500
	7		JENNY	7050

```
**Proc sort with nodupkey option:** This method removes the records which are duplicated by given variable.

```
proc sort data = employee nodupkey;
by E_ID;
run;

```
Output result:

```
	E_ID	NAME	SALARY
	1		JEFF	4500
	2		SAMI	8000
	3		JESS	8700
	4		BOB		6500
	5		JENNY	7000
	6		DAN		6500
	7		JENNY	7050
	
```
## 07: How can you remove duplicate records using first.var and last.var in sas data step?
If we need to identify duplicate recors using a variable then need to sort the data by given variable. Next step we use first.var and last.var options.

```
proc sort data=employee;
run;
by E_id;
data New_Data;
set employee;
1. by E_ID;
if first.E_Id then output;
run; 

```
Output result displays first record of each E_ID;

```
	E_ID	NAME	SALARY
	1		JEFF	4500
	2		SAMI	8000
	3		JESS	8700
	4		BOB		6500
	5		JENNY	7000
	6		DAN		6500
	7		JENNY	7050

```
## 08: How do you seperate all the records whose E_ID present only one time?

If there exist first.E_ID and Last.E_ID then those records fulfill our requirement.

```
proc sort data=employee;
run;
by E_id;
data New_Data;
set employee;
1. by E_ID;
if first.E_Id and last.E_ID then output;
run; 

```
Output result:

```
	E_ID	NAME	SALARY
	4		BOB		6500
	6		DAN		6500
	7		JENNY	7050

```

## 09: How do you store removed duplicate record in a table?


```
proc sort data=employee;
by E_id;
data Without_Dup Deleted_Rec;
set employee;
by E_ID;
if first.E_Id then output without_dup;
else output Deleted_Rec;
run; 

```
Output result:

```
Deleted_Rec:
	E_ID	NAME	SALARY	
	1		MARK	8000	
	1		ROBERT	8750	
	2		SAMI	8000	
	3		JESS	8700	
	3		MARY	6200	
	5		KEN		8700
	
```

## 10: How do you seperate distinct and duplicate records into different table?
Using data set from 05. Following code helps to seperate distinct and duplicate records into different table.

```
%let varlst= E_ID Name Salary;
proc sort data=employee;
run;
by &varlst;
data unique dups;
set employee;
by &varlst;
if first.salary and last.Salary then output unique;
else output dups;
run; 

```
Output result:

```
Unique records:
	E_ID	NAME	SALARY	
	1		JEFF	4500	
	1		MARK	8000	
	1		ROBERT	8750	
	3		MARY	6200	
	4		BOB		6500	
	5		JENNY	7000	
	5		KEN		8700	
	6		DAN		6500	
	7		JENNY	7050

Dups records:
	E_ID	NAME	SALARY	
	2		SAMI	8000	
	2		SAMI	8000	
	3		JESS	8700	
	3		JESS	8700

```


[<img align="center" src="../static/images/arrow_left.svg" height="20" width="20"/> Part 2](./Interview_QA_Post7_05_29_2023.md)&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;[Part 4 <img align="center" src="../static/images/arrow_right.svg" height="20" width="20"/>](./Interview_QA_Post4_05_26_2023.md)
