### cut() or Hmisc::cut2() help in binning a continuous variable by quantiles or intervals as required but they differ in the quantile grouping levels they produce
|![](https://github.com/Tanmoy-Rath/misc/blob/master/imageshack/1cutcut2.https...www.browserling.com.tools.bmp-to-jpg.jpg)|![](https://github.com/Tanmoy-Rath/misc/blob/master/imageshack/2cutcut2.https...www.browserling.com.tools.bmp-to-jpg.jpg)|
|---|---|

```R
# cuts it into 5 quantile groups
> table(mergedData$Income.Group, cut2(mergedData$V2, g=5))
                      
                       [  3, 42) [ 42, 80) [ 80,117) [117,155) [155,191]
                               0         0         0         0         0
  High income: nonOECD         5         6         1         6         5
  High income: OECD            5         1         8        13         3
  Low income                  11        17         4         1         4
  Lower middle income         10         9        11        12        12
  Upper middle income          7         5        14         6        13

# cuts it into 5 quantile groups
> table(mergedData$Income.Group, cut(mergedData$V2, breaks=quantile(mergedData$V2,probs=seq(0,1,0.20)), dig.lab=8))
                      
                       (3,41.6] (41.6,79.2] (79.2,115.8] (115.8,153.4] (153.4,191]
                              0           0            0             0           0
  High income: nonOECD        5           6            1             6           5
  High income: OECD           4           1            8            13           3
  Low income                 11          17            4             1           4
  Lower middle income        10           9           11            11          13
  Upper middle income         7           5           13             7          13

> quantile(mergedData$V2,probs=seq(0,1,0.20))
   0%   20%   40%   60%   80%  100% 
  3.0  41.6  79.2 115.8 153.4 191.0 



# cuts it into 5 intervals, not 5 quantile groups
> table(mergedData$Income.Group, cut(mergedData$V2, breaks=5, dig.lab=8))
                      
                       (2.812,40.6] (40.6,78.2] (78.2,115.8] (115.8,153.4] (153.4,191.188]
                                  0           0            0             0               0
  High income: nonOECD            5           6            1             6               5
  High income: OECD               5           1            8            13               3
  Low income                     11          16            5             1               4
  Lower middle income             9          10           11            11              13
  Upper middle income             7           5           13             7              13
> 
```
