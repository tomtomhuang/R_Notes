# Merging Dataframes

## Introduction
One of the important steps of data cleaning is to put several dataset together. Below are a few examples of dataframe merging. 

## Import data into RStudio
``` r
GPA <- read.csv(".../.../GPA.csv", header = T)
DOB <- read.csv(".../.../DOB.csv", header = T)
Postal <- read.csv(".../.../Postal Code.csv", header = T)
```
#### GPA
| ID          | Name        | Gender      | GPA             | 
|:------------|:------------|:------------|:----------------|
| 1           | Tom         | M           | 3.97            |
| 2           | Sally       | F           | 4.00            |
| 3           | Chris       | M           | 3.60            | 
| 4           | Emma        | F           | 3.80            | 

#### DOB
|ID |Name |Gender|DOB       |
|---|-----|------|----------|
|2  |Sally|Female|22/6/2000 |
|3  |Chris|Male  |18/2/1992 |
|4  |Emma |Female|31/12/1999|

#### Postal
|ID |col 1|col 2 |Postal Code|
|---|-----|------|-----------|
|1  |Tom  |Male  |94078      |
|2  |Sally|Female|90006      |
|5  |Harvey|Male |80912      |
|6  |Grace|Female|45678      |

<br />

## Merge two dataframes, GPA and DOB
Merge the first two dataframes with only overlapping observations
``` r
M1 <- merge(GPA, DOB)
View(M1)
```
|ID   |Name  |Gender    |GPA|DOB       |
|-----|------|----------|---|----------|
|2    |Sally |Female    |4  |22/6/2000 |
|3    |Chris |Male      |3.6|18/2/1992 |
|4    |Emma  |Female    |3.8|31/12/1999|


Merge the first two dataframes with all observations where empty values to be reflected with NA
``` r
M2 <- merge(GPA, DOB, all = TRUE) # the argument all = TRUE means to include all datapoints
View(M2)
```
|ID   |Name  |Gender    |GPA|DOB       |
|-----|------|----------|---|----------|
|1    |Tom   |Male      |3.97|NA        |
|2    |Sally |Female    |4  |22/6/2000 |
|3    |Chris |Male      |3.6|18/2/1992 |
|4    |Emma  |Female    |3.8|31/12/1999|

## Merge dataframes which column names needed to be mapped and specificed
``` r
M3 <- merge(M2, Postal, by.x = c("Name", "Gender", "ID"), by.y = c("col.1", "col.2", "ID"), all = TRUE)
View(M3)
```
|Name  |Gender|ID |GPA |DOB       |Postal.Code|
|------|------|---|----|----------|-----------|
|Chris |Male  |3  |3.6 |18/2/1992 |NA         |
|Emma  |Female|4  |3.8 |31/12/1999|NA         |
|Grace |Female|6  |NA  |NA        |45678      |
|Harvey|Male  |5  |NA  |NA        |80912      |
|Sally |Female|2  |4   |22/6/2000 |90006      |
|Tom   |Male  |1  |3.97|NA        |94078      |

## Left join to merge dataframes using base R
``` r
# ---- Left join by ID while mapping column names ----
M4 <- merge(M2, Postal, by = "ID", by.x = c("Name", "Gender", "ID"), by.y = c("col.1", "col.2", "ID"), all = TRUE) 
```
|Name  |Gender|ID |GPA |DOB       |Postal.Code|
|------|------|---|----|----------|-----------|
|Chris |Male  |3  |3.6 |18/2/1992 |NA         |
|Emma  |Female|4  |3.8 |31/12/1999|NA         |
|Grace |Female|6  |NA  |NA        |45678      |
|Harvey|Male  |5  |NA  |NA        |80912      |
|Sally |Female|2  |4   |22/6/2000 |90006      |
|Tom   |Male  |1  |3.97|NA        |94078      |
