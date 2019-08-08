### Shows % of NA's per column(as column name) with column numbers in [ ]

```R
NA_Data <- function( DataSet ){
   colnems <- colnames(DataSet)
   NAS <- 100*colSums(is.na(DataSet))/(dim(DataSet)[1])
   # above outputs same as : NAS <- vapply(DataSet, function(x) 100*sum(is.na(x))/length(x), numeric(1))
   NA_list <- tapply(NAS, as.factor(NAS), function(x) paste0(names(x),"[",match(names(x),colnems),"]"), simplify=FALSE)
   # NA_list <- tapply(NAS, as.factor(NAS), names, simplify = FALSE)
   max_is <- max(vapply(NA_list, length, numeric(1)))
   # as.data.frame(sapply(    NA_list,    function(x)    c(   x,   rep("",max_is-length(x))   )    ))
   as.data.frame(vapply(NA_list, function(x) c(x, rep("",max_is-length(x))), character(max_is)))
}
```
<details>
  <summary><b>Details...</b>click to expand!!</summary>

```R
> NA_Data(airquality)
         0 4.57516339869281 24.1830065359477
1  Wind[3]       Solar.R[2]         Ozone[1]
2  Temp[4]                                  
3 Month[5]                                  
4   Day[6]                                  

> library(gapminder)
# Strange thing about gapminder is, a matrix is returned, not a vector which should have been returned.
# The matrix then gets converted to a data.frame
> NA_Data(gapminder)
             0
1   country[1]
2 continent[2]
3      year[3]
4   lifeExp[4]
5       pop[5]
6 gdpPercap[6]
# My analysis: sapply returns a vector only when returns from list components are of single length.
# If return lengths are > 1, a matrix is returned if all return lengths are same.
# A matrix is returned even if sapply operates only on a single component of a list, as is the case with gapminder.
# Same goes for diamonds dataset

> library(ggplot2)
> data(diamonds)
> NA_Data(diamonds)
            0
1    carat[1]
2      cut[2]
3    color[3]
4  clarity[4]
5    depth[5]
6    table[6]
7    price[7]
8        x[8]
9        y[9]
10      z[10]
```
</details>

#### Same task but uses data.table, may not be faster than above
WARNING..!! This converts your input to type **data.table**.
```R
NA_Data_2 <- function( Dataset ){
        library(data.table)
        setDT(Dataset)
        Dataset[, {
                colnems <- colnames(.SD)
                NAS <- vapply(.SD, function(x) 100*sum(is.na(x))/length(x), numeric(1))
                NA_list <- tapply(NAS, as.factor(NAS), function(x) paste0(names(x),"[",match(names(x),colnems),"]"), simplify=FALSE)
                max_is <- max(vapply(NA_list, length, numeric(1)))
                as.data.frame(vapply(NA_list, function(x) c(x, rep("",max_is-length(x))), character(max_is)))
        }]
}
```

---






### Bins a continuous variable into separate quantile groups
```R
cut(X, breaks=quantile(X, probs=seq(0, 1, 0.2)), include.lowest=TRUE, right=TRUE)
```

```R
> X <- seq(51, 100, by=0.1)
> qnts <- quantile(X, probs=seq(0, 1, 0.2))

> Y <- cut(X, breaks=qnts, include.lowest=TRUE, right=TRUE)
> table(Y)
Y
  [51,60.8] (60.8,70.6] (70.6,80.4] (80.4,90.2]  (90.2,100] 
         99          98          98          98          98 
```

---






### Turns a numeric vector to represent corresponding factor levels of a factor-level character vector
```R
New_data <- factor(activity_labels[long_vector], levels=activity_labels)
```
You have a character vector of activity labels and a long numeric vector representing those labels.
<details>
  <summary><b>open...</b></summary>

```R
> activity_labels
[1] "WALKING"            "WALKING_UPSTAIRS"   "WALKING_DOWNSTAIRS" "SITTING"           
[5] "STANDING"           "LAYING"            

> long_vector
   [1] 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6
  [76] 6 6 6 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
 [151] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 4 4 4 4 4 4 4 4 4 4 4 6 6 6 6 6 6 6 6 6 6 6 6
 [226] 4 4 4 4 4 4 4 4 4 4 4 4 6 6 6 6 6 6 6 6 6 6 6 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 3 3 3 3
 [301] 3 3 3 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5
 [376] 5 5 5 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
 [451] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 5 5 5
 [526] 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6
 [601] 6 6 6 6 6 6 6 6 6 6 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 2 2 2 2 2 2 2 2 2 2 2 2
 [676] 2 2 2 2 2 2 2 2 2 2 2 2 2 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 6 6 6 6 6 6 6 6 6 6 6
 [751] 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 2 2 2
 [826] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 6 6 6 6
 [901] 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 2 2 2 2 2 2 2
 [976] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 5 5 5 5 5 5 5 5 5 5
 [ reached getOption("max.print") -- omitted 6352 entries ]
```
</details>

You want to turn the numeric vector to a factor variable but you also want the numbers in it to represent the corresponding factor levels in the activity_labels. Simply writing **factor(activity_labels[long_vector])** will represent the same factor levels but change the underlying numbers. This is because it assigns factors based on alphabetical order.
<details>
  <summary><b>open...</b></summary>
   
```R
> factor(activity_labels[long_vector])
   [1] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
   [8] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
  [15] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
  [22] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           SITTING           
  [29] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
  [36] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
  [43] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
  [50] SITTING            SITTING            LAYING             LAYING             LAYING             LAYING             LAYING            
  [57] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
  [64] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
  [71] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
  [78] LAYING             WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
  [85] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
  [92] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
  [99] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [106] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [113] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [120] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING_DOWNSTAIRS
 [127] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [134] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [141] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [148] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [155] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [162] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [169] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [176] WALKING_UPSTAIRS   STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [183] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [190] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [197] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           SITTING           
 [204] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [211] SITTING            SITTING            SITTING            LAYING             LAYING             LAYING             LAYING            
 [218] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [225] LAYING             SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [232] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            LAYING            
 [239] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [246] LAYING             LAYING             LAYING             WALKING            WALKING            WALKING            WALKING           
 [253] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [260] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [267] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [274] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [281] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [288] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [295] WALKING            WALKING            WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [302] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [309] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [316] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [323] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [330] WALKING_UPSTAIRS   WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [337] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [344] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS STANDING           STANDING           STANDING          
 [351] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [358] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [365] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [372] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [379] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [386] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [393] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [400] SITTING            SITTING            SITTING            SITTING            SITTING            LAYING             LAYING            
 [407] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [414] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [421] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [428] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [435] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [442] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [449] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [456] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [463] WALKING            WALKING            WALKING            WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [470] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [477] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [484] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [491] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [498] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [505] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [512] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [519] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   STANDING           STANDING           STANDING          
 [526] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [533] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [540] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [547] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           SITTING           
 [554] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [561] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [568] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [575] SITTING            SITTING            SITTING            SITTING            LAYING             LAYING             LAYING            
 [582] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [589] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [596] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [603] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [610] LAYING             WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [617] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [624] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [631] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [638] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [645] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [652] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [659] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [666] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [673] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [680] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [687] WALKING_UPSTAIRS   WALKING_UPSTAIRS   STANDING           STANDING           STANDING           STANDING           STANDING          
 [694] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [701] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [708] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [715] STANDING           STANDING           SITTING            SITTING            SITTING            SITTING            SITTING           
 [722] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [729] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [736] SITTING            SITTING            SITTING            SITTING            LAYING             LAYING             LAYING            
 [743] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [750] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [757] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [764] LAYING             LAYING             LAYING             LAYING             WALKING            WALKING            WALKING           
 [771] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [778] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [785] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [792] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING_DOWNSTAIRS
 [799] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [806] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [813] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [820] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [827] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [834] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [841] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [848] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [855] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [862] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [869] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [876] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [883] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [890] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [897] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [904] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [911] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [918] LAYING             LAYING             LAYING             WALKING            WALKING            WALKING            WALKING           
 [925] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [932] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [939] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [946] WALKING            WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [953] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [960] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [967] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [974] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [981] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [988] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   STANDING           STANDING           STANDING           STANDING          
 [995] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [ reached getOption("max.print") -- omitted 6352 entries ]
Levels: LAYING SITTING STANDING WALKING WALKING_DOWNSTAIRS WALKING_UPSTAIRS
```
</details>

wwefwe
```R
Labels <- factor(activity_labels[test], levels=activity_labels)
> factor(activity_labels$V2[y_train$V1], levels=activity_labels$V2)
   [1] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
   [8] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
  [15] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
  [22] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           SITTING           
  [29] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
  [36] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
  [43] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
  [50] SITTING            SITTING            LAYING             LAYING             LAYING             LAYING             LAYING            
  [57] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
  [64] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
  [71] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
  [78] LAYING             WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
  [85] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
  [92] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
  [99] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [106] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [113] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [120] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING_DOWNSTAIRS
 [127] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [134] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [141] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [148] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [155] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [162] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [169] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [176] WALKING_UPSTAIRS   STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [183] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [190] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [197] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           SITTING           
 [204] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [211] SITTING            SITTING            SITTING            LAYING             LAYING             LAYING             LAYING            
 [218] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [225] LAYING             SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [232] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            LAYING            
 [239] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [246] LAYING             LAYING             LAYING             WALKING            WALKING            WALKING            WALKING           
 [253] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [260] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [267] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [274] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [281] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [288] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [295] WALKING            WALKING            WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [302] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [309] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [316] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [323] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [330] WALKING_UPSTAIRS   WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [337] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [344] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS STANDING           STANDING           STANDING          
 [351] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [358] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [365] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [372] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [379] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [386] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [393] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [400] SITTING            SITTING            SITTING            SITTING            SITTING            LAYING             LAYING            
 [407] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [414] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [421] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [428] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [435] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [442] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [449] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [456] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [463] WALKING            WALKING            WALKING            WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [470] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [477] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [484] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [491] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [498] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [505] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [512] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [519] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   STANDING           STANDING           STANDING          
 [526] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [533] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [540] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [547] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           SITTING           
 [554] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [561] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [568] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [575] SITTING            SITTING            SITTING            SITTING            LAYING             LAYING             LAYING            
 [582] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [589] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [596] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [603] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [610] LAYING             WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [617] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [624] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [631] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [638] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [645] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [652] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [659] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [666] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [673] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [680] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [687] WALKING_UPSTAIRS   WALKING_UPSTAIRS   STANDING           STANDING           STANDING           STANDING           STANDING          
 [694] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [701] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [708] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [715] STANDING           STANDING           SITTING            SITTING            SITTING            SITTING            SITTING           
 [722] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [729] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [736] SITTING            SITTING            SITTING            SITTING            LAYING             LAYING             LAYING            
 [743] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [750] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [757] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [764] LAYING             LAYING             LAYING             LAYING             WALKING            WALKING            WALKING           
 [771] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [778] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [785] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [792] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING_DOWNSTAIRS
 [799] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [806] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [813] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [820] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [827] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [834] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [841] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [848] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [855] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [862] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [869] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [876] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [883] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [890] SITTING            SITTING            SITTING            SITTING            SITTING            SITTING            SITTING           
 [897] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [904] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [911] LAYING             LAYING             LAYING             LAYING             LAYING             LAYING             LAYING            
 [918] LAYING             LAYING             LAYING             WALKING            WALKING            WALKING            WALKING           
 [925] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [932] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [939] WALKING            WALKING            WALKING            WALKING            WALKING            WALKING            WALKING           
 [946] WALKING            WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [953] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [960] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS
 [967] WALKING_DOWNSTAIRS WALKING_DOWNSTAIRS WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [974] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [981] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS  
 [988] WALKING_UPSTAIRS   WALKING_UPSTAIRS   WALKING_UPSTAIRS   STANDING           STANDING           STANDING           STANDING          
 [995] STANDING           STANDING           STANDING           STANDING           STANDING           STANDING          
 [ reached getOption("max.print") -- omitted 6352 entries ]
Levels: WALKING WALKING_UPSTAIRS WALKING_DOWNSTAIRS SITTING STANDING LAYING
```
