```R
> library(ggplot2)
> str(mpg)
Classes ‘tbl_df’, ‘tbl’ and 'data.frame':	234 obs. of  11 variables:
 $ manufacturer: chr  "audi" "audi" "audi" "audi" ...
 $ model       : chr  "a4" "a4" "a4" "a4" ...
 $ displ       : num  1.8 1.8 2 2 2.8 2.8 3.1 1.8 1.8 2 ...
 $ year        : int  1999 1999 2008 2008 1999 1999 2008 1999 1999 2008 ...
 $ cyl         : int  4 4 4 4 6 6 6 4 4 4 ...
 $ trans       : chr  "auto(l5)" "manual(m5)" "manual(m6)" "auto(av)" ...
 $ drv         : chr  "f" "f" "f" "f" ...
 $ cty         : int  18 21 20 21 16 18 18 18 16 20 ...
 $ hwy         : int  29 29 31 30 26 26 27 26 25 28 ...
 $ fl          : chr  "p" "p" "p" "p" ...
 $ class       : chr  "compact" "compact" "compact" "compact" ...
```

|||
|---|---|
|qplot(displ, hwy, data = mpg)|qplot(displ, hwy, data=mpg, color=drv)|
|<img src="https://github.com/Tanmoy-Rath/misc/blob/master/imageshack/exploratory/ggplot2%20(part%202)01.png">|<img src="https://github.com/Tanmoy-Rath/misc/blob/master/imageshack/exploratory/ggplot2%20(part%202)02.png">|



```R
> qplot(displ, hwy, data = mpg)
```
![](https://github.com/Tanmoy-Rath/misc/blob/master/imageshack/exploratory/ggplot2%20(part%202)01.png)

```R
> qplot(displ, hwy, data=mpg, color=drv)
```

![](https://github.com/Tanmoy-Rath/misc/blob/master/imageshack/exploratory/ggplot2%20(part%202)02.png)

```R
> qplot(displ, hwy, data=mpg, geom=c("point","smooth"))
`geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

![](https://github.com/Tanmoy-Rath/misc/blob/master/imageshack/exploratory/ggplot2%20(part%202)03.png)

```R
> qplot(hwy, data=mpg, fill=drv)
`stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

![](https://github.com/Tanmoy-Rath/misc/blob/master/imageshack/exploratory/ggplot2%20(part%202)04.png)

```R
> qplot(displ, hwy, data=mpg, facets=.~drv)
```

![](https://github.com/Tanmoy-Rath/misc/blob/master/imageshack/exploratory/ggplot2%20(part%202)05.png)

```R
> qplot(displ, hwy, data=mpg, facets=.~drv, color=factor(cyl))
```

![](https://github.com/Tanmoy-Rath/misc/blob/master/imageshack/exploratory/ggplot2%20(part%202)extra01.png)

```R
> qplot(displ, hwy, data=mpg, facets=.~cyl, color=factor(drv))
```

![](https://github.com/Tanmoy-Rath/misc/blob/master/imageshack/exploratory/ggplot2%20(part%202)extra02.png)

```R
> qplot(hwy, data=mpg, facets = drv~., binwidth=2)
```

![](https://github.com/Tanmoy-Rath/misc/blob/master/imageshack/exploratory/ggplot2%20(part%202)06.png)

```R
> load("maacs.rda")
> str(maacs)
'data.frame':	750 obs. of  9 variables:
 $ id            : int  1 2 3 4 5 6 7 8 9 10 ...
 $ eno           : num  141 124 126 164 99 68 41 50 12 30 ...
 $ duBedMusM     : num  2423 2793 3055 775 1634 ...
 $ pm25          : num  15.6 34.4 39 33.2 27.1 ...
 $ mopos         : Factor w/ 2 levels "no","yes": 2 2 2 2 2 2 2 2 2 2 ...
 $ logpm25       : num  1.19 1.54 1.59 1.52 1.43 ...
 $ NocturnalSympt: int  0 0 2 2 2 2 0 1 0 0 ...
 $ bmicat        : Factor w/ 2 levels "normal weight",..: 1 2 2 1 1 1 2 2 2 1 ...
 $ logno2_new    : num  1.62 1.88 1.71 1.46 1.29 ...
```

```R
> qplot(log(eno), data=maacs)
`stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
Warning message:
Removed 108 rows containing non-finite values (stat_bin). 
```

![](https://github.com/Tanmoy-Rath/misc/blob/master/imageshack/exploratory/ggplot2%20(part%202)07.png)

```R
> qplot(log(eno), data=maacs, fill=mopos)
`stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
Warning message:
Removed 108 rows containing non-finite values (stat_bin). 
```

![](https://github.com/Tanmoy-Rath/misc/blob/master/imageshack/exploratory/ggplot2%20(part%202)08.png)

```R
> qplot(log(eno), data=maacs, geom="density")
Warning message:
Removed 108 rows containing non-finite values (stat_density). 
```

![](https://github.com/Tanmoy-Rath/misc/blob/master/imageshack/exploratory/ggplot2%20(part%202)09.png)

```R
> qplot(log(eno), data=maacs, geom="density", color=mopos)
Warning message:
Removed 108 rows containing non-finite values (stat_density). 
> qplot(log(pm25), log(eno), data=maacs)
Warning message:
Removed 184 rows containing missing values (geom_point). 
> qplot(log(pm25), log(eno), data=maacs, shape=mopos)
Warning message:
Removed 184 rows containing missing values (geom_point). 
> qplot(log(pm25), log(eno), data=maacs, color=mopos)
Warning message:
Removed 184 rows containing missing values (geom_point). 
> qplot(log(pm25), log(eno), data=maacs, color=mopos) + geom_smooth(method = "lm")
Warning messages:
1: Removed 184 rows containing non-finite values (stat_smooth). 
2: Removed 184 rows containing missing values (geom_point). 
> qplot(log(pm25), log(eno), data=maacs, facets=.~mopos) + geom_smooth(method = "lm")
Warning messages:
1: Removed 184 rows containing non-finite values (stat_smooth). 
2: Removed 184 rows containing missing values (geom_point). 
> 
```