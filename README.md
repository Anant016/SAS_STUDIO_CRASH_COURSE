## SAS_STUDIO_CRASH_COURSE

### 1. Create Dataset

```.sas
DATA mydata;
INPUT name $ 1-48 mobile gender$;
DATALINES;
Anant 9312585135 M
Priya 9999 F
;
RUN;
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
   by Name;
run;

```

### 4. Import

```.sas
data imp_data;
infile '/folders/myfolders/b.txt' dlm=',';
input A$ B;
run;

*PROC IMPORT datafile ='/folders/myfolders/a.xlsx' OUT=imp_data.SAS;
*run;
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
```

### 7. Set/ Concatenate 
- same attributes

```.sas
data x;
   set data1 data2;
run;
```

### 8. Merge
- like join 

```.sas
data employee_info;
   merge company finance;
   by name;
run;
```
