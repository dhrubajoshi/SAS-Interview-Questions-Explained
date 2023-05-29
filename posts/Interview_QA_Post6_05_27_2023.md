# SAS Interview Questions and Answers Part 6

This is a collection of SAS interview question answers. Most of the questions are commonly asked in the SAS Developer/Programmer position interview. Every post has covered 10 related questions and answers.

SAS MACRO:
## 01: What do you mean by SAS MACROS?
SAS Macros are powerful feature of the SAS Programming which allow to avoid repetation of code and allow to use them again and again as needed. If we need to claculative descriptive statistics of different fields of 100 different table then one macro programming is sufficient and we pass parameter into it as required.


## 02: What are the types of the SAS Macro variables?

Sas Macro variables are two type:

**User defined Macro Variable**: Those macro variables which are created by user

and 

**Automated Macro Variables**: Macro Variables which are system defined, these are called automated macro variables: Example- &SYSDATE9. , &SYSTIME 

Further they are classified into two macro variable type:

1.	**Local Macro Variable**: Those SAS macro variables which are defined inside the macro and can me use inside the macro only and will be ended when macro is ended.

2.	**Global Macro-Variable**: Those macro variables which are defined inside the macro as well as outside the macro and can be used anywhere inside the program and will be ended when the program is closed. Example: Automated Global macro variables are &SYSDATE9. , &SYSTIME.


## 03: What are the different ways to create SAS Macro variables?
There five different ways to create the SAS MACRO Variables:
1. %let 
2.	Macro Parameters
3.	INTO clause in Proc SQL
4.	Call Symput
5.	Iterative %Do


## 04: Where did you use SAS Macro program in your previous project and why?
One of the example I used SAS Macro in my previous job counting daily record of SOR by Entity. There were 20 Entities(Table) and one field is SOR(Source of record) which has 18 category. I used SAS Macro program( macro parameters- macro variable method). Thia single macro program is sufficient for all entity, just need to pass entity name in parameter section. It has highly reduced reperative coding task.

```
%Macro Daily_count(DSN, Entity);
Proc sql;
create table &Entity. as
select "&Entity." as Entity, SOR, Custom_Date as BUS_Date, weekday(Datepart(Custom_Date)) as week_Day, count(*) as count_N
from libname.&DSN.
where datepart(Custom_Date)="23MAY2019"d
group by Entity, SOR
quit;
%Mend;
%Daily_count(Party_custom, Party_custom);
%Daily_count(Party, Party);
.
.
.
%Daily_count(Account, Account);

```
## 05: You have given a credit card customer information. Manager is asking the list of customer name with comma seperation. How do you solve the issue?
We can use sas macro - INTO clause in Proc SQL. 
A sample code is as below:

```
PROC SQL;
SELECT NAME INTO: MICRO_VAR SEPARATED BY ','
FROM SASHELP.CLASS;
QUIT;
%PUT &MICRO_VAR;

DATA TEST;
NAME="&MICRO_VAR.";
RUN;

```

Output Result:

```
Alfred,Alice,Barbara,Carol,Henry,James,Jane,Janet,Jeffrey,John,Joyce,Judy

```
## 06: How do you debug SAS Macro code?
Following Macro Options can be used to debug SAS Macro.
1.	MPRINT
2.	MLOGIC
3.	SYMBOLGEN

## 07: What are the differences between MPRINT, MLOGIC and SYMBOLGEN?

All three options are used for SAS MACRO debugging. 

1. **MPRINT** translates the macro language to regular SAS language. It displays all the SAS statements of the resolved macro code:

```
OPTIONS MPRINT;
%MACRO TEST (INPUT,OUTPUT);
PROC MEANS DATA = &INPUT NOPRINT;
VAR HEIGHT;
OUTPUT OUT = &OUTPUT MEAN= ;
RUN;
%MEND;
%TEST(SASHELP.HEART,TEST);
```
It returns the following message in LOG window :

```
MPRINT(TEST):   proc means data = sashelp.heart noprint;
MPRINT(TEST):   var height;
MPRINT(TEST):   output out = test mean= ;
MPRINT(TEST):   run;
```
2. **MLOGIC** is very helpful while dealing with nested macro. When we use %DO loop or %if-%then -%ELSE statements inside the macro code then logic option will display how the macro variable resolved each time in the LOG file as TRUE or FALSE.

```
OPTIONS MLOGIC;
OPTIONS MINDELIMITER=,;
OPTIONS MINOPERATOR;
%MACRO TEST();
%DO I = 1 %TO 9 ;
%IF &I IN (1,3,5,7,9) %THEN %DO;
%PUT I = &I - ODD;
%END;
%ELSE %DO;
%PUT I = &I - EVEN;
%END;
%END;
%MEND;
%TEST();
```
It returns the following message in LOG window :

```
MLOGIC(TEST):  Beginning execution.
 MLOGIC(TEST):  %DO loop beginning; index variable I; start value is 1; stop value is 9; by value is 1.  
 MLOGIC(TEST):  %IF condition &i in (1,3,5,7,9) is TRUE
 MLOGIC(TEST):  %PUT i = &i - odd
 i = 1 – odd
MLOGIC(TEST):  %DO loop index variable I is now 2; loop will iterate again.
 MLOGIC(TEST):  %IF condition &i in (1,3,5,7,9) is FALSE
 MLOGIC(TEST):  %PUT i = &i - even
 i = 2 - even
```
3. **Symbolgen** writes the result of resolving macro variable reference to the SAS log for debugging.

```

OPTIONS SYMBOLGEN;
%MACRO TEST (INPUT =,OUTPUT=);
PROC MEANS DATA = &INPUT NOPRINT;
VAR HEIGHT;
OUTPUT OUT = &OUTPUT MEAN= ;
RUN;
%MEND;
%TEST(INPUT=SASHELP.HEART,OUTPUT=TEST);
```
It returns the following message in LOG window :

```
SYMBOLGEN:  Macro variable INPUT resolves to sashelp.heart
SYMBOLGEN:  Macro variable OUTPUT resolves to test

```

## 08: What is the importance of use of multiple ampersand in sas macro?
Multiple ampersands allow more flexible and dynamic coding in SAS macros. It enables us to generate and resolve macro variable names based on different criteria (loop counters, data values, or other conditions) providing a powerful way to automate repetitive tasks and perform conditional operations in macros.

```


```
## 09: What do you mean by %SYSFUNC Function and what is it's use?
There are number of Base SAS function which are not directly available in SAS MACRO, %Sysfunc is used to enable those function to make them work in a macro.

```
DATA _NULL_;
%LET TODAY_DATE=%SYSFUNC(TODAY(), DATE9.);
RUN;
%PUT &TODAY_DATE;
```

OUTPUT:

```
26MAY2023

```

## 10: What is difference between  %eval and  %sysevalf in sas MACOR?
Both %eval and %sysevalf are SAS Macro function. %eval function is used to perform mathematical operations with macro variables for integer values.

```
DATA _NULL_;
%LET A=6;
%LET B=15;
%LET C=%EVAL(&B-&A);
%LET D=%EVAL(&A*&B);
RUN;
%PUT &C;
%PUT &D;
```
Output:

```
C=9
D=90
```
%sysevalf function is used to perform mathematical operations with macro variables for float values.

```
DATA _NULL_;
DATA _NULL_;
%LET M=6.5;
%LET N=19.4;
%LET X=%SYSEVALF(&N-&M);
%LET Y=%SYSEVALF(&M*&N);
RUN;
%PUT &X;
%PUT &Y;

```
Output:

```
X=12.9
Y=126.1

```



[<img align="center" src="../static/images/arrow_left.svg" height="20" width="20"/> Part 5](./Interview_QA_Post5_05_27_2023.md)&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;[Part 7 <img align="center" src="../static/images/arrow_right.svg" height="20" width="20"/>](./Interview_QA_Post7_05_28_2023.md)