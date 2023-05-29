# SAS Interview Questions and Answers Part 8

This is a collection of SAS interview question answers. Most of the questions are commonly asked in the SAS Developer/Programmer position interview. Every post has covered 10 related questions and answers.

SAS Macro (Part-2):

## 01: What is the use of %let method to create sas macro variabl? Where do we use %let macro variable?



One of the SAS Macro variable creation method is %LET statement which is used to create and assign values to macro variables. They are particularly useful for creating dynamic and reusable code. A simple example: If you have given thousand lines of code and need to change a value two thousand times then effective way is to create macro variable using %let and assign new value. This will solve the problem.


## 02: What is the importance of 

## 03: 

## 04: What is the difference between %str and %nrstr function in sas macro?

1. The %STR and %NRSTR functions mask a character string **during compilation**of a macro or macro language statement. 
2. They mask the following special characters and mnemonic operators: + − * / < > = ¬ ^ ~ ; ,  # blank
AND OR NOT EQ NE LE LT GE GT IN


## 05: What is the difference between %quote and %nrquote sas macro function.
1. The %QUOTE and %NRQUOTE functions mask a character string or resolved value of a text expression **during execution** of a macro or macro language statement. 
2. They mask the following special characters and mnemonic operators: + 
	 − * / < > = ¬ ^ ~ ; , #  blank AND OR NOT EQ NE LE LT GE GT IN

## 06: Where do you use %superq sas macro function? 
%SUPERQ is particularly useful for masking macro variables that might contain an ampersand or a percent sign when they are used with the %INPUT, or the SYMPUT routine.

Sample program with warning information.

```
DATA null;
	call symput("program1","SAS&SQL");
run;
%put &program1;

```
Output log with warning:

```
WARNING: Apparent symbolic reference SQL not resolved.
 72      %put &program1.;
 SAS&SQL

```
Sample program removing warning information.

```
DATA null;
	call symput("program1","SAS&SQL");
run;
%let program2 = %superq(program1);
%put &program2.;

```
Output log without warning:

```
72     %let program2 = %superq(program1);
73     %put &program2.;
SAS&SQL

```










[<img align="center" src="../static/images/arrow_left.svg" height="20" width="20"/> Part 2](./Interview_QA_Post2_05_24_2023.md)&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;[Part 4 <img align="center" src="../static/images/arrow_right.svg" height="20" width="20"/>](./Interview_QA_Post4_05_26_2023.md)
