https://www.r-bloggers.com/a-quick-primer-on-split-apply-combine-problems/<br/>
https://vita.had.co.nz/papers/plyr.pdf
# Solutions to S-A-C problems (split-apply-combine)
<details>
  <summary><b>InsectSprays data...</b>click to expand!!</summary>

```R
> InsectSprays
   count spray
1     10     A
2      7     A
3     20     A
4     14     A
5     14     A
6     12     A
7     10     A
8     23     A
9     17     A
10    20     A
11    14     A
12    13     A
13    11     B
14    17     B
15    21     B
16    11     B
17    16     B
18    14     B
19    17     B
20    17     B
21    19     B
22    21     B
23     7     B
24    13     B
25     0     C
26     1     C
27     7     C
28     2     C
29     3     C
30     1     C
31     2     C
32     1     C
33     3     C
34     0     C
35     1     C
36     4     C
37     3     D
38     5     D
39    12     D
40     6     D
41     4     D
42     3     D
43     5     D
44     5     D
45     5     D
46     5     D
47     2     D
48     4     D
49     3     E
50     5     E
51     3     E
52     5     E
53     3     E
54     6     E
55     1     E
56     1     E
57     3     E
58     2     E
59     6     E
60     4     E
61    11     F
62     9     F
63    15     F
64    22     F
65    15     F
66    16     F
67    13     F
68    10     F
69    26     F
70    26     F
71    24     F
72    13     F
```
</details>

### Technique-1
```R
> X <- split(InsectSprays$count,InsectSprays$spray)

# using sapply()
> sapply(X, mean)
        A         B         C         D         E         F 
14.500000 15.333333  2.083333  4.916667  3.500000 16.666667

# using vapply()
> vapply(X, mean, numeric(1))
        A         B         C         D         E         F 
14.500000 15.333333  2.083333  4.916667  3.500000 16.666667 
```

### Technique-2
```R
> tapply(InsectSprays$count, InsectSprays$spray, mean)
        A         B         C         D         E         F 
14.500000 15.333333  2.083333  4.916667  3.500000 16.666667 
```

### Technique-3
```R
> by(InsectSprays$count, InsectSprays$spray, mean)
InsectSprays$spray: A
[1] 14.5
--------------------------------------------------------------------------- 
InsectSprays$spray: B
[1] 15.33333
--------------------------------------------------------------------------- 
InsectSprays$spray: C
[1] 2.083333
--------------------------------------------------------------------------- 
InsectSprays$spray: D
[1] 4.916667
--------------------------------------------------------------------------- 
InsectSprays$spray: E
[1] 3.5
--------------------------------------------------------------------------- 
InsectSprays$spray: F
[1] 16.66667
```

### Technique-4
```R
> aggregate(count ~ spray, InsectSprays, mean)
  spray     count
1     A 14.500000
2     B 15.333333
3     C  2.083333
4     D  4.916667
5     E  3.500000
6     F 16.666667
```

### Technique-5
```R
library(plyr)
> ddply(InsectSprays, .(spray), summarize, ave=mean(count))
  spray       ave
1     A 14.500000
2     B 15.333333
3     C  2.083333
4     D  4.916667
5     E  3.500000
6     F 16.666667
```

### Technique-6
```R
> library(dplyr)
> summarise(  group_by(InsectSprays, spray), ave=mean(count)  )
# A tibble: 6 x 2
  spray   ave
  <fct> <dbl>
1 A     14.5 
2 B     15.3 
3 C      2.08
4 D      4.92
5 E      3.5 
6 F     16.7 
```

### Technique-7
```R
library(plyr)
> dlply(InsectSprays, .(spray), summarise, mean.count = mean(count))
$A
  mean.count
1       14.5

$B
  mean.count
1   15.33333

$C
  mean.count
1   2.083333

$D
  mean.count
1   4.916667

$E
  mean.count
1        3.5

$F
  mean.count
1   16.66667

attr(,"split_type")
[1] "data.frame"
attr(,"split_labels")
  spray
1     A
2     B
3     C
4     D
5     E
6     F
```
