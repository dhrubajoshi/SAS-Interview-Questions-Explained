# SAS Interview Questions and Answers Part 1

This is a collection of SAS interview question Answer. Most of questions are commonly asked in the SAS Developer/Programmer position interview. Every post has covered 10 related questions and answewrs.


## 01: How to copy data in sas?

A set statement is used to copy sas dataset. Refer to the following code to copy the SAS data

```
DATA NEW_DATA;
SET SASHELP.CLASS;
RUN;
```

## 02: What is the difference between single and multiple set statement?

Sigle and multiole set statement in SAS data step results differently.

```
DATA TEST1;
INPUT NAME $ SEX $ AGE HEIGHT WEIGHT;
DATALINES;
Jess F 30 65 122
Bob M 33 67 157
Philip M 55 70 170
Krish M 32 62 166

;
RUN;
DATA TEST2;
INPUT NAME $ SEX $ AGE HEIGHT WEIGHT;
DATALINES;
GAYLE F 38 60 125
MIKE M 48 67 153
;
RUN;

DATA TEST3;
INPUT NAME $ SEX $ AGE HEIGHT WEIGHT;
DATALINES;
Jaferry M 14 67 153
;
RUN;

---------------------
DATA COMBINE1;
SET TEST1 TEST2 TEST3;
RUN;

Result: Data will append TEST1 TEST2 TEST3 accordingly.
---------------------
DATA COMBINE2;
SET TEST1 TEST2 TEST3;
BY Sex;
RUN;
Result: Data will append TEST1 TEST2 TEST3 accordingly with sorting by sex variable.
---------------------
DATA COMBINE3;
SET TEST3;
SET TEST2;
SET TEST1;
RUN;

 Result: It displays records from the first data set where the number of the record is controlled by the minimum records of the provided data sets.
[Displays only first one record from test1 - Jess F 30 65 122]

 
```

## 03: How do you group values in comma seperated format?
Following sample code helps to convert values by group with comma seperated format.


```
DATA DATA_ONE;
INPUT ID VAL;
DATALINES;
12 22
12 56
12 88
13 89
13 90
13 98
14 56
14 89
14 43
;
RUN;
DATA DATA_FINAL(KEEP=ID LIST);
  DO UNTIL (LAST.ID);
    SET DATA_ONE;
    BY ID;
    LENGTH LIST $30;    
    LIST = CATX(', ',LIST,VAL);
  END;
RUN;
```

## 04: What do you mean by informat and format statement in SAS?
1. Format can be used in both data step and proc step but informat is used only in data step.
2. An informat is a specification for how the raw data should read where format is a layout specification for how a variable be printed or displayed.

## 05: How can you import different data format into sas dataset?

Following sample code can be used to import data into SAS Data set. We can choose DBMS='Data format  as given'.

```
PROC IMPORT 
DATAFILE="/DIRECTORY_LOCATION/FILE_NAME.XLS"
OUT =SASDATA_SET
DBMS=XLS
REPLACE;
GETNAME =YES;
RUN

```

## 06: What are the differences between Proc Sql pass through and SAS Libname method to connect the database? Which one is the more efficient?

1. Both SQL Passthrough and Libname method are used to connect the database. 
2. Passthrough method is more efficient than libname method because in pass through we can work directly into the data base and able to select the variables as needed and filter the observations. 
3. SQL Passthrough is sql so some of SAS functions such as INPUT, PUT, INTCK, INTNX etc. do not work. 
4. But in libname method we extract all the data into sas and can be use all the sas function.
5. While deleting and updating the information in the database, Proc SQL passthrough method is significiently efficient.

## 07: What are the most efficient ways to write the sas code?

Following techniques can be include while writing the SAS code.

1.	Using keep and drop option with source data set to select variables.
2.	Using if and where statement for subgrouping the rocords of the data set.(Where is more efficient than if).
3.	Use of the efficient way of appending the data set – using proc append instead of the data step set statement in sas.
4.	Subset early,
5.	Use index and use compress= yes option.
6.	Use appropriate variable length (Identify max length and use it).
7.	Delete work data set.

## 08: What is the difference between view and SAS data set?
1. SAS data set is created by reading data from an external file or by creating data using data step. It is stored in physical file on disk with a specific structure that includes variables and observations.
2. A view is a virtual data set that is created dynamically based on the specifications provided in the view definition. It does not contain any physical data storage. It is a logical representation of the data that is generated on the fly when the view is accessed.

## 09: How to include and exclude specific variables in a data set?
Keep/Drop option in set statement is more efficient than data statement.
To remove unnecessary variables, we can use keep/drop option as well as keep/drop statement.


## 10: What is the difference between SAS data step set and proc append? Which is more efficient?

1. Proc append procedure adds the observations from one SAS data set to the end of another SAS data set.
2. In SAS set statement, it writes each and every record one after other.
3. Instead of the rewriting both data sets proc append just writes the data set identified by data= after the observations in the data set identified by BASE=. So it takes less time as compare to data set statement.

[<img align="center" src="../static/images/arrow_left.svg" height="20" width="20"/> Home](../README.md)&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;[Part 2 <img align="center" src="../static/images/arrow_right.svg" height="20" width="20"/>](./Interview_QA_Post2_05_24_2023.md)



