---
title: "Clean Up"
author: "Ben Elam"
date: "March 1, 2016"
output: html_document
---
First of all, we're going to be really, really unhappy until we break this up into different data sets.
Remember that problem with missing values becoming strings and strings becoming factors?
It's happening here!

We have NA values, missing values, and even a value of "Na". This mixed case will be treated as a string, not as an NA value.


```r
set_sp_grp = function(species) {
  if (species %in% c('Q.stellata', 'Q.alba')) # Post and white
    return('WhiteOak')
  else if (species %in% c('Q.shumardii', 'Q.pagoda')) # Shumard and cherrybark
    return('RedOak')
  else if (species %in% c('C.ovata', 'C.glabra')) # Shagbark and pignut hickories 
    return('Carya')
  else return(NA)
}

set_sp_grp = Vectorize(set_sp_grp, vectorize.args = 'species')

setwd('~/work/nmsds/guides/showandtell/atf')
atf = read.csv('ATFMangled.csv',
               stringsAsFactors = FALSE,
               na.strings = c('NA', 'Na', '')) # Skip this step and you're gonna have a bad time.

atf$SpGrp = set_sp_grp(atf$Species)
```

Note that the procedure is the same for all of the ions.
Just change the column names as appropriate.
We'll restrict our investigation to the chloride data.


```r
#clnames = c('Date', 'PgTF', 'Species', 'Cl', 'ClER')
clnames = c('Date', 'Species', 'Cl', 'ClER', 'SpGrp')
cl = atf[atf$Species != 'precipitation', clnames]
names(cl)[names(cl) == 'ClER'] = 'ER'
```

Now, we would like to do the following:

- Split our chloride data set, `cl`, into a list of data frames by date.
- Remove data frames with NA values.
- Merge the good data frames back together.

To accomplish this, we'll first write a function that determines
whether or not a data frame contains an NA entry.


```r
no_NA = function(df) {return(!as.logical(sum(colSums(is.na(df)))))}

cl_dates = split(cl, cl$Date)
cl_dates_clean = cl_dates[sapply(cl_dates, no_NA, simplify = 'vector')]
# Because `cl_clean = rbind(cl_dates_clean, make.row.names=FALSE)` is not what we want:
cl_clean = do.call("rbind", as.list(c(cl_dates_clean, make.row.names=FALSE)))
```

We can aggregate our species group data easily enough:


```r
sp_grp_er = aggregate(ER ~ Date + SpGrp, cl_clean, mean)
names(sp_grp_er)[names(sp_grp_er) == 'ER'] = 'ERSpGrp'
sp_grp_er = sp_grp_er[order(sp_grp_er$Date),]
sp_grp_er
```

```
##          Date    SpGrp    ERSpGrp
## 1  2015_01_13    Carya 0.72691912
## 10 2015_01_13   RedOak 0.24549709
## 19 2015_01_13 WhiteOak 0.82313641
## 2  2015_01_23    Carya 0.28134019
## 11 2015_01_23   RedOak 0.14954205
## 20 2015_01_23 WhiteOak 0.66113005
## 3  2015_02_02    Carya 0.17511998
## 12 2015_02_02   RedOak 0.32625403
## 21 2015_02_02 WhiteOak 0.42185583
## 4  2015_02_17    Carya 0.29565505
## 13 2015_02_17   RedOak 0.26791164
## 22 2015_02_17 WhiteOak 0.30810051
## 5  2015_03_16    Carya 0.05664276
## 14 2015_03_16   RedOak 0.57575012
## 23 2015_03_16 WhiteOak 0.68391768
## 6  2015_03_23    Carya 0.34589331
## 15 2015_03_23   RedOak 0.41716513
## 24 2015_03_23 WhiteOak 0.45800020
## 7  2015_04_19    Carya 0.69434724
## 16 2015_04_19   RedOak 0.40872924
## 25 2015_04_19 WhiteOak 0.39311644
## 8  2015_04_25    Carya 0.92971030
## 17 2015_04_25   RedOak 0.27116108
## 26 2015_04_25 WhiteOak 0.59401961
## 9  2015_09_13    Carya 0.47781593
## 18 2015_09_13   RedOak 0.40972473
## 27 2015_09_13 WhiteOak 0.23089207
```

That's great, but how do we get this data into our `cl_clean` data frame?


```r
merged = merge(cl_clean, sp_grp_er, by = c('Date', 'SpGrp'))
merged = merged[, c('Date', 'Species', 'Cl', 'ER', 'SpGrp', 'ERSpGrp')]
head(merged)
```

```
##         Date     Species        Cl        ER    SpGrp   ERSpGrp
## 1 2015_01_13     C.ovata 0.3727461 0.5320190    Carya 0.7269191
## 2 2015_01_13    C.glabra 0.5758170 0.9218192    Carya 0.7269191
## 3 2015_01_13 Q.shumardii 0.4287336 0.2508475   RedOak 0.2454971
## 4 2015_01_13    Q.pagoda 0.4285066 0.2401467   RedOak 0.2454971
## 5 2015_01_13  Q.stellata 0.6290466 0.9827344 WhiteOak 0.8231364
## 6 2015_01_13      Q.alba 0.5839427 0.6635384 WhiteOak 0.8231364
```

```r
#write.csv(merged, 'data/Chloride.csv', row.names = FALSE)
```
