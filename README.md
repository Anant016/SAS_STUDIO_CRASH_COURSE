## SAS_STUDIO_CRASH_COURSE

# NOTES
```.sas
*Read data separated by blanks into SAS;

/* 
Tim M 50 145
Sara . 23 130 
Mike M 65 180
Laura F . 130
Sean M 15 167
*/

data sdata_blanks;
   infile "/folders/myfolders/DATA_blanks.txt";
   input name $ Gender $ Age Weight;
run;

proc print data=sdata_blanks;
run;


/* Read .csv data*/

/* 
Tim, M, 50, 145
Sara, , 23, 130 
Mike, M, 65, 180
Laura, F, ,130
Sean, M, 15, 167
*/
 
data sdata_commas;
   infile "/folders/myfolders/DATA_commas.csv" dsd;
   input name $ Gender $ Age Weight;
run;

proc print data=sdata_commas;
run;

DSD (delimiter-sensitive data)
The DSD option changes how SAS treats delimiters when you use LIST input and sets the default delimiter to a comma. When you specify DSD, SAS treats two consecutive delimiters as a missing value and removes quotation marks from character values.


data sdata_commas;
   infile "/folders/myfolders/DATA_commas.csv" delimiter/dlm=':';
   input name $ Gender $ Age Weight;
run;

Formats
	1.W.d - 3.   
	- 3.1- 3 columns and 1 decimal pt ex: 3.1

	2. $w. (max char)

	3. MMDDYY10. 10/21/1978 - 8 digit 2 /
Sas date is value b/w no. of days from x to 1st Jan 1960![Uploading image.png…]()


```





### 1. Create/Process Dataset

```.sas
DATA mydata;
INPUT name $ 10. mobile gender$ 1-2;
DATALINES;
Anant 9312585135 M
Priya 9999 F
; 
RUN;

DATA myclass;
   set sashelp.class;
   PROFIT=SELL-CP; * Add Extra column;
   X=upcase(x);
   Name=propcase(name);
   AB=cats(a,b);
   zz=substr(abc,2,1);
   Age=yrdif(Date, today(), "age");
   Anniversary=mdy(month(Date), day(Date), year(today()) );
   where..;
run;
```

### 2. Print

```.sas
PROC PRINT DATA=mydata;
title 'ABC';
RUN;
```

### 3. Some Proc statements

```.sas
PROC CONTENTS DATA=mydata; *metadata;
PROC MEANS DATA=mydata; *mean,max,min,SD;
PROC UNIVARIATE DATA=mydata; *all details, use BY/ID;
PROC FREQ DATA=mydata;

proc sort data=company;
   by <DESCENDING> Name;
   NODUPRECS by _ALL_; * remove duplicates;
   NODUPKEY by Name;
   where expression1 and/or expression2; * Filtering;
   where col_name in(val1,val2..);
   where Age is missing;
   is null;
   is not missing;
   between
   Like 
run;

/* Grouping */
Proc print data=<>;
By country;
Sum salary;
Run;


```
## Formatting

```.sas
proc print data="";
   format x y 3. DOB date9.
run;

x--y 3.1
_numeric_ 3.1
* 3. - nearest whole number, date9. -date format;
* 8.1, COMMA8.1, DOLLAR10.1, YEN;
* DATE7., DATE9., MMDDYY10., DDMMYY8.,MONYY7., MONNAME.,WEEKDATE. ;
```

### 4. Import

```.sas
data imp_data;
infile '/folders/myfolders/b.txt' dlm=',';
input A$ B;
run;

PROC IMPORT datafile ='/folders/myfolders/a.xlsx' OUT=imp_data <REPLACE> dbms=xlsx/csv; sheet='sheet1';
run;
```

### Libname
```.sas
options validvarname=v7; *set colum name <space to _>;
liibname <libref> <engine> <path>;
libname mylib base/xlsx "path";

libname <libref> clear;

proc content data=mylib.table_name
```

### 5. Loops

```.sas
* do;
data A;
do i = 1 to 5 by 0.5;
   y = i**2; /* values are 1, 2.25, 4, ..., 16, 20.25, 25 */
   output;
end;
run;

*do while;
data A;
n=0;
do while(n<5);
   put n=;
   n+1;
   output;
end;

*do until;
data A;
n=0;
do until(n>=5);
   put n=;
   n+1;
   output;
end;
```

### 6. Conditions

```.sas
IF expression THEN statement;
*<ELSE statement;>

If expression THEN DO;
<expressions>;
END;
```

### 7. Set/ Concatenate 
- same attributes

```.sas
data x; 
   length x $ 9;
   set data1 data2(rename =(x=y) drop z );
run;
```

### 8. Merge
- like join 
- can also use proc sql;

```.sas
1. one to one 
data employee_info;
   merge company(rename=(temp=temp_2018)) finance(rename=(temp=temp_2017));
   by name;
run;

2. one to many 

```

### 9. Export

```.sas
proc export data=mydata
  dbms=xlsx
  outfile='/folder/mydolder/a.xlsx' replace;
  sheet="Sheet1";
run;

* csv;
ods csvall file="filename.csv";
proc print data=...;
run;
ods csvall close;

* excel;
ods excel file="filename.csv" <style> <options> ;
proc print data=...;
run;
ods excel close;

* all styles;
proc template;
list styles;
run;



```


### 10. Macros

```.sas
%let day=&sysday.;
%let date=&sysdate;
%let name 'Anant Rungta';
%let x abc;

%put &day;
%put &date;
%put &name;
%put &x;

%let year=2005;
%let month=Jan;
Proc print data=&year&month.;
run;

%put %sysfunc(year(%sysfunc(today()));


```

### 11. SQL

```.sas
proc sql;
connect to oracle (server='myserver' database='myDB' user=myusr1 
                       password=mypwd1 validvarname=v6)
create table work.foo as
 select * from connection to netezza
     (select "Amount Budgeted$", "Amount Spent$"
      from annual_budget);
quit;

proc contents data=work.foo;run;

```

### 12. Linear Regression (*)

```.sas
DATA x;
input xx yy;
datalines;
1 2
2 3
3 5
4 8
0 9
;
run;

PROC SGSCATTER DATA=x;
PLOT xx*yy;

PROC reg data=x;
model yy=xx/clb;

DATA x2;
input xx yy;
datalines;
8.5 .
;

DATA x3;
set x x2;

PROC REG data=x3;
model yy=xx/cli; 

PROC PRINT data=x3;

```

### title & Footnote

```.sas
title<n> "title"
footnote<n> ""
```

### ods - output delivery system

```.sas
ods noproctitle; * switch off title;

* graphs/tables;
ods graphics on;

proc freq data=.. order=freq nlevels;
tables x y/ nocum plots=freqplot(orient=horizontal scale=percent);
formats ...;
run;
```

### pivot up/down - wide/narrow

1. pivot down
```.sas
name maths science -> name subject score

data new
set ...
keep Name Subject Score
Subject="Math"
score =math
output;
Subject="science"
score=science
output;
run;
```

2. pivot up

```.sas
If Subject="Math" then Math=Score
else If Subject="Science" then Science=Score
```

3. Transpose
```.sas
- pivot down
proc transpose data=.. out=..(drop=x) <prefix=> <name=>
id =col_name<d>
by=col_names...<a,b,c>
var=..
run;

- pivot up
by a b c;
var wind1-wind4;
```
