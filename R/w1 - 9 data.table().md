https://cran.r-project.org/web/packages/data.table/vignettes/datatable-intro.html
```R

# data.table() , is written in C, hence is much much faster subsetting, grouping, updating

> set.seed(12345)

> library(data.table)
data.table 1.12.2 using 2 threads (see ?getDTthreads).  Latest news: r-datatable.com


> DF <- data.frame(x=rnorm(9), y=rep(c("a","b","c"),each=3), z=rnorm(9)  )
> head(DF,3)
           x y          z
1  0.5855288 a -0.9193220
2  0.7094660 a -0.1162478
3 -0.1093033 a  1.8173120


> set.seed(12345)
> DT <- data.table(x=rnorm(9), y=rep(c("a","b","c"),each=3), z=rnorm(9)  )
> head(DT,3)
            x y          z
1:  0.5855288 a -0.9193220
2:  0.7094660 a -0.1162478
3: -0.1093033 a  1.8173120


> data.table::tables()
   NAME NROW NCOL MB  COLS KEY
1:   DT    9    3  0 x,y,z    
Total: 0MB


# subset by rows and column values
> DT[2,]
          x y          z
1: 0.709466 a -0.1162478
> DT[DT$y=="a"]
            x y          z
1:  0.5855288 a -0.9193220
2:  0.7094660 a -0.1162478
3: -0.1093033 a  1.8173120


# subsetting by single index
> DT[c(2,5)]# subset by rows
           x y          z
1: 0.7094660 a -0.1162478
2: 0.6058875 b  0.5202165
> DT[,c(1,3)]# subset by columns
            x          z
1:  0.5855288 -0.9193220
2:  0.7094660 -0.1162478
3: -0.1093033  1.8173120
4: -0.4534972  0.3706279
5:  0.6058875  0.5202165
6: -1.8179560 -0.7505320
7:  0.6300986  0.8168998
8: -0.2761841 -0.8863575
9: -0.2841597 -0.3315776


# pass a list of functions, they operate on columns
> DT[,list(mean(x),sum(z))]
            V1        V2
1: -0.04556883 0.5210193
> DT[table(y)]# single functions donot require comma, no need for DT[,table(y)]
            x y        z
1: -0.1093033 a 1.817312
2: -0.1093033 a 1.817312
3: -0.1093033 a 1.817312


> DT[,w:=z^2]# add a new column, very memory efficient, very fast
> DT
            x y          z          w
1:  0.5855288 a -0.9193220 0.84515294
2:  0.7094660 a -0.1162478 0.01351355
3: -0.1093033 a  1.8173120 3.30262306
4: -0.4534972 b  0.3706279 0.13736501
5:  0.6058875 b  0.5202165 0.27062516
6: -1.8179560 b -0.7505320 0.56329827
7:  0.6300986 c  0.8168998 0.66732535
8: -0.2761841 c -0.8863575 0.78562966
9: -0.2841597 c -0.3315776 0.10994370


> DT[9,] <- list(1,"xyz",3,-6)# works
> DT[10:=list(1,"xyz",0,-4),]# doesn't work
Error: Check that is.data.table(DT) == TRUE. Otherwise, := and `:=`(...) are defined for use in j, once only and in particular ways. See help(":=").


# if you create another variable, it still references the first object
> DT2 <- DT
> DT[,m:={tmp <- (x+z); log2(tmp+5)}]# you can write multiple statements
> DT[,a:=x>0]# plyr like operations
> DT2# same changes will be reflected in the new reference
            x   y          z           w        m     a
1:  0.5855288   a -0.9193220  0.84515294 2.222250  TRUE
2:  0.7094660   a -0.1162478  0.01351355 2.483679  TRUE
3: -0.1093033   a  1.8173120  3.30262306 2.745885 FALSE
4: -0.4534972   b  0.3706279  0.13736501 2.297817 FALSE
5:  0.6058875   b  0.5202165  0.27062516 2.614970  TRUE
6: -1.8179560   b -0.7505320  0.56329827 1.281854 FALSE
7:  0.6300986   c  0.8168998  0.66732535 2.688628  TRUE
8: -0.2761841   c -0.8863575  0.78562966 1.940151 FALSE
9:  1.0000000 xyz  3.0000000 -6.00000000 3.169925  TRUE
# if you need a copy of the original, use the as.data.table() function
# setDT() converts a data.frame or lists to data.table by reference, no copy is created


# takes the mean of x+w where a=True, and places that mean in those rows where a=True
# takes the mean of x+w where a=False, and places that mean in those rows where a=False
> DT[,b:=mean(x+w),by=a]
> DT[,bb:=mean(x+w),by=y]
# takes the mean those x+w where y has a particular value, and places that mean in those rows where 'y' has that particular value
> DT
            x   y          z           w        m     a          b         bb
1:  0.5855288   a -0.9193220  0.84515294 2.222250  TRUE -0.1344804  1.7823270
2:  0.7094660   a -0.1162478  0.01351355 2.483679  TRUE -0.1344804  1.7823270
3: -0.1093033   a  1.8173120  3.30262306 2.745885 FALSE  0.5329939  1.7823270
4: -0.4534972   b  0.3706279  0.13736501 2.297817 FALSE  0.5329939 -0.2314257
5:  0.6058875   b  0.5202165  0.27062516 2.614970  TRUE -0.1344804 -0.2314257
6: -1.8179560   b -0.7505320  0.56329827 1.281854 FALSE  0.5329939 -0.2314257
7:  0.6300986   c  0.8168998  0.66732535 2.688628  TRUE -0.1344804  0.9034347
8: -0.2761841   c -0.8863575  0.78562966 1.940151 FALSE  0.5329939  0.9034347
9:  1.0000000 xyz  3.0000000 -6.00000000 3.169925  TRUE -0.1344804 -5.0000000
# DT[,b:=x+w,by=a]# investigate := here by=a has no effect/meaning, result is plain x+w


# .N performs the count operation
# much faster than doing this with '$' operator with data.frames
> set.seed(123)
> DT <- data.table(x=sample(letters[1:3],1E5,TRUE))
> DT[, .N , by=x]
   x     N
1: a 33387
2: c 33201
3: b 33412


# Data table displays head and tail on printing(if rows > 10), upto 5 rows by default
> DT <- data.table(x=rnorm(108), y=rep(c("a","b","c"),each=3), z=rnorm(108)  )
> DT
              x y          z
  1:  0.2595897 a  0.3465684
  2:  0.9175107 a -0.5586167
  3: -0.7223183 a -0.5779154
  4: -0.8082840 b -0.4857059
  5: -0.1413520 b  2.1646988
 ---                        
104:  0.1497959 b  0.5695802
105:  0.4448392 b  0.5444199
106:  0.5927161 c  0.4580150
107: -0.7308481 c -0.8395697
108:  0.9478600 c  1.7600820

# setting a key and subsetting by the key
> DT <- data.table(x=rep(c("a","b","c"),each=100), y=rnorm(300))
> setkey(DT,x)# sets the key column
> DT['a']# subsets DT by the key column where value is 'a'
     x            y
  1: a -1.306669139
  2: a  2.274916566
  3: a  0.061858554
  4: a -1.855072142
  5: a  0.338466569
  6: a -0.923976487
  7: a  1.097144160
  8: a -0.055702686
  9: a -1.743302520
 10: a  0.494960874
 11: a  1.661066826
 12: a -0.040365291
 13: a  0.344891869
 14: a -0.729853447
 15: a -0.747471910
 16: a -0.224614059
 17: a  0.272710573
 18: a -1.213054986
 19: a -0.939785098
 20: a -0.124348733
 21: a  0.021787387
 22: a -0.310363495
 23: a  0.033636997
 24: a  0.928996256
 25: a  0.700675785
 26: a -0.368244725
 27: a -0.959786572
 28: a -0.747360608
 29: a  0.218970195
 30: a  1.774939640
 31: a  1.825295509
 32: a  0.437885846
 33: a  1.439990556
 34: a -0.301208049
 35: a -0.637588823
 36: a  0.695498167
 37: a -1.163612614
 38: a -0.820462397
 39: a -0.972846729
 40: a -0.751088181
 41: a  0.409575958
 42: a -1.917457854
 43: a  0.566555938
 44: a -0.500603301
 45: a -0.330591733
 46: a -0.186026390
 47: a  1.196320039
 48: a -0.992055195
 49: a -1.791544025
 50: a  0.532988603
 51: a  0.468435764
 52: a  0.139692709
 53: a  1.611187474
 54: a  1.619931270
 55: a  0.598534929
 56: a  0.106652486
 57: a -0.577713467
 58: a -2.069376473
 59: a -1.530218243
 60: a -0.099710472
 61: a  1.234266863
 62: a  0.289219543
 63: a -0.455315619
 64: a  1.288299394
 65: a -0.296794818
 66: a  0.009959562
 67: a  0.397110322
 68: a  1.596552925
 69: a -1.387607725
 70: a  2.664512851
 71: a  0.138421527
 72: a  2.568424578
 73: a -1.166016139
 74: a  1.816470720
 75: a  0.511291381
 76: a -0.423561016
 77: a -0.346115177
 78: a -0.741495668
 79: a -1.009643433
 80: a  1.606456088
 81: a  1.190564476
 82: a -2.138818017
 83: a  0.503598365
 84: a -0.410865656
 85: a  0.306584679
 86: a  0.322743507
 87: a  1.366079297
 88: a -0.588816303
 89: a  1.697831865
 90: a  0.056363268
 91: a -0.099593465
 92: a -0.210198996
 93: a -1.855162735
 94: a  0.415003310
 95: a  0.502506377
 96: a -1.081812841
 97: a  2.301772157
 98: a  0.855549385
 99: a  0.283150573
100: a -0.337277742
     x            y


# Join 2 tables by keys
> DT <- data.table(x=c('a','a','b','b','dt1'),y=1:5)
> DT2 <- data.table(x=c('a','b','b','dt2'),y=5:8)
> setkey(DT,x)
> setkey(DT2,x)
> merge(DT,DT2)
   x y.x y.y
1: a   1   5
2: a   2   5
3: b   3   6
4: b   3   7
5: b   4   6
6: b   4   7


# FAST reading of files from disk
> big_df <- data.frame(x=rnorm(1E6),y=rnorm(1E6))
> file1 <- tempfile()
> write.table(big_df, file = file1, row.names = FALSE, col.names = TRUE, sep="\t", quote = FALSE)

# this is much faster
> system.time(fread(file1))
   user  system elapsed 
   0.66    0.05    0.61 

# this is much slower
> system.time(read.table(file1, header=TRUE, sep="\t"))
   user  system elapsed 
  18.50    0.37   19.00 
```

https://r-forge.r-project.org/scm/viewvc.php/pkg/NEWS?view=markup&root=datatable

https://stackoverflow.com/questions/13618488/what-you-can-do-with-a-data-frame-that-you-cant-with-a-data-table

https://github.com/raphg/Biostat-578/blob/master/Advanced_data_manipulation.Rpres
