# SAS Interview Questions and Answers Part 3

This is a collection of SAS interview question answers. Most of the questions are commonly asked in the SAS Developer/Programmer position interview. Every post has covered 10 related questions and answers.


## 01: What are the SAS character functions that you have used for data cleaning?
Frequently used character functions are:
 
- Length
- Compress
- Find
- Substr
- Scan
- Subrstr
- Trim
- Upcase
- Lowcase
- Strip
- Input
- Catx


## 02: What are the SAS numeric functions that you have used for data cleaning?
Frequently used numeric functions are:

- INT
- ABS
- SQRT
- MIN
- MAX
- SUM
- MEAN
- ROUND
- LAG
- LOG
- CILL
- FLOOR


## 03: What are the SAS Date functions?
Some SAS DDATE functions are:


-  **DATE ()** &rarr; converts todays date into days from JAN01 1960. Oct24,2021-> 22577 With format date9.-> OCT24, 2021
-  **DATETIME ()** &rarr; Converts today’s this instant into second calculating from JAN01 1960.
- **DAY (Date)**&rarr; Converts day of the month.
- **TODAY ()** &rarr; converts todays date into days from JAN01 1960. Oct24,2021-> 22577With format date9.-> OCT24, 2021
- **WEEKDAY (Date)** &rarr; DAY (Date)=> Converts day of the week, SUN-1, Mon-2,…
- **QTR (Date)** &rarr; Converts into quarter of the year. 1 to 4.
- **MONTH (Date)** &rarr; Convert into nth month of the year. 1 to 12.
##### h. INTCK (Returns number, Time interval between two-time frame)
- **intck('year','28JAN1970'd,'24OCT2021'd);**&rarr; 51
- **intnx('month','24OCT2021'd,4);** &rarr; 01FEB2022



## 04: How to convert character variable to numeric variable?
In data step  INPUT function is used to change character to numeric variable with different name.
Expression used to convert character to numeric variable is as below:

Numvar=input(charvar,Informat.);

```
Sample dataset cration:
DATA CHAR_NUM_1;
INPUT X $10.;
DATALINES;
2323456789
5433333
66
;
RUN;
New Dataset and new var with numeric data type.

DATA CHAR_NUM_2;
SET CHAR_NUM_1;
NEW_VAR=(INPUT(X,10.));
RUN;

Validating New_Var data type:

PROC CONTENTS DATA = CHAR_NUM_2;
RUN;

```


## 05: How to convert numeric variable to character variable?
In data step  PUT function is used to change numeric to character variable with different name.
Expression used to convert character to character variable is as below:

Charvar=put(Numvar,Informat.);

```
Sample dataset cration:
DATA NUM_CHAR_1;
INPUT Y $10.;
DATALINES;
2323456789
5433333
66
;
RUN;
New Dataset and new var with character data type.

DATA NUM_CHAR_2;
SET NUM_CHAR_1;
NEW_VAR=PUT(Y,10.);
RUN;

Validating New_Var data type:

PROC CONTENTS DATA = NUM_CHAR_2;
RUN;

```
## 06: What are the default length of character and numeric variable?

Default length for numerical variable is 8. Default legth of character function is 8. 
## 07: Accidiently unwanted special character are attached to customer name. How do you remove those unwanted special characters?
Unwanted special characacters can be remove using sas character function compress with modifier.

```
Sample dataset:
DATA JUNK_DATA;
INPUT ID NAME $20. SALARY;
DATALINES;
101 KRI%%%SH             66669
102 $$$MA@GAN#           89000
103 JAFFERY&!            90000
105 A#NNAYA***           75900
;
RUN;

Clean data after using SAS character function:
DATA CLEAN_DATA;
SET JUNK_DATA;
NAME=COMPRESS(NAME,'$,@,%,#,!,*,&');
RUN;

```

## 08: Need to change area code of several customer's phone number, how do you change area code?
There are several ways to change area code, one of the simplest way is using subrstr function.

```
DATA OLD_AREA_CODE;
INPUT ID NAME$ PHONE_NUM $12.;
DATALINES;
101 KRISH 778-324-6767
102 MAGHI 898-332-8908
103 KAFLY 778-312-1234
104 GAINT 778-231-6543
;
RUN;

Area code has changed by 757:
DATA UPDATED_AREA_CODE;
SET OLD_AREA_CODE;
SUBSTR(PHONE_NUM,1,3)='757';
RUN;

```


## 09: Explain a scenarion where you have used nested function in sas.
If we use function inside function then it is called nested function. One example is clculating length of numeric variable. 

```
DATA NUMERIC_LENGTH_INCORRECT;
VAR = 564390;
NUM_CNT= LENGTH(VAR);
RUN;

DATA NUMERIC_LENGTH_CORRECT;
SET NUMERIC_LENGTH_INCORRECT;
NUM_CNT = LENGTH(STRIP(PUT(VAR,12.)));
RUN;

```


## 10: Last 5 character of product id represents the certain batch id. You are asking o generate distribution of batch id. How do you solve the problem?

We can us following two nested function method can be used to get last 5 digit from product ID and data distribution can be obtained using proc freq.

```
DATA PRODUCT_ID;
INPUT PRODUCTID $15.;
DATALINES;
ABBSA1234Z1X253
ABBQA1234Q1X253
AEBSA1234Z2X252
ABGSA1234Z1Y253
ABBSA1234Q1X253
ABBSW1234Z1X253
AQBSA1234Z2X252
;
RUN;
DATA CATEGORY;
SET PRODUCT_ID;
CAT_ID = SUBSTR(PRODUCTID,LENGTH(PRODUCTID)-4,5);
CAT_ID1 = REVERSE(SUBSTR(REVERSE(PRODUCTID),1,5));
RUN;

PROC FREQ DATA =CATEGORY;
TABLES CAT_ID/LIST ;
RUN;

```



[<img align="center" src="../static/images/left.svg" height="20" width="20"/> Part 2](./Interview_QA_Post2_05_24_2023.md)&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;[Part 4 <img align="center" src="../static/images/right.svg" height="20" width="20"/>](./Interview_QA_Post4_05_26_2023.md)




