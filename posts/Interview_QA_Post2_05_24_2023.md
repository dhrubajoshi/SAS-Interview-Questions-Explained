# SAS Interview Questions and Answers Part 2

This is a collection of SAS interview question answers. Most of the questions are commonly asked in the SAS Developer/Programmer position interview. Every post has covered 10 related questions and answers.


## 01: What are the difference between data step and proc step in sas?
1. Data step starts with DATA statement and procedure starts with PROC statement.
2. Data step read input data into a SAS dataset, edit, merge, update,rearrage and create data set. Proc step use to perform specific analysis on data and produce results or report.

## 02: How to copy specific number of records from SAS data set?
Following code helps to copy specific number of records:

First 10 records:

```
DATA FIRST_TEN_RECORD;
SET SASHELP.CLASS(OBS=10);
RUN;
```

Any number of records(Any starting and ending record number):

```
DATA ANY_NUM_RECORD;
SET SASHELP.CLASS(FIRSTOBS=5 OBS=14);
RUN;
```
## 03: How to print specific number of records from SAS data set?
Following code helps to print specific number of records:

First 10 records:

```
PROC PRINT DATA=SASHELP.CLASS(OBS=10);
RUN;
```

Any number of records(Any starting and ending record number):

```
PROC PRINT DATA=SASHELP.CLASS(FIRSTOBS=5 OBS=14);
RUN;
```
## 04: What are the default statistics that PROC MEANS produce?

Default statistics that proc means are N, MIN, MAX, MEAN and STD DEV.


## 05: What are commonly used procedure that helps to get descriptive statistics?
Commonly use procedures are:

proc contents: It provides information -number or record, number of variables, types of variables(Numeric, Character, Date with format and informat).

```
PROC CONTENTS DATA =SASHELP.CLASS;
RUN;

```
proc freq: It provides record count of missing and non missing character variable.

```
PROC FREQ DATA = SASHELP.HEART;
TABLES _CHARACTER_/LIST MISSING NOCOL;
RUN;

```
proc means: It provides record count, max, min, mean, std dev, and missing numeric variable.

```
PROC MEANS DATA = SASHELP.HEART N NMISS MEAN MAX MIN STD;
VAR _NUMERIC_;
RUN;

```
## 06: How to calculate N, Nmiss, Min, Max, Mean, STD DEV by group?

To get groupwise information we need to sort data by group variable as below:

```
PROC SORT DATA = SASHELP.CLASS OUT =TEST1;
BY SEX;
PROC MEANS DATA=TEST1;
BY SEX;
VAR AGE HEIGHT WEIGHT;
RUN;

```
## 07: What is the difference between proc means and proc summary?
By default Proc MEANS produces printed output in the OUTPUT window.

```
PROC MEANS DATA=SASHELP.CLASS;
VAR AGE;
RUN;
OUTPUT:

The MEANS Procedure

Analysis Variable : Age
N	Mean	Std Dev	Minimum	Maximum
19	13.3157895	1.4926722	11.0000000	16.0000000
```



By default proc summary doesn't produce printed output. We need to provide either output or print option.

```
PROC SUMMARY DATA =SASHELP.CLASS PRINT;
VAR AGE;
RUN;

OUtput:
The SUMMARY Procedure

Analysis Variable : Age
N	Mean	Std Dev	Minimum	Maximum
19	13.3157895	1.4926722	11.0000000	16.0000000

```
Omitting the var statement in PROC MEANS analyses all the numeric variable.

```
proc means data=sashelp.class;
run;
Output:

| The MEANS Procedure |     |             |            |         |         |
| :------------------ | :-- | :---------- | :--------- | :------ | :------ |
| Variable            | N   | Mean        | Std Dev    | Minimum | Maximum |
| Age                 | 19  | 13.3157895  | 1.4926722  | 11      | 16      |
| Height              | 19  | 62.3368421  | 5.1270752  | 51.3    | 72      |
| Weight              | 19  | 100.0263158 | 22.7739335 | 50.5    | 150     |

```
Omitting the variable statement in PROC SUMMARY produces a simple count of observation.

```
PROC SUMMARY DATA =SASHELP.CLASS PRINT;
RUN;

Output:
The SUMMARY Procedure

N Obs
19

```

## 08: If the role of proc means and summary with print option has the same where do we use means and where is proc summary?

The difference between the two procedures is that PROC MEANS produces a report by default, whereas PROC SUMMARY produces an output data set by default. So if you want a report printed to the listing - use proc means - if you want the info passed to a data set for further use - proc summary may be a better choice.

## 09: What are the differences between proc means and proc univariate?
1. Proc means cannot calculate custom percentile where proc univariate can.
2. Proc means calculate one max and min value but proc univariate gives five lowest and five highest records.
3. Proc means does not support normality test where proc univariate can. 
4. Proc means does not support graphics but proc univariate can.

## 10: What are the differences between MEAN function and PROC MEANS?
1. The Mean function average across row and mean procesure will work across column.
2. The mean function results the average value of several variables in one observation but proc means results five default statics for each numeric variable.






 








