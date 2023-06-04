# SAS Interview Questions and Answers Part 7

This is a collection of SAS interview question answers. Most of the questions are commonly asked in the SAS Developer/Programmer position interview. This post has covered 9 related questions and answers.

SAS Macro (Part-2):

## 01: What is the use of %let method to create sas macro variable? Where do we use %let macro variable?

One of the SAS Macro variable creation method is %LET statement which is used to create and assign values to macro variables. They are particularly useful for creating dynamic and reusable code. A simple example: If you have given thousand lines of code and need to change a value two thousand times then effective way is to create macro variable using %let and assign new value. This will solve the problem.

Sample sas code to create macro variable using %let.

```
%let Dept_ID = 101;

```
Here Dept_ID is macro variable whose value is 100. 

## 02: Where do you use % Do Iterative method.
In SAS, the %DO Iteration is a type of creation of macro variable which allows us to perform iterative operations in a macro. It is used when we need to repeat a block of code a certain number of times or iterate over a list of values.

Sample code:

```
options mindelimiter=,;
options minoperator;
%MACRO Odd_Even();
%DO i = 1 %to 7 ;
%if &i in (2,4,6) %then %do;
%PUT i = &i - even
%END;
%ELSE %DO;
%PUT i = &i - odd;
%end;
%end;%MEND;
%Odd_Even();

```
Output result:

```
i = 1 - odd
i = 2 - even
i = 3 - odd
i = 4 - even
i = 5 - odd
i = 6 - even
i = 7 - odd
 
```

## 03: Why do you use Proc SQl into clause to create macro variable?

PROC SQL INTO clause is used to create macro variables by assigning values from the result of a SQL query. The INTO clause allows us to store query result into macro a variable, enabling us to use those value later in our SAS program.

We have following sample dataset and we need to change status_code as we require.

```
Data status_Table;
input JOB_ID Business_Date mmddyy10. Status_Code;
format Business_Date date9.;
datalines;
305 05/30/2023 .
304 05/29/2023 12
303 05/28/2023 0
302 05/27/2023 0
301 05/26/2023 0
300 05/25/2023 0
301 05/24/2023 0
;
run;
```
Let us use proc sql into clause: ( We need to change status_code to 0 whose value is neither 0 nor missing (.).

```
Proc sql;
select JOB_ID into: Var1
from Status_Table
where status_code not in (.,0);
quit;
%put &Var1.;

proc sql;
update Status_Table
set status_code=0
where Job_ID =&Var1.;
quit;

```
Checking updated status table:

```
proc sql;
select * from status_Table;
quit;

```
Output result:

```

JOB_ID	Business_Date	Status_Code
305	30MAY2023	.
304	29MAY2023	0
303	28MAY2023	0
302	27MAY2023	0
301	26MAY2023	0
300	25MAY2023	0
301	24MAY2023	0

```

## 04: What is the difference between %str and %nrstr function in sas macro?

The %STR and %NRSTR functions mask a character string **during compilation** of a macro or macro language statement. They mask the following special characters and mnemonic operators: + − * / < > = ¬ ^ ~ ; ,  # blank
AND OR NOT EQ NE LE LT GE GT IN

%nrstr is able to mask macro triggers & %.

## 05: Why do you use %nrstr in place of %str in SAS MACRO?
If we need to mask macro triggers & and % then we use %nrster inplace of %str.

Use of %str:

```
%let x=100;
%let eg  = %str(&x);
%let eg1  = %str(%"x);
%put &eg;
%put &eg1;

```
Output result:

```
 %put &eg;
 100
 %put &eg1;
 "x"
 
 ```
 Use of %nrstr:
 
 ```
%let eg2 = %nrstr(&x);
%let eg3 = %nrstr(%"x);
%put &eg2;
%put &eg3;      

```
Output result: Macro variable x didn'r resolve because %nrstr function mask macro trigger(&).

```
 %put &eg2;
 &x
 %put &eg3;
 "x"
```
## 06:  What is the difference between %quote and %nrquote sas macro function.

The %QUOTE and %NRQUOTE functions mask a character string or resolved value of a text expression **during execution** of a macro or macro language statement. 
They mask the following special characters and mnemonic operators: + 
	 − * / < > = ¬ ^ ~ ; , #  blank AND OR NOT EQ NE LE LT GE GT IN.
	 
%nrquote is able to mask macro triggers & %.

Note: %BQUOTE and %NRBQUOTE have same function as %QUOTE and %NRQUOTE.

## 07: Where do you use %superq sas macro function? 
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
## 08: What are the differences between call symput and call symputx?

- CALL SYMPUTX is similar to CALL SYMPUT. Here are the differences.

**1.** CALL SYMPUT writes a note to the log stating that numeric values were converted to character values. But CALL SYMPUTX does not write a note to the SAS log when the second argument is numeric.

```
DATA _NULL_;
YEAR=1978;
CALL SYMPUT('YDATE',YEAR,’G’);
RUN;
%PUT &YDATE;

```
Output result: 

```
NOTE: Numeric values have been converted to character values at the places given by: (Line):(Column).

%PUT &YDATE;
1978

```
When using CALL SYMPUTX:

```
DATA _NULL_;
YEAR=1978;
CALL SYMPUTX('YDATE',YEAR,’G’);
RUN;
%PUT &YDATE;

```
Output result: 

```
It won't display warning message, directly get following result.

 %PUT &YDATE;
 1978

```
**2.** CALL SYMPUTX uses a field width of up to 32 characters when it converts a numeric second argument to a character value. CALL SYMPUT uses a field width of up to 12 characters.
CALL SYMPUTX left-justifies both arguments and trims trailing blanks. 



CALL SYMPUT does not left-justify the arguments, and trims trailing blanks from the first argument only. Leading blanks in the value of name cause an error.
CALL SYMPUTX enables you to specify the symbol table in which to store the macro variable, whereas CALL SYMPUT does not.




## 09: What are the uses of SYMGET and SYMGETN function in SAS Macero?
The SYMGET and SYMGETN functions are used to retrieve the values of previously stored macro variables and assigns these values in new programmer-defined variables. Only difference is datas type. SYMGET is used to store character values and SYMGETN is used for numeric values.

Sample SAS Code:

```

data test1;
set sashelp.class;
call symputx(name, age);
run;

data test2;
set test1(Keep=name);
run;

data test3;
set test2;
Age=symget(name);
run;

```
Output result: Only 6 records has coppied from test3 result.

```

	Name		Age
	Alfred		14
	Alice		13
	Barbara		13
	Carol		14
	Henry		14
	James		12
	
```
Checking data type:

```
proc contents data =test3;
run;

```
Output result: Age is character data type.

```	

Alphabetic List of Variables and Attributes
	Variable	Type	Len
2	Age			Char	200
1	Name		Char	8

```
Sample Code:

```
data test4;
set test2;
Age=symgetn(name);
run;

```
Output result: Only 6 records has coppied.

```

	Name	Age
	Alfred	14
	Alice	13
	Barbara	13
	Carol	14
	Henry	14
	James	12


```
Checking data type:

```
Proc contents data = test4;
run;

```
Output result: Age is numeric data type.

```

Alphabetic List of Variables and Attributes
	Variable	Type	Len
2	Age			Num		8
1	Name		Char	8

```

## 10: What is call execute and where do you use?

 









[<img align="center" src="../static/images/arrow_left.svg" height="20" width="20"/> Part 6](./Interview_QA_Post6_05_24_2023.md)&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;[Part 4 <img align="center" src="../static/images/arrow_right.svg" height="20" width="20"/>](./Interview_QA_Post4_05_26_2023.md)
