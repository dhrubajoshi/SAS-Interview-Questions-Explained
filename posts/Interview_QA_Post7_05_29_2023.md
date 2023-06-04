[# SAS Interview Questions and Answers Part 7
> 
> This is a collection of SAS interview question answers. Most of the questions are commonly asked in the SAS Developer/Programmer position interview. Every post has covered 10 related questions and answers.
> 
> SAS Macro (Part-2):
> 
> ## 01: What is the use of %let method to create sas macro variabl? Where do we use %let macro variable?
> 
> One of the SAS Macro variable creation method is %LET statement which is used to create and assign values to macro variables. They are particularly useful for creating dynamic and reusable code. A simple example: If you have given thousand lines of code and need to change a value two thousand times then effective way is to create macro variable using %let and assign new value. This will solve the problem.
> 
> ```
> %let Macro_Var= 101;
> 
> ```
> 
> 
> ## 02: Where do you use % do Iterative method.
> In SAS, the %DO iteration is a type of creation of macro variable which allows us to perform iterative operations in a macro. It is used when we need to repeat a block of code a certain number of times or iterate over a list of values.
> 
> Sample code:
> 
> ```
> options mindelimiter=,;
> options minoperator;
> %MACRO Odd_Even();
> %DO i = 1 %to 7 ;
> %if &i in (1,3,5,7) %then %do;
> %PUT i = &i - odd;
> %END;
> %ELSE %DO;
> %PUT i = &i - even;
> %end;
> %end;%MEND;
> %Odd_Even();
> 
> ```
> Output result:
> 
> ```
> i = 1 - odd
> i = 2 - even
> i = 3 - odd
> i = 4 - even
> i = 5 - odd
> i = 6 - even
> i = 7 - odd
>  
> ```
> 
> ## 03: Why do you use Proc SQl into clause to create macro variable?
> 
> PROC SQL INTO clause is used to create macro variables by assigning values from the result of a SQL query. The INTO clause allows us to store query results into macro variables, enabling us to use those values later in our SAS program.
> 
> We have following sample dataset and we need to change status_code as we require.
> 
> ```
> Data status_Table;
> input JOB_ID Business_Date mmddyy10. Status_Code;
> format Business_Date date9.;
> datalines;
> 305 05/30/2023 .
> 304 05/29/2023 12
> 303 05/28/2023 0
> 302 05/27/2023 0
> 301 05/26/2023 0
> 300 05/25/2023 0
> 301 05/24/2023 0
> ;
> run;
> ```
> Let us use proc sql into clause:
> 
> ```
> Proc sql;
> select JOB_ID into: Var1
> from Status_Table
> where status_code not in (.,0);
> quit;
> %put &Var1.;
> 
> proc sql;
> update Status_Table
> set status_code=0
> where Job_ID =&Var1.;
> quit;
> 
> ```
> Checking updated status table:
> 
> ```
> proc sql;
> select * from status_Table;
> quit;
> 
> ```
> Output result:
> 
> ```
> 
> JOB_ID	Business_Date	Status_Code
> 305	30MAY2023	.
> 304	29MAY2023	0
> 303	28MAY2023	0
> 302	27MAY2023	0
> 301	26MAY2023	0
> 300	25MAY2023	0
> 301	24MAY2023	0
> 
> ```
> 
> ## 04: What is the difference between %str and %nrstr function in sas macro?
> 
> 1. The %STR and %NRSTR functions mask a character string **during compilation**of a macro or macro language statement. 
> 2. They mask the following special characters and mnemonic operators: + − * / < > = ¬ ^ ~ ; ,  # blank
> AND OR NOT EQ NE LE LT GE GT IN
> 
> 
> ## 05: What is the difference between %quote and %nrquote sas macro function.
> 1. The %QUOTE and %NRQUOTE functions mask a character string or resolved value of a text expression **during execution** of a macro or macro language statement. 
> 2. They mask the following special characters and mnemonic operators: + 
> 	 − * / < > = ¬ ^ ~ ; , #  blank AND OR NOT EQ NE LE LT GE GT IN
> 
> ## 06: Where do you use %superq sas macro function? 
> %SUPERQ is particularly useful for masking macro variables that might contain an ampersand or a percent sign when they are used with the %INPUT, or the SYMPUT routine.
> 
> Sample program with warning information.
> 
> ```
> DATA null;
> 	call symput("program1","SAS&SQL");
> run;
> %put &program1;
> 
> ```
> Output log with warning:
> 
> ```
> WARNING: Apparent symbolic reference SQL not resolved.
>  72      %put &program1.;
>  SAS&SQL
> 
> ```
> Sample program removing warning information.
> 
> ```
> DATA null;
> 	call symput("program1","SAS&SQL");
> run;
> %let program2 = %superq(program1);
> %put &program2.;
> 
> ```
> Output log without warning:
> 
> ```
> 72     %let program2 = %superq(program1);
> 73     %put &program2.;
> SAS&SQL
> 
> ```
> ## 07: 
> 
> 
> 
> 
> 
> 
> 
> 
> 
> [<img align="center" src="../static/images/arrow_left.svg" height="20" width="20"/> Part 2](./Interview_QA_Post2_05_24_2023.md)&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;[Part 4 <img align="center" src="..](``)/static/images/arrow_right.svg" height="20" width="20"/>](./Interview_QA_Post4_05_26_2023.md)
