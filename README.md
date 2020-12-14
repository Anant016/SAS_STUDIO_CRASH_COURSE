## SAS_STUDIO_CRASH_COURSE

### 1. Create Dataset

```.sas
DATA mydata;
INPUT name$ mobile gender$;
DATALINES;
Anant 9312585135 M
Priya 9999 F
;
RUN;
```

### 2. Print

```.sas
PROC PRINT DATA=mydata;
RUN;
```

### 3. Some Proc statements

```.sas
PROC CONTENTS DATA=mydata; *metadata;
PROC MEANS DATA=mydata; *mean,max,min,SD;
PROC UNIVARIATE DATA=mydata; *all details, use BY/ID;
PROC FREQ DATA=mydata;
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
