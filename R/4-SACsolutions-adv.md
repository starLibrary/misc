https://www.r-bloggers.com/a-quick-primer-on-split-apply-combine-problems/<br/>
https://vita.had.co.nz/papers/plyr.pdf<br/>
A wonderful article: https://stat545.com/block024_group-nest-split-map.html<br/>
# S-A-C solutions (advanced)

### Technique-1
<details>
  <summary><b>s <- split(airquality, airquality$Month)</b> click to expand structure of <b>s</b> !!</summary>
  
```R
> s
$`5`
   Ozone Solar.R Wind Temp Month Day
1     41     190  7.4   67     5   1
2     36     118  8.0   72     5   2
3     12     149 12.6   74     5   3
4     18     313 11.5   62     5   4
5     NA      NA 14.3   56     5   5
6     28      NA 14.9   66     5   6
7     23     299  8.6   65     5   7
8     19      99 13.8   59     5   8
9      8      19 20.1   61     5   9
10    NA     194  8.6   69     5  10
11     7      NA  6.9   74     5  11
12    16     256  9.7   69     5  12
13    11     290  9.2   66     5  13
14    14     274 10.9   68     5  14
15    18      65 13.2   58     5  15
16    14     334 11.5   64     5  16
17    34     307 12.0   66     5  17
18     6      78 18.4   57     5  18
19    30     322 11.5   68     5  19
20    11      44  9.7   62     5  20
21     1       8  9.7   59     5  21
22    11     320 16.6   73     5  22
23     4      25  9.7   61     5  23
24    32      92 12.0   61     5  24
25    NA      66 16.6   57     5  25
26    NA     266 14.9   58     5  26
27    NA      NA  8.0   57     5  27
28    23      13 12.0   67     5  28
29    45     252 14.9   81     5  29
30   115     223  5.7   79     5  30
31    37     279  7.4   76     5  31

$`6`
   Ozone Solar.R Wind Temp Month Day
32    NA     286  8.6   78     6   1
33    NA     287  9.7   74     6   2
34    NA     242 16.1   67     6   3
35    NA     186  9.2   84     6   4
36    NA     220  8.6   85     6   5
37    NA     264 14.3   79     6   6
38    29     127  9.7   82     6   7
39    NA     273  6.9   87     6   8
40    71     291 13.8   90     6   9
41    39     323 11.5   87     6  10
42    NA     259 10.9   93     6  11
43    NA     250  9.2   92     6  12
44    23     148  8.0   82     6  13
45    NA     332 13.8   80     6  14
46    NA     322 11.5   79     6  15
47    21     191 14.9   77     6  16
48    37     284 20.7   72     6  17
49    20      37  9.2   65     6  18
50    12     120 11.5   73     6  19
51    13     137 10.3   76     6  20
52    NA     150  6.3   77     6  21
53    NA      59  1.7   76     6  22
54    NA      91  4.6   76     6  23
55    NA     250  6.3   76     6  24
56    NA     135  8.0   75     6  25
57    NA     127  8.0   78     6  26
58    NA      47 10.3   73     6  27
59    NA      98 11.5   80     6  28
60    NA      31 14.9   77     6  29
61    NA     138  8.0   83     6  30

$`7`
   Ozone Solar.R Wind Temp Month Day
62   135     269  4.1   84     7   1
63    49     248  9.2   85     7   2
64    32     236  9.2   81     7   3
65    NA     101 10.9   84     7   4
66    64     175  4.6   83     7   5
67    40     314 10.9   83     7   6
68    77     276  5.1   88     7   7
69    97     267  6.3   92     7   8
70    97     272  5.7   92     7   9
71    85     175  7.4   89     7  10
72    NA     139  8.6   82     7  11
73    10     264 14.3   73     7  12
74    27     175 14.9   81     7  13
75    NA     291 14.9   91     7  14
76     7      48 14.3   80     7  15
77    48     260  6.9   81     7  16
78    35     274 10.3   82     7  17
79    61     285  6.3   84     7  18
80    79     187  5.1   87     7  19
81    63     220 11.5   85     7  20
82    16       7  6.9   74     7  21
83    NA     258  9.7   81     7  22
84    NA     295 11.5   82     7  23
85    80     294  8.6   86     7  24
86   108     223  8.0   85     7  25
87    20      81  8.6   82     7  26
88    52      82 12.0   86     7  27
89    82     213  7.4   88     7  28
90    50     275  7.4   86     7  29
91    64     253  7.4   83     7  30
92    59     254  9.2   81     7  31

$`8`
    Ozone Solar.R Wind Temp Month Day
93     39      83  6.9   81     8   1
94      9      24 13.8   81     8   2
95     16      77  7.4   82     8   3
96     78      NA  6.9   86     8   4
97     35      NA  7.4   85     8   5
98     66      NA  4.6   87     8   6
99    122     255  4.0   89     8   7
100    89     229 10.3   90     8   8
101   110     207  8.0   90     8   9
102    NA     222  8.6   92     8  10
103    NA     137 11.5   86     8  11
104    44     192 11.5   86     8  12
105    28     273 11.5   82     8  13
106    65     157  9.7   80     8  14
107    NA      64 11.5   79     8  15
108    22      71 10.3   77     8  16
109    59      51  6.3   79     8  17
110    23     115  7.4   76     8  18
111    31     244 10.9   78     8  19
112    44     190 10.3   78     8  20
113    21     259 15.5   77     8  21
114     9      36 14.3   72     8  22
115    NA     255 12.6   75     8  23
116    45     212  9.7   79     8  24
117   168     238  3.4   81     8  25
118    73     215  8.0   86     8  26
119    NA     153  5.7   88     8  27
120    76     203  9.7   97     8  28
121   118     225  2.3   94     8  29
122    84     237  6.3   96     8  30
123    85     188  6.3   94     8  31

$`9`
    Ozone Solar.R Wind Temp Month Day
124    96     167  6.9   91     9   1
125    78     197  5.1   92     9   2
126    73     183  2.8   93     9   3
127    91     189  4.6   93     9   4
128    47      95  7.4   87     9   5
129    32      92 15.5   84     9   6
130    20     252 10.9   80     9   7
131    23     220 10.3   78     9   8
132    21     230 10.9   75     9   9
133    24     259  9.7   73     9  10
134    44     236 14.9   81     9  11
135    21     259 15.5   76     9  12
136    28     238  6.3   77     9  13
137     9      24 10.9   71     9  14
138    13     112 11.5   71     9  15
139    46     237  6.9   78     9  16
140    18     224 13.8   67     9  17
141    13      27 10.3   76     9  18
142    24     238 10.3   68     9  19
143    16     201  8.0   82     9  20
144    13     238 12.6   64     9  21
145    23      14  9.2   71     9  22
146    36     139 10.3   81     9  23
147     7      49 10.3   69     9  24
148    14      20 16.6   63     9  25
149    30     193  6.9   70     9  26
150    NA     145 13.2   77     9  27
151    14     191 14.3   75     9  28
152    18     131  8.0   76     9  29
153    20     223 11.5   68     9  30
```
</details>

```R
> s <- split(airquality, airquality$Month)
> sapply(  s, function(x) colMeans(x[,c("Ozone","Solar.R","Wind")])  )
               5         6          7        8        9
Ozone         NA        NA         NA       NA       NA
Solar.R       NA 190.16667 216.483871       NA 167.4333
Wind    11.62258  10.26667   8.941935 8.793548  10.1800

# If you want to remove NAs...
> sapply(  s, function(x) colMeans(x[,c("Ozone","Solar.R","Wind","Temp")],na.rm=TRUE)  )
                5         6          7          8         9
Ozone    23.61538  29.44444  59.115385  59.961538  31.44828
Solar.R 181.29630 190.16667 216.483871 171.857143 167.43333
Wind     11.62258  10.26667   8.941935   8.793548  10.18000
Temp     65.54839  79.10000  83.903226  83.967742  76.90000
```


### Technique-2
**READ FIRST**: https://stackoverflow.com/questions/2851327/convert-a-list-of-data-frames-into-one-data-frame
```R
> s <- split(airquality, airquality$Month)
> do.call(   rbind, lapply(  s, function(x) colMeans(x[,c("Ozone","Solar.R","Wind","Temp")],na.rm=TRUE)  )   )
     Ozone  Solar.R      Wind     Temp
5 23.61538 181.2963 11.622581 65.54839
6 29.44444 190.1667 10.266667 79.10000
7 59.11538 216.4839  8.941935 83.90323
8 59.96154 171.8571  8.793548 83.96774
9 31.44828 167.4333 10.180000 76.90000
```


### Technique 3
```R
> aggregate(airquality[,c("Ozone","Solar.R","Wind","Temp")], list(Month=airquality$Month), mean, na.rm=TRUE)
  Month    Ozone  Solar.R      Wind     Temp
1     5 23.61538 181.2963 11.622581 65.54839
2     6 29.44444 190.1667 10.266667 79.10000
3     7 59.11538 216.4839  8.941935 83.90323
4     8 59.96154 171.8571  8.793548 83.96774
5     9 31.44828 167.4333 10.180000 76.90000

> aggregate(airquality[,c("Ozone","Solar.R","Wind","Temp")], list(Month=airquality$Month), range, na.rm=TRUE)
  Month Ozone.1 Ozone.2 Solar.R.1 Solar.R.2 Wind.1 Wind.2 Temp.1 Temp.2
1     5       1     115         8       334    5.7   20.1     56     81
2     6      12      71        31       332    1.7   20.7     65     93
3     7       7     135         7       314    4.1   14.9     73     92
4     8       9     168        24       273    2.3   15.5     72     97
5     9       7      96        14       259    2.8   16.6     63     93

# Formula method
> aggregate(. ~ Month ,data = airquality, mean)
  Month    Ozone  Solar.R      Wind     Temp      Day
1     5 24.12500 182.0417 11.504167 66.45833 16.08333
2     6 29.44444 184.2222 12.177778 78.22222 14.33333
3     7 59.11538 216.4231  8.523077 83.88462 16.23077
4     8 60.00000 173.0870  8.860870 83.69565 17.17391
5     9 31.44828 168.2069 10.075862 76.89655 15.10345

> aggregate(. ~ Month, airquality, range)
  Month Ozone.1 Ozone.2 Solar.R.1 Solar.R.2 Wind.1 Wind.2 Temp.1 Temp.2 Day.1 Day.2
1     5       1     115         8       334    5.7   20.1     57     81     1    31
2     6      12      71        37       323    8.0   20.7     65     90     7    20
3     7       7     135         7       314    4.1   14.9     73     92     1    31
4     8       9     168        24       273    2.3   15.5     72     97     1    31
5     9       7      96        14       259    2.8   16.6     63     93     1    30
```


### Technique 4
```R
> library(dplyr)
> airquality %>% group_by(Month) %>% summarise_at(vars(Ozone, Solar.R, Wind, Temp), mean, na.rm=TRUE)
# A tibble: 5 x 5
  Month Ozone Solar.R  Wind  Temp
  <int> <dbl>   <dbl> <dbl> <dbl>
1     5  23.6    181. 11.6   65.5
2     6  29.4    190. 10.3   79.1
3     7  59.1    216.  8.94  83.9
4     8  60.0    172.  8.79  84.0
5     9  31.4    167. 10.2   76.9
# You dont need to specify the grouping variable in the vars() list

# But it doesn't work with multiple output functions, such as range()
> airquality %>% group_by(Month) %>% summarise_at(vars(Ozone, Solar.R, Wind, Temp), range, na.rm=TRUE)
Error: Column `Ozone` must be length 1 (a summary value), not 2

> # if you want to summarize all the columns
> airquality %>% group_by(Month) %>% summarise_all(mean, na.rm=TRUE)
# A tibble: 5 x 6
  Month Ozone Solar.R  Wind  Temp   Day
  <int> <dbl>   <dbl> <dbl> <dbl> <dbl>
1     5  23.6    181. 11.6   65.5  16  
2     6  29.4    190. 10.3   79.1  15.5
3     7  59.1    216.  8.94  83.9  16  
4     8  60.0    172.  8.79  84.0  16  
5     9  31.4    167. 10.2   76.9  15.5
> 
```


### Technique
```R
> vapply(  s, function(x) colMeans(x[,c("Ozone","Solar.R","Wind","Temp")],na.rm=TRUE), numeric(4)  )
                5         6          7          8         9
Ozone    23.61538  29.44444  59.115385  59.961538  31.44828
Solar.R 181.29630 190.16667 216.483871 171.857143 167.43333
Wind     11.62258  10.26667   8.941935   8.793548  10.18000
Temp     65.54839  79.10000  83.903226  83.967742  76.90000
```


### Technique-9
```R
> by(airquality, airquality$Month, function(x) colMeans(x[,c("Ozone","Solar.R","Wind","Temp")],na.rm=TRUE)  )
airquality$Month: 5
    Ozone   Solar.R      Wind      Temp 
 23.61538 181.29630  11.62258  65.54839 
-------------------------------------------------------------------------------------------------- 
airquality$Month: 6
    Ozone   Solar.R      Wind      Temp 
 29.44444 190.16667  10.26667  79.10000 
-------------------------------------------------------------------------------------------------- 
airquality$Month: 7
     Ozone    Solar.R       Wind       Temp 
 59.115385 216.483871   8.941935  83.903226 
-------------------------------------------------------------------------------------------------- 
airquality$Month: 8
     Ozone    Solar.R       Wind       Temp 
 59.961538 171.857143   8.793548  83.967742 
-------------------------------------------------------------------------------------------------- 
airquality$Month: 9
    Ozone   Solar.R      Wind      Temp 
 31.44828 167.43333  10.18000  76.90000 
```


### combo Technique
```R
> spl <- split(airquality, airquality$Month)
> lapply(   spl,   function(x)   sapply(  x[,c("Ozone","Solar.R","Wind","Temp")],  function(x)  range(x,na.rm=TRUE)  )   )
$`5`
     Ozone Solar.R Wind Temp
[1,]     1       8  5.7   56
[2,]   115     334 20.1   81

$`6`
     Ozone Solar.R Wind Temp
[1,]    12      31  1.7   65
[2,]    71     332 20.7   93

$`7`
     Ozone Solar.R Wind Temp
[1,]     7       7  4.1   73
[2,]   135     314 14.9   92

$`8`
     Ozone Solar.R Wind Temp
[1,]     9      24  2.3   72
[2,]   168     273 15.5   97

$`9`
     Ozone Solar.R Wind Temp
[1,]     7      14  2.8   63
[2,]    96     259 16.6   93



> lapply(   spl,   function(x)   sapply(  x[,c("Ozone","Solar.R","Wind","Temp")],  summary  )   )
$`5`
$`5`$Ozone
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
   1.00   11.00   18.00   23.62   31.50  115.00       5 

$`5`$Solar.R
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
    8.0    72.0   194.0   181.3   284.5   334.0       4 

$`5`$Wind
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
   5.70    8.90   11.50   11.62   14.05   20.10 

$`5`$Temp
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  56.00   60.00   66.00   65.55   69.00   81.00 


$`6`
$`6`$Ozone
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
  12.00   20.00   23.00   29.44   37.00   71.00      21 

$`6`$Solar.R
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
   31.0   127.0   188.5   190.2   270.8   332.0 

$`6`$Wind
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
   1.70    8.00    9.70   10.27   11.50   20.70 

$`6`$Temp
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  65.00   76.00   78.00   79.10   82.75   93.00 


$`7`
$`7`$Ozone
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
   7.00   36.25   60.00   59.12   79.75  135.00       5 

$`7`$Solar.R
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    7.0   175.0   253.0   216.5   273.0   314.0 

$`7`$Wind
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  4.100   6.900   8.600   8.942  10.900  14.900 

$`7`$Temp
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
   73.0    81.5    84.0    83.9    86.0    92.0 


$`8`
$`8`$Ozone
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
   9.00   28.75   52.00   59.96   82.50  168.00       5 

$`8`$Solar.R
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
   24.0   107.0   197.5   171.9   231.0   273.0       3 

$`8`$Wind
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  2.300   6.600   8.600   8.794  11.200  15.500 

$`8`$Temp
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  72.00   79.00   82.00   83.97   88.50   97.00 


$`9`
$`9`$Ozone
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    NA's 
   7.00   16.00   23.00   31.45   36.00   96.00       1 

$`9`$Solar.R
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
   14.0   116.8   192.0   167.4   234.5   259.0 

$`9`$Wind
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
   2.80    7.55   10.30   10.18   12.32   16.60 

$`9`$Temp
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
   63.0    71.0    76.0    76.9    81.0    93.0 
```
