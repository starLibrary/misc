![](https://github.com/Tanmoy-Rath/misc/blob/master/imageshack/4-dplyr.browserling.com.tools.bmp-to-jpg.jpg)

```R
> # dplyr
> library(dplyr)

Attaching package: ‘dplyr’

The following objects are masked from ‘package:stats’:

    filter, lag

The following objects are masked from ‘package:base’:

    intersect, setdiff, setequal, union

> chicago <- readRDS("chicago.rds")
> dim(chicago)
[1] 6940    8
> str(chicago)
'data.frame':	6940 obs. of  8 variables:
 $ city      : chr  "chic" "chic" "chic" "chic" ...
 $ tmpd      : num  31.5 33 33 29 32 40 34.5 29 26.5 32.5 ...
 $ dptp      : num  31.5 29.9 27.4 28.6 28.9 ...
 $ date      : Date, format: "1987-01-01" "1987-01-02" "1987-01-03" ...
 $ pm25tmean2: num  NA NA NA NA NA NA NA NA NA NA ...
 $ pm10tmean2: num  34 NA 34.2 47 NA ...
 $ o3tmean2  : num  4.25 3.3 3.33 4.38 4.75 ...
 $ no2tmean2 : num  20 23.2 23.8 30.4 30.3 ...
> names(chicago)
[1] "city"       "tmpd"       "dptp"       "date"       "pm25tmean2" "pm10tmean2" "o3tmean2"   "no2tmean2" 
> 
> # selcts columns from city to dptp
> head(select(chicago, city:dptp))
  city tmpd   dptp
1 chic 31.5 31.500
2 chic 33.0 29.875
3 chic 33.0 27.375
4 chic 29.0 28.625
5 chic 32.0 28.875
6 chic 40.0 35.125
> head(select(chicago, city, dptp))
  city   dptp
1 chic 31.500
2 chic 29.875
3 chic 27.375
4 chic 28.625
5 chic 28.875
6 chic 35.125
> # also runs in reverse order
> head(select(chicago, o3tmean2:dptp))
  o3tmean2 pm10tmean2 pm25tmean2       date   dptp
1 4.250000   34.00000         NA 1987-01-01 31.500
2 3.304348         NA         NA 1987-01-02 29.875
3 3.333333   34.16667         NA 1987-01-03 27.375
4 4.375000   47.00000         NA 1987-01-04 28.625
5 4.750000         NA         NA 1987-01-05 28.875
6 5.833333   48.00000         NA 1987-01-06 35.125
> 
> head(select(chicago, -(city:dptp)))
        date pm25tmean2 pm10tmean2 o3tmean2 no2tmean2
1 1987-01-01         NA   34.00000 4.250000  19.98810
2 1987-01-02         NA         NA 3.304348  23.19099
3 1987-01-03         NA   34.16667 3.333333  23.81548
4 1987-01-04         NA   47.00000 4.375000  30.43452
5 1987-01-05         NA         NA 4.750000  30.33333
6 1987-01-06         NA   48.00000 5.833333  25.77233
> head(select(chicago, -(o3tmean2:dptp)))
  city tmpd no2tmean2
1 chic 31.5  19.98810
2 chic 33.0  23.19099
3 chic 33.0  23.81548
4 chic 29.0  30.43452
5 chic 32.0  30.33333
6 chic 40.0  25.77233
> 
> i <- match("city", names(chicago))
> j <- match("dptp", names(chicago))
> head(chicago[, -(i:j)])
        date pm25tmean2 pm10tmean2 o3tmean2 no2tmean2
1 1987-01-01         NA   34.00000 4.250000  19.98810
2 1987-01-02         NA         NA 3.304348  23.19099
3 1987-01-03         NA   34.16667 3.333333  23.81548
4 1987-01-04         NA   47.00000 4.375000  30.43452
5 1987-01-05         NA         NA 4.750000  30.33333
6 1987-01-06         NA   48.00000 5.833333  25.77233
> 
> chic.f <- filter(chicago, pm25tmean2 > 30)
> head(chic.f, 10)
   city tmpd dptp       date pm25tmean2 pm10tmean2  o3tmean2 no2tmean2
1  chic   23 21.9 1998-01-17      38.10   32.46154  3.180556  25.30000
2  chic   28 25.8 1998-01-23      33.95   38.69231  1.750000  29.37630
3  chic   55 51.3 1998-04-30      39.40   34.00000 10.786232  25.31310
4  chic   59 53.7 1998-05-01      35.40   28.50000 14.295125  31.42905
5  chic   57 52.0 1998-05-02      33.30   35.00000 20.662879  26.79861
6  chic   57 56.0 1998-05-07      32.10   34.50000 24.270422  33.99167
7  chic   75 65.8 1998-05-15      56.50   91.00000 38.573007  29.03261
8  chic   61 59.0 1998-06-09      33.80   26.00000 17.890810  25.49668
9  chic   73 60.3 1998-07-13      30.30   64.50000 37.018865  37.93056
10 chic   78 67.1 1998-07-14      41.40   75.00000 40.080902  32.59054
> chic.f <- filter(chicago, pm25tmean2 > 30 & tmpd > 80)
> head(chic.f)
  city tmpd dptp       date pm25tmean2 pm10tmean2 o3tmean2 no2tmean2
1 chic   81 71.2 1998-08-23    39.6000       59.0 45.86364  14.32639
2 chic   81 70.4 1998-09-06    31.5000       50.5 50.66250  20.31250
3 chic   82 72.2 2001-07-20    32.3000       58.5 33.00380  33.67500
4 chic   84 72.9 2001-08-01    43.7000       81.5 45.17736  27.44239
5 chic   85 72.6 2001-08-08    38.8375       70.0 37.98047  27.62743
6 chic   84 72.6 2001-08-09    38.2000       66.0 36.73245  26.46742
> 
> chicago <- arrange(chicago, date)
> head(chicago)
  city tmpd   dptp       date pm25tmean2 pm10tmean2 o3tmean2 no2tmean2
1 chic 31.5 31.500 1987-01-01         NA   34.00000 4.250000  19.98810
2 chic 33.0 29.875 1987-01-02         NA         NA 3.304348  23.19099
3 chic 33.0 27.375 1987-01-03         NA   34.16667 3.333333  23.81548
4 chic 29.0 28.625 1987-01-04         NA   47.00000 4.375000  30.43452
5 chic 32.0 28.875 1987-01-05         NA         NA 4.750000  30.33333
6 chic 40.0 35.125 1987-01-06         NA   48.00000 5.833333  25.77233
> tail(chicago)
     city tmpd dptp       date pm25tmean2 pm10tmean2  o3tmean2 no2tmean2
6935 chic   35 29.6 2005-12-26    8.40000        8.5 14.041667  16.81944
6936 chic   40 33.6 2005-12-27   23.56000       27.0  4.468750  23.50000
6937 chic   37 34.5 2005-12-28   17.75000       27.5  3.260417  19.28563
6938 chic   35 29.4 2005-12-29    7.45000       23.5  6.794837  19.97222
6939 chic   36 31.0 2005-12-30   15.05714       19.2  3.034420  22.80556
6940 chic   35 30.1 2005-12-31   15.00000       23.5  2.531250  13.25000
> chicago <- arrange(chicago, desc(date))
> head(chicago)
  city tmpd dptp       date pm25tmean2 pm10tmean2  o3tmean2 no2tmean2
1 chic   35 30.1 2005-12-31   15.00000       23.5  2.531250  13.25000
2 chic   36 31.0 2005-12-30   15.05714       19.2  3.034420  22.80556
3 chic   35 29.4 2005-12-29    7.45000       23.5  6.794837  19.97222
4 chic   37 34.5 2005-12-28   17.75000       27.5  3.260417  19.28563
5 chic   40 33.6 2005-12-27   23.56000       27.0  4.468750  23.50000
6 chic   35 29.6 2005-12-26    8.40000        8.5 14.041667  16.81944
> tail(chicago)
     city tmpd   dptp       date pm25tmean2 pm10tmean2 o3tmean2 no2tmean2
6935 chic 40.0 35.125 1987-01-06         NA   48.00000 5.833333  25.77233
6936 chic 32.0 28.875 1987-01-05         NA         NA 4.750000  30.33333
6937 chic 29.0 28.625 1987-01-04         NA   47.00000 4.375000  30.43452
6938 chic 33.0 27.375 1987-01-03         NA   34.16667 3.333333  23.81548
6939 chic 33.0 29.875 1987-01-02         NA         NA 3.304348  23.19099
6940 chic 31.5 31.500 1987-01-01         NA   34.00000 4.250000  19.98810
> 
> # check rename function in R
> chicago <- rename(chicago, pm25=pm25tmean2, dewpoint=dptp)
> head(chicago)
  city tmpd dewpoint       date     pm25 pm10tmean2  o3tmean2 no2tmean2
1 chic   35     30.1 2005-12-31 15.00000       23.5  2.531250  13.25000
2 chic   36     31.0 2005-12-30 15.05714       19.2  3.034420  22.80556
3 chic   35     29.4 2005-12-29  7.45000       23.5  6.794837  19.97222
4 chic   37     34.5 2005-12-28 17.75000       27.5  3.260417  19.28563
5 chic   40     33.6 2005-12-27 23.56000       27.0  4.468750  23.50000
6 chic   35     29.6 2005-12-26  8.40000        8.5 14.041667  16.81944
> 
> chicago <- mutate(chicago, pm25detrend=pm25-mean(pm25, na.rm=TRUE))
> head(select(chicago, pm25, pm25detrend))
      pm25 pm25detrend
1 15.00000   -1.230958
2 15.05714   -1.173815
3  7.45000   -8.780958
4 17.75000    1.519042
5 23.56000    7.329042
6  8.40000   -7.830958
> 
> chicago <- mutate(chicago, tempcat=factor(1*(tmpd>80), labels=c("cold","hot")) )
> hotcold <- group_by(chicago, tempcat)
Warning message:
Factor `tempcat` contains implicit NA, consider using `forcats::fct_explicit_na` 
> hotcold
# A tibble: 6,940 x 10
# Groups:   tempcat [3]
   city   tmpd dewpoint date        pm25 pm10tmean2 o3tmean2 no2tmean2 pm25detrend tempcat
   <chr> <dbl>    <dbl> <date>     <dbl>      <dbl>    <dbl>     <dbl>       <dbl> <fct>  
 1 chic     35     30.1 2005-12-31 15          23.5     2.53      13.2       -1.23 cold   
 2 chic     36     31   2005-12-30 15.1        19.2     3.03      22.8       -1.17 cold   
 3 chic     35     29.4 2005-12-29  7.45       23.5     6.79      20.0       -8.78 cold   
 4 chic     37     34.5 2005-12-28 17.8        27.5     3.26      19.3        1.52 cold   
 5 chic     40     33.6 2005-12-27 23.6        27       4.47      23.5        7.33 cold   
 6 chic     35     29.6 2005-12-26  8.4         8.5    14.0       16.8       -7.83 cold   
 7 chic     35     32.1 2005-12-25  6.7         8      14.4       13.8       -9.53 cold   
 8 chic     37     35.2 2005-12-24 30.8        25.2     1.77      32.0       14.5  cold   
 9 chic     41     32.6 2005-12-23 32.9        34.5     6.91      29.1       16.7  cold   
10 chic     22     23.3 2005-12-22 36.6        42.5     5.39      33.7       20.4  cold   
# ... with 6,930 more rows
> summarize(hotcold, pm25=mean(pm25), o3=max(o3tmean2), no2=median(no2tmean2))
# A tibble: 3 x 4
  tempcat  pm25    o3   no2
  <fct>   <dbl> <dbl> <dbl>
1 cold     NA   66.6   24.5
2 hot      NA   63.0   24.9
3 NA       47.7  9.42  37.4
> summarize(hotcold, pm25=mean(pm25, na.rm=TRUE), o3=max(o3tmean2), no2=median(no2tmean2))
# A tibble: 3 x 4
  tempcat  pm25    o3   no2
  <fct>   <dbl> <dbl> <dbl>
1 cold     16.0 66.6   24.5
2 hot      26.5 63.0   24.9
3 NA       47.7  9.42  37.4
> 
> chicago <- mutate(chicago, year=as.POSIXlt(date)$year + 1900)
> years <- group_by(chicago, year)
> summarize(years, pm25=mean(pm25, na.rm=TRUE), o3=max(o3tmean2), no2=median(no2tmean2))
# A tibble: 19 x 4
    year  pm25    o3   no2
   <dbl> <dbl> <dbl> <dbl>
 1  1987 NaN    63.0  23.5
 2  1988 NaN    61.7  24.5
 3  1989 NaN    59.7  26.1
 4  1990 NaN    52.2  22.6
 5  1991 NaN    63.1  21.4
 6  1992 NaN    50.8  24.8
 7  1993 NaN    44.3  25.8
 8  1994 NaN    52.2  28.5
 9  1995 NaN    66.6  27.3
10  1996 NaN    58.4  26.4
11  1997 NaN    56.5  25.5
12  1998  18.3  50.7  24.6
13  1999  18.5  57.5  24.7
14  2000  16.9  55.8  23.5
15  2001  16.9  51.8  25.1
16  2002  15.3  54.9  22.7
17  2003  15.2  56.2  24.6
18  2004  14.6  44.5  23.4
19  2005  16.2  58.8  22.6
> 
> # pipeline operator, no need for temporary variables
> # skip the first argument of any function
> chicago %>% mutate(month=as.POSIXlt(date)$mon + 1) %>% group_by(month) %>% summarize(pm25=mean(pm25, na.rm=TRUE), o3=max(o3tmean2), no2=median(no2tmean2))
# A tibble: 12 x 4
   month  pm25    o3   no2
   <dbl> <dbl> <dbl> <dbl>
 1     1  17.8  28.2  25.4
 2     2  20.4  37.4  26.8
 3     3  17.4  39.0  26.8
 4     4  13.9  47.9  25.0
 5     5  14.1  52.8  24.2
 6     6  15.9  66.6  25.0
 7     7  16.6  59.5  22.4
 8     8  16.9  54.0  23.0
 9     9  15.9  57.5  24.5
10    10  14.2  47.1  24.2
11    11  15.2  29.5  23.6
12    12  17.5  27.7  24.5
> mtcars %>% sapply(sd)
        mpg         cyl        disp          hp        drat          wt        qsec          vs 
  6.0269481   1.7859216 123.9386938  68.5628685   0.5346787   0.9784574   1.7869432   0.5040161 
         am        gear        carb 
  0.4989909   0.7378041   1.6152000 
> # dplyr can work with other dataframe backends
> # datatables for large fast tables
> # SQL interface for relational databases via the DBI package
> 
```
