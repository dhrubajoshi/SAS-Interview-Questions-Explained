# SQL Interview Questions and Answers Part 4

This is a collection of SQL interview question answers. Most of the questions are commonly asked in the SQL Developer/Programmer/Analyst position interview. Every post has covered 10 related questions and answers.

Window function continue:

## 01: What do you mean by Dense_Rank() function?

Dense_RANK(): It is a analytical or window function which provides the rank of the observations based on value. For two same value it provides same rank and the subsequent rank won't skip resulting in consecutive ranks.

Sample query:

```
select Employee_id,
First_Name as Employee_Name,
Salary,
Dense_rank() over (order by salary desc) as DRank_Num
from employees;

```
Output result: Only 10 records are copied here.

```
Employee_id Employee_Name Salary DRow_Num
100			Steven			24000	1
101			Neena			17000	2
102			Lex				17000	2
145			John			14000	3
146			Karen			13500	4
201			Michael			13000	5
108			Nancy			12000	6
147			Alberto			12000	6
205			Shelley			12000	6
168			Lisa			11500	7

```

## 02: What are the use of row_number, rank, and dense_rank function?

## 03: Where do we use union and union all operators?
When two or more tables having same columns need to join vertically (one after another) then union operator results records 

## 04: How do you compare two tables?
We can use the EXCEPT operator to compare two tables. It returns rows from the first table that are not part of the second table.

```
proc sql;
select * from Old_Table
except
select * from New_Table
quit;

```

## 