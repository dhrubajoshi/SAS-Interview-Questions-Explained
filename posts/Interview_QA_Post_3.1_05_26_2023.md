# SAS Interview Questions and Answers Part 3.1

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
Name1_Scan=scan(Name1,2,’&’);
Name1_Substr=substr(Name1,1,4);
Run;

```


[<img align="center" src="../static/images/arrow_left.svg" height="20" width="20"/> Part 2](./Interview_QA_Post2_05_24_2023.md)&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;[Part 4 <img align="center" src="../static/images/arrow_right.svg" height="20" width="20"/>](./Interview_QA_Post4_05_26_2023.md)
