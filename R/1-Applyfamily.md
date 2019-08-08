https://help.github.com/en/articles/basic-writing-and-formatting-syntax
# The functions of the apply family

### 1. apply(X, MARGIN, FUN, ...)
>- It is most often used to apply a function to the rows or columns of a matrix.
>- It can be used with general arrays, e.g. taking the average of an array of matrices.
>- It is not really faster than writing a loop, but it works in one line!

The apply() can be used to immitate the functions below:<br/>
- rowSums() = apply(x, 1, sum)<br/>
- rowMeans() = apply(x, 1, mean)<br/>
- colSums() = apply(x, 2, sum)<br/>
- colMeans() = apply(x, 2, mean)

But the shortcut functions, due to having been designed for specific purpose, are faster than their apply() counterparts, the difference being apparent only when used on a large matrix.

http://www.rdocumentation.org/packages/base/versions/3.5.3/topics/apply<br/>
https://stat.ethz.ch/R-manual/R-devel/library/base/html/apply.html
<details>
  <summary><b>Details...</b>click to expand!!</summary>

|Argument|Description|
|---|---|
|X|an array, including a matrix.|
|MARGIN|a vector giving the subscripts which the function will be applied over. E.g., for a matrix 1 indicates rows, 2 indicates columns, c(1, 2) indicates rows and columns. Where X has named dimnames, it can be a character vector selecting dimension names.|
|FUN|the function to be applied: see ‘Details’. In the case of functions like +, %*%, etc., the function name must be backquoted or quoted.|
|...|optional arguments to FUN.|
```R
> set.seed(18949)
> x <- matrix(rnorm(200),20,10)
> apply(x, 1, quantile, probs=c(0.25, 0.75))
          [,1]       [,2]       [,3]      [,4]
25% -0.9914692 -0.1908423 -0.4300005 -1.255110
75%  0.7904808  0.5512175  0.5697090  0.244837
          [,5]       [,6]       [,7]       [,8]
25% -0.5256669 -0.4636574 -1.3609265 -0.7012947
75%  0.4640788  0.3641334  0.1896196  0.8205340
          [,9]      [,10]      [,11]      [,12]
25% -0.8150329 -0.344708  -0.5216225  0.2128561
75%  0.7361397  0.813752   0.6109588  0.6443443
         [,13]      [,14]      [,15]      [,16]
25% -0.77378447 0.2209238 -1.1365713 -0.7600171
75%  0.08931413 0.7466569 -0.1099666  0.5997105
         [,17]      [,18]      [,19]      [,20]
25% -0.1963744 -0.4609184 -1.1621672 -0.7849068
75%  1.0905260  1.0427894 -0.3418321  0.3916902
################################################################################################

> set.seed(18949)
> a <- array(rnorm(2*5*3), c(2,5,3))
> a
, , 1

           [,1]       [,2]        [,3]       [,4]       [,5]
[1,] -2.0702028 -1.4553385  0.40501198  0.2917507 -1.1514895
[2,]  0.6493751 -0.1889221 -0.04674509 -1.5768760  0.1921096

, , 2

           [,1]       [,2]       [,3]       [,4]      [,5]
[1,] -0.3167998 -0.4482162 -1.1936750  0.5656030 0.1047564
[2,]  0.1792631  0.8629761  0.7418777 -0.4702795 0.3604304

, , 3

           [,1]       [,2]       [,3]       [,4]      [,5]
[1,]  0.7002806 -0.2662845 -0.3091816 -0.2429141 0.7591960
[2,] -0.6699911 -1.0847956 -0.4682024  0.7537504 0.4481469
################################################################################################

# c(1,2) preserves the 1st and 2nd dimensions and collapses the 3rd dimension
> apply(a, c(1,2), mean)
            [,1]       [,2]        [,3]       [,4]        [,5]
[1,] -0.56224065 -0.7232797 -0.36594820  0.2048132 -0.09584571
[2,]  0.05288237 -0.1369139  0.07564341 -0.4311350  0.33356231

> rowMeans(a, dims=2)
            [,1]       [,2]        [,3]       [,4]        [,5]
[1,] -0.56224065 -0.7232797 -0.36594820  0.2048132 -0.09584571
[2,]  0.05288237 -0.1369139  0.07564341 -0.4311350  0.33356231
```
</details>

---






### 2. lapply(X, FUN, ...)
>- returns a **"list"** from input list **"X"**.
>- each element of output list is the result of applying FUN to the corresponding element of **"X"**.
>- input data-frames (or any other R objects) are converted to lists first before any processing.

https://www.rdocumentation.org/packages/base/versions/3.5.3/topics/lapply<br/>
https://stat.ethz.ch/R-manual/R-patched/library/base/html/lapply.html
<details>
  <summary><b>Details...</b>click to expand!!</summary>

|Argument|Description|
|---|---|
|X|a vector (atomic or list) or an expression object. Other objects (including classed objects) will be coerced by base::as.list.|
|FUN|the function to be applied to each element of X: see ‘Details’. In the case of functions like +, %*%, the function name must be backquoted or quoted.|
|...|optional arguments to FUN.|
```R
> x <- 1:4
> lapply(x, runif)
[[1]]
[1] 0.2511177

[[2]]
[1] 0.2150691 0.6094760

[[3]]
[1] 0.3834446 0.7552710 0.3797362

[[4]]
[1] 0.7949721 0.9056911 0.9840262 0.5879480
################################################################################################

> lapply(x, runif, min=100, max=200)
[[1]]
[1] 100.9464

[[2]]
[1] 132.0792 155.9457

[[3]]
[1] 151.4918 108.9712 168.3251

[[4]]
[1] 170.9972 180.0232 194.4078 118.6646
################################################################################################

> # Anonymous function
> x <- list(a=matrix(1:4,2,2), b=matrix(1:6,3,2))
> x
$a
     [,1] [,2]
[1,]    1    3
[2,]    2    4

$b
     [,1] [,2]
[1,]    1    4
[2,]    2    5
[3,]    3    6

> # Select the first column
> lapply(x, function(elt) elt[,1])
$a
[1] 1 2

$b
[1] 1 2 3
################################################################################################

> # Select the 1st row
> lapply(x, "[", 1,)
$a
[1] 1 3

$b
[1] 1 4
################################################################################################

> # Select the 2nd column
> lapply(x, "[", ,2)
$a
[1] 3 4

$b
[1] 4 5 6
################################################################################################

# Advanced examples
> x <- list(a = 1:10, beta = exp(-3:3), logic = c(TRUE,FALSE,FALSE,TRUE))
> lapply(x, runif)
$a
 [1] 0.32122467 0.06019516 0.04345645 0.05505382 0.62554280 0.96447029 0.82730287 0.31502824 0.21302545 0.73249612

$beta
 [1] 0.49924102 0.72977197 0.08033604 0.43553048 0.23658045 0.79156780 0.25868432

$logic
 [1] 0.9859838 0.7568737 0.9797782 0.2189478
################################################################################################

> # compute the list mean for each list element
> lapply(x, mean)
$a
[1] 5.5

$beta
[1] 4.535125

$logic
[1] 0.5
################################################################################################

> # median and quartiles for each list element
> lapply(x, quantile, probs = 1:3/4)
$a
 25%  50%  75% 
3.25 5.50 7.75 

$beta
      25%       50%       75% 
0.2516074 1.0000000 5.0536690 

$logic
25% 50% 75% 
0.0 0.5 1.0
```
</details>

---






### 3. sapply(X, FUN, ..., simplify = TRUE, USE.NAMES = TRUE)
>- If the result is a list where every element is length 1, then a **"vector"** is returned.
>- If the result is a list where every element is a vector of same length (>1), then a **"matrix"** is returned.
>- If it can't figure things out, then a **"list"** is returned.

https://www.rdocumentation.org/packages/memisc/versions/0.99.17.1/topics/Sapply
<details>
  <summary><b>Details...</b>click to expand!!</summary>

|Argument|Description|
|---|---|
|X|a vector (atomic or list) or an expression object. Other objects (including classed objects) will be coerced by base::as.list.|
|FUN|the function to be applied to each element of X: see ‘Details’. In the case of functions like +, %*%, the function name must be backquoted or quoted.|
|...|optional arguments to FUN.|
|simplify|logical or character string; should the result be simplified to a vector, matrix or higher dimensional array if possible? For sapply it must be named and not abbreviated. The default value, TRUE, returns a vector or matrix if appropriate, whereas if simplify = "array" the result may be an array of “rank” (=length(dim(.))) one higher than the result of FUN(X[[i]]).|
|USE.NAMES|logical; if TRUE and if X is character, use X as names for the result unless it had names already. Since this argument follows ... its name cannot be abbreviated.|
```R
> x <- list(a = 1:10, beta = exp(-3:3), logic = c(TRUE,FALSE,FALSE,TRUE))
> sapply(x, mean)
       a     beta    logic 
5.500000 4.535125 0.500000
# returned a "vector"

> sapply(x, quantile)
         a        beta logic
0%    1.00  0.04978707   0.0
25%   3.25  0.25160736   0.0
50%   5.50  1.00000000   0.5
75%   7.75  5.05366896   1.0
100% 10.00 20.08553692   1.0
# returned a "matrix"

> i39 <- sapply(3:9, seq) # list of vectors
> sapply(i39, fivenum)
     [,1] [,2] [,3] [,4] [,5] [,6] [,7]
[1,]  1.0  1.0    1  1.0  1.0  1.0    1
[2,]  1.5  1.5    2  2.0  2.5  2.5    3
[3,]  2.0  2.5    3  3.5  4.0  4.5    5
[4,]  2.5  3.5    4  5.0  5.5  6.5    7
[5,]  3.0  4.0    5  6.0  7.0  8.0    9
```
</details>

---






### 4. mapply(FUN, ..., MoreArgs = NULL, SIMPLIFY = TRUE, USE.NAMES = TRUE)
>- It is a sort of a multi-variate version of apply that applies a function in parallel over a set of arguments.
>- It can produce output with vector arguments from functions that do not support vector arguments, hence eliminating for loops.

https://www.rdocumentation.org/packages/base/versions/3.5.3/topics/mapply<br/>
https://stat.ethz.ch/R-manual/R-devel/library/base/html/mapply.html
<details>
  <summary><b>Details...</b>click to expand!!</summary>

|Argument|Description|
|---|---|
|FUN|function to apply, found via match.fun.|
|...|arguments to vectorize over (vectors or lists of strictly positive length, or all of zero length). See also ‘Details’.|
|MoreArgs|a list of other arguments to FUN.|
|SIMPLIFY|logical or character string; attempt to reduce the result to a vector, matrix or higher dimensional array; see the simplify argument of sapply.|
|USE.NAMES|logical; use names if the first ... argument has names, or if it is a character vector, use that character vector as the names.|
```R
> mapply(rep, 1:4, 4:1)
[[1]]
[1] 1 1 1 1

[[2]]
[1] 2 2 2

[[3]]
[1] 3 3

[[4]]
[1] 4
################################################################################################

> mapply(rep, times = 1:4, x = 4:1)
[[1]]
[1] 4

[[2]]
[1] 3 3

[[3]]
[1] 2 2 2

[[4]]
[1] 1 1 1 1
################################################################################################

> mapply(rep, times = 1:4, MoreArgs = list(x = 42))
[[1]]
[1] 42

[[2]]
[1] 42 42

[[3]]
[1] 42 42 42

[[4]]
[1] 42 42 42 42
################################################################################################

> set.seed(18949)
> list(rnorm(1,1,2), rnorm(2,2,2), rnorm(3,3,2), rnorm(4,4,2), rnorm(5,5,2))
[[1]]
[1] -3.140406

[[2]]
[1]  3.2987502 -0.9106769

[[3]]
[1] 2.622156 3.810024 2.906510

[[4]]
[1] 4.5835013 0.8462481 1.6970210 4.3842191

[[5]]
[1] 4.366400 5.358526 4.103568 6.725952 2.612650

# Above same thing can be done with mapply() below
> set.seed(18949)
> mapply(rnorm, 1:5, 1:5, 2)
[[1]]
[1] -3.140406

[[2]]
[1]  3.2987502 -0.9106769

[[3]]
[1] 2.622156 3.810024 2.906510

[[4]]
[1] 4.5835013 0.8462481 1.6970210 4.3842191

[[5]]
[1] 4.366400 5.358526 4.103568 6.725952 2.612650
################################################################################################

> mapply(function(x, y) seq_len(x) + y,
        c(a =  1, b = 2, c = 3),  # names from first
        c(A = 10, B = 0, C = -10))
$a
[1] 11

$b
[1] 1 2

$c
[1] -9 -8 -7

> word <- function(C, k) paste(rep.int(C, k), collapse = "")
> utils::str(mapply(word, LETTERS[1:6], 6:1, SIMPLIFY = FALSE))
List of 6
 $ A: chr "AAAAAA"
 $ B: chr "BBBBB"
 $ C: chr "CCCC"
 $ D: chr "DDD"
 $ E: chr "EE"
 $ F: chr "F"
```
</details>

---







### 5. tapply(X, INDEX, FUN = NULL, ..., default = NA, simplify = TRUE)
>- It is used to apply a function over subsets of a vector.
>- It however needs a vector of factor levels to do this. However it can also be a simple numeric vector.
>- Since the factor variable is a vector, tapply() can work only on **"vectors"**. It cannot work on **"lists"** or anything else.

https://www.rdocumentation.org/packages/base/versions/3.5.3/topics/tapply<br/>
https://stat.ethz.ch/R-manual/R-devel/library/base/html/tapply.html
<details>
  <summary><b>Details...</b>click to expand!!</summary>

|Argument|Description|
|---|---|
|X|an atomic object, typically a vector.|
|INDEX|a list of one or more factors, each of same length as X. The elements are coerced to factors by as.factor.|
|FUN|the function to be applied, or NULL. In the case of functions like +, %*%, etc., the function name must be backquoted or quoted. If FUN is NULL, tapply returns a vector which can be used to subscript the multi-way array tapply normally produces.|
|...|optional arguments to FUN. Note: Optional arguments to FUN supplied by the ... argument are not divided into cells. It is therefore inappropriate for FUN to expect additional arguments with the same length as X.|
|default|(only in the case of simplification to an array) the value with which the array is initialized as array(default, dim = ..). Before R 3.4.0, this was hard coded to array()'s default NA. If it is NA (the default), the missing value of the answer type, e.g. NA_real_, is chosen (as.raw(0) for "raw"). In a numerical case, it may be set, e.g., to FUN(integer(0)), e.g., in the case of FUN = sum to 0 or 0L.|
|simplify|logical; if FALSE, tapply always returns an array of mode "list"; in other words, a list with a dim attribute. If TRUE (the default), then if FUN always returns a scalar, tapply returns an array with the mode of the scalar.|
```R
> set.seed(14657)
> x <- c(rnorm(10), runif(10), rnorm(10,1))
> x
 [1] -1.2892994  1.6734810 -0.3615852  0.1937537 -1.6371557
 [6]  0.2596941 -0.7552847  0.4357292  0.1230147 -1.5369686
[11]  0.7955234  0.4814413  0.3173244  0.5735813  0.3353314
[16]  0.3980635  0.5082634  0.3929499  0.4615895  0.2688306
[21]  1.9067960  1.4941630  2.0046694  2.7300762  1.3405568
[26]  1.8557122  1.2624954  1.2541206  0.6466761  3.0822887

> f <- gl(3, 10)
> f
 [1] 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 3 3 3 3 3 3 3 3 3 3
Levels: 1 2 3

> tapply(x, f, mean)
         1          2          3 
-0.2894621  0.4532899  1.7577554
################################################################################################

# This gives a list
> tapply(x, f, range)
$`1`
[1] -1.637156  1.673481

$`2`
[1] 0.2688306 0.7955234

$`3`
[1] 0.6466761 3.0822887

# This also gives a list
> tapply(x, f, quantile, probs = 1:3/4)
$`1`
       25%        50%        75% 
-1.1557957 -0.1192853  0.2432090 

$`2`
      25%       50%       75% 
0.3497361 0.4298265 0.5015579 

$`3`
     25%      50%      75% 
1.282011 1.674938 1.980201 


> tapply(x, f, summary)
$`1`
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
-1.6372 -1.1558 -0.1193 -0.2895  0.2432  1.6735 

$`2`
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
 0.2688  0.3497  0.4298  0.4533  0.5016  0.7955 

$`3`
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
 0.6467  1.2820  1.6749  1.7578  1.9802  3.0823
################################################################################################

# My function to generate factor levels of differing frequencies
generate_factor_levels <- function(n, t){
        # 'n' take a numeric vector of required factor levels
        # 't' takes a numeric vecor of corresponding frequencies for each factor level in 'n'
        # Both 'n' and 't' must be of same length
        if(length(n) == length(t)){
                factor(unlist(mapply(rep, n, t)))
        }
        else{
                stop("'n' and 't' must be of same length")
        }
}

> a <- c(4,0,1,5)
> b <- c(2,9,3,2)

> generate_factor_levels(a,b)
 [1] 4 4 0 0 0 0 0 0 0 0 0 1 1 1 5 5
Levels: 0 1 4 5
################################################################################################

# Below code calculates summary statistics of 4 groups being 1-40, 41-70, 71-90 and 91-100
> x <- 1:100
> x
  [1]   1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16  17  18  19  20  21  22  23  24  25
 [26]  26  27  28  29  30  31  32  33  34  35  36  37  38  39  40  41  42  43  44  45  46  47  48  49  50
 [51]  51  52  53  54  55  56  57  58  59  60  61  62  63  64  65  66  67  68  69  70  71  72  73  74  75
 [76]  76  77  78  79  80  81  82  83  84  85  86  87  88  89  90  91  92  93  94  95  96  97  98  99 100

> a <- 1:4
> b <- c(40,30,20,10)
> f <- generate_factor_levels(a,b)
> f
  [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2
 [51] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 4 4 4 4 4 4 4 4 4 4
Levels: 1 2 3 4

> tapply(x, f, summary)
$`1`
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
   1.00   10.75   20.50   20.50   30.25   40.00 

$`2`
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  41.00   48.25   55.50   55.50   62.75   70.00 

$`3`
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  71.00   75.75   80.50   80.50   85.25   90.00 

$`4`
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
  91.00   93.25   95.50   95.50   97.75  100.00
```
</details>

---






### 6. split(x, f, drop = FALSE, ...)
### S3 method (default): split(x, f, drop = FALSE, sep = ".", lex.order = FALSE, ...)
>- split() divides the dataframe or vector "**x**" into the groups defined by f; f is a "**list**" of factor levels.
>- in case of dataframe or vector input, it will be a list of multiple dataframes or vectors respectively.
>- If you specify "**f**" as a list, then "**interaction()**" is called internally to combine the factor levels.

https://www.rdocumentation.org/packages/base/versions/3.5.3/topics/split<br/>
https://stat.ethz.ch/R-manual/R-patched/library/base/html/split.html
<details>
  <summary><b>Details...</b>click to expand!!</summary>

|Argument|Description|
|---|---|
|x|vector or data frame containing values to be divided into groups.|
|f|a ‘factor’ in the sense that as.factor(f) defines the grouping, or a list of such factors in which case their interaction is used for the grouping.|
|drop|logical indicating if levels that do not occur should be dropped (if f is a factor or a list).|
|value|a list of vectors or data frames compatible with a splitting of x. Recycling applies if the lengths do not match.|
|sep|character string, passed to interaction in the case where f is a list.|
|lex.order|logical, passed to interaction when f is a list.|
|...|further potential arguments passed to methods.|
```R
> x <- c(rnorm(10), runif(10), rnorm(10,1))
> f <- gl(3,10)
> split(x,f)
$`1`
 [1]  1.1569045  0.7179864  1.1045040 -0.1905366  0.1516832  0.6769356 -0.2131922  0.6491381  0.4399765
[10]  0.8860714

$`2`
 [1] 0.8927252 0.8139218 0.0700290 0.9323876 0.4533011 0.8681981 0.8158227 0.8526874 0.3309505 0.9017579

$`3`
 [1]  3.93247213  0.48364955  0.13147744  1.22116363 -0.02540756  0.99815180 -1.02639942  1.57687136
 [9] -0.26233866 -0.07133822
################################################################################################

> lapply(split(x,f), mean)# This way is rarely used. tapply or sapply are preferred.
$`1`
[1] 0.5379471

$`2`
[1] 0.6931781

$`3`
[1] 0.6958302

> tapply(x, f, mean)
        1         2         3 
0.5379471 0.6931781 0.6958302 

> sapply(split(x,f), mean)
        1         2         3 
0.5379471 0.6931781 0.6958302 
################################################################################################

# This example demonstrates the combined power of sapply() with anonymous function.
> head(airquality)
  Ozone Solar.R Wind Temp Month Day
1    41     190  7.4   67     5   1
2    36     118  8.0   72     5   2
3    12     149 12.6   74     5   3
4    18     313 11.5   62     5   4
5    NA      NA 14.3   56     5   5
6    28      NA 14.9   66     5   6

> s <- split(airquality, airquality$Month)
> lapply(  s, function(x) colMeans(x[,c("Ozone","Solar.R","Wind")])  )
$`5`
   Ozone  Solar.R     Wind 
      NA       NA 11.62258 

$`6`
    Ozone   Solar.R      Wind 
       NA 190.16667  10.26667 

$`7`
     Ozone    Solar.R       Wind 
        NA 216.483871   8.941935 

$`8`
   Ozone  Solar.R     Wind 
      NA       NA 8.793548 

$`9`
   Ozone  Solar.R     Wind 
      NA 167.4333  10.1800 

# sapply() will give a more compact form
> sapply(  s, function(x) colMeans(x[,c("Ozone","Solar.R","Wind")])  )
               5         6          7        8        9
Ozone         NA        NA         NA       NA       NA
Solar.R       NA 190.16667 216.483871       NA 167.4333
Wind    11.62258  10.26667   8.941935 8.793548  10.1800

# If you want to remove NAs...
> sapply(  s, function(x) colMeans(x[,c("Ozone","Solar.R","Wind")],na.rm=TRUE)  )
                5         6          7          8         9
Ozone    23.61538  29.44444  59.115385  59.961538  31.44828
Solar.R 181.29630 190.16667 216.483871 171.857143 167.43333
Wind     11.62258  10.26667   8.941935   8.793548  10.18000
################################################################################################

> set.seed(14657)

> # You can combine factor levels with interaction() or list()
> x <- rnorm(10)
> f1 <- gl(2,5)
> f2 <- gl(5,2)
> interaction(f1,f2)
 [1] 1.1 1.1 1.2 1.2 1.3 2.3 2.4 2.4 2.5 2.5
Levels: 1.1 2.1 1.2 2.2 1.3 2.3 1.4 2.4 1.5 2.5

> split(x, interaction(f1,f2))
$`1.1`
[1] -1.289299  1.673481

$`2.1`
numeric(0)

$`1.2`
[1] -0.3615852  0.1937537

$`2.2`
numeric(0)

$`1.3`
[1] -1.637156

$`2.3`
[1] 0.2596941

$`1.4`
numeric(0)

$`2.4`
[1] -0.7552847  0.4357292

$`1.5`
numeric(0)

$`2.5`
[1]  0.1230147 -1.5369686


> # A little more neater output
> str(split(x, interaction(f1,f2)))
List of 10
 $ 1.1: num [1:2] -1.29 1.67
 $ 2.1: num(0) 
 $ 1.2: num [1:2] -0.362 0.194
 $ 2.2: num(0) 
 $ 1.3: num -1.64
 $ 2.3: num 0.26
 $ 1.4: num(0) 
 $ 2.4: num [1:2] -0.755 0.436
 $ 1.5: num(0) 
 $ 2.5: num [1:2] 0.123 -1.537

> # same thing can be achieved with list() as R automatically calls interaction() internally
> str(split(x, list(f1,f2)))
List of 10
 $ 1.1: num [1:2] -1.29 1.67
 $ 2.1: num(0) 
 $ 1.2: num [1:2] -0.362 0.194
 $ 2.2: num(0) 
 $ 1.3: num -1.64
 $ 2.3: num 0.26
 $ 1.4: num(0) 
 $ 2.4: num [1:2] -0.755 0.436
 $ 1.5: num(0) 
 $ 2.5: num [1:2] 0.123 -1.537

> # You can drop the empty elements
> str(split(x, list(f1,f2), drop=TRUE))
List of 6
 $ 1.1: num [1:2] -1.29 1.67
 $ 1.2: num [1:2] -0.362 0.194
 $ 1.3: num -1.64
 $ 2.3: num 0.26
 $ 2.4: num [1:2] -0.755 0.436
 $ 2.5: num [1:2] 0.123 -1.537
```
</details>

---






### 7. vapply(X, FUN, FUN.VALUE, ..., USE.NAMES = TRUE)
>- It is similar to sapply, but has a pre-specified type of return value, so it can be safer (and sometimes faster) to use.
<details>
  <summary><b>Details...</b>click to expand!!</summary>

|Argument|Description|
|---|---|
|X|a vector (atomic or list) or an expression object. Other objects (including classed objects) will be coerced by base::as.list.|
|FUN|the function to be applied to each element of X: see ‘Details’. In the case of functions like +, %*%, the function name must be backquoted or quoted.|
|FUN.VALUE|a (generalized) vector; a template for the return value from FUN. See ‘Details’.|
|...|optional arguments to FUN.|
|USE.NAMES|logical; if TRUE and if X is character, use X as names for the result unless it had names already. Since this argument follows ... its name cannot be abbreviated.|
```R
> count_by_spray <- with(InsectSprays, split(count, spray))
> vapply(count_by_spray, mean, numeric(1))
        A         B         C         D         E         F 
14.500000 15.333333  2.083333  4.916667  3.500000 16.666667 
```
</details>

---






### 8. by(data, INDICES, FUN, ..., simplify = TRUE)
>- Function by is an object-oriented wrapper for tapply applied to data frames.
<details>
  <summary><b>Details...</b>click to expand!!</summary>

|Argument|Description|
|---|---|
|data|an R object, normally a data frame, possibly a matrix.|
|INDICES|a factor or a list of factors, each of length nrow(data).|
|FUN|a function to be applied to (usually data-frame) subsets of data.|
|...|further arguments to FUN.|
|simplify|logical: see tapply.|
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
</details>

---






### 9. do.call(what, args, quote = FALSE, envir = parent.frame())
>- This function allows you to call any R function, but instead of writing out the arguments one by one, you can use a list to hold the arguments of the function.
>- <a href="https://github.com/Tanmoy-Rath/misc/blob/master/do.call().md">Read more . . .</a>
<details>
  <summary><b>Details...</b>click to expand!!</summary>

|Argument|Description|
|---|---|
|what|either a function or a non-empty character string naming the function to be called.|
|args|a list of arguments to the function call. The names attribute of args gives the argument names.|
|quote|a logical value indicating whether to quote the arguments.|
|envir|an environment within which to evaluate the call. This will be most useful if what is a character string and the arguments are symbols or quoted expressions.|
```R
> spl <- split(airquality, airquality$Month)
> do.call(
    rbind,
    lapply(  spl,  function(x)  colMeans( x[,c("Ozone","Solar.R","Wind","Temp")], na.rm=TRUE)  )
)
     Ozone  Solar.R      Wind     Temp
5 23.61538 181.2963 11.622581 65.54839
6 29.44444 190.1667 10.266667 79.10000
7 59.11538 216.4839  8.941935 83.90323
8 59.96154 171.8571  8.793548 83.96774
9 31.44828 167.4333 10.180000 76.90000
```

</details>

---






### 9. aggregate()
>- Splits the data into subsets, computes summary statistics for each, and returns the result in a convenient form.
- <b>aggregate(x, ...)</b> ; Default S3 method
- <b>aggregate(x, by, FUN, ..., simplify = TRUE, drop = TRUE)</b> ; S3 method for class 'data.frame'
- <b>aggregate(formula, data, FUN, ..., subset, na.action = na.omit)</b> ; S3 method for class 'formula'
- <b>aggregate(x, nfrequency = 1, FUN = sum, ndeltat = 1, ts.eps = getOption("ts.eps"), ...)</b> ; S3 method for class 'ts'
<details>
  <summary><b>Details...</b>click to expand!!</summary>

|Argument|Description|
|---|---|
|x|an R object.|
|by|a list of grouping elements, each as long as the variables in the data frame x. The elements are coerced to factors before use.|
|FUN|a function to compute the summary statistics which can be applied to all data subsets.|
|simplify|a logical indicating whether results should be simplified to a vector or matrix if possible.|
|drop|a logical indicating whether to drop unused combinations of grouping values. The non-default case drop=FALSE has been available since R 3.3.0, and may change in some cases where unused combinations are still dropped.|
|formula|a formula, such as y ~ x or cbind(y1, y2) ~ x1 + x2, where the y variables are numeric data to be split into groups according to the grouping x variables (usually factors).|
|data|a data frame (or list) from which the variables in formula should be taken.|
|subset|an optional vector specifying a subset of observations to be used.|
|na.action|a function which indicates what should happen when the data contain NA values. The default is to ignore missing values in the given variables.|
|nfrequency|new number of observations per unit of time; must be a divisor of the frequency of x.|
|ndeltat|new fraction of the sampling period between successive observations; must be a divisor of the sampling interval of x.|
|ts.eps|tolerance used to decide if nfrequency is a sub-multiple of the original frequency.|
|...|further arguments passed to or used by methods.|
</details>
