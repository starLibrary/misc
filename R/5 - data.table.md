<hr>

<a href="https://www.r-exercises.com/2017/06/29/data-manipulation-with-data-table-part-2/">https://www.r-exercises.com/2017/06/29/data-manipulation-with-data-table-part-2/</a>





|SELECT|UNSELECT|
|---|---|
|**Select Rows**|**Unselect Rows**|
|flights[ 1:4 ]<br/>flights[ origin == "JFK" & month == 6L ]<br/>flights[ order(origin, -dest) ]<br/>_order(...) within the frame of a data.table uses data.table’s internal<br/>fast radix order forder()_|---|
|**Select Columns**|**Unselect Columns**|
|_gives vector: flights[  ,  arr_delay   ]_<br/>flights[  ,  .(arr_delay)   ]<br/>flights[  ,  .(arr_delay, dep_delay)  ]<br/>flights[  ,  c("arr_delay")   ]<br/>flights[  ,  c("arr_delay", "dep_delay")  ]<br/>flights[  ,  arr_delay : dep_delay  ]|flights[  ,  !c("arr_delay", "dep_delay")  ]<br/>flights[  ,  -c("arr_delay", "dep_delay")  ]<br/>flights[  ,  -(arr_delay : dep_delay)  ]|
|select_cols = c("arr_delay", "dep_delay")<br/>flights[  ,  ..select_cols  ]<br/>flights[  ,  select_cols, with=FALSE  ]|---|
|---|---|
|**Operations on Columns**|---|
|_gives vector: flights[  ,  mean(arr_delay)   ]_<br/>flights[  ,  .(m_arr = mean(arr_delay))   ]<br/>flights[  ,  .(mean(arr_delay), range(dep_delay))   ]<br/>flights[  ,  .(m_arr = mean(arr_delay), m_dep = range(dep_delay))   ]|---|
|flights[  ,  .(delay_arr = arr_delay, delay_dep = dep_delay)  ]<br/>flights[  ,  .(sum((arr_delay + dep_delay) < 0))  ]<br/>_gives vector: flights[  ,  sum((arr_delay + dep_delay) < 0)  ]_|---|
|**Operations on Rows & Columns**|---|
|flights[origin == "JFK" & month == 6L, .(.N)]<br/>_gives vector: flights[origin == "JFK" & month == 6L, .N]_<br/>_gives vector: flights[origin == "JFK" & month == 6L, length(dest)]_<br/>flights[ origin == "JFK" & month == 6L,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.(m_arr = mean(arr_delay), m_dep = range(dep_delay)) ]|---|
|---|---|
|**group 'by'**|---|
|_writing 'by=' is optional_<br/>flights[  ,  .N,  by=origin  ]<br/>flights[  ,  .N,  by=.(origin)  ]<br/>flights[  ,  .N,  by=.(origin, dest)  ]<br/>**flights[  ,  .N,  by=.(dep_delayed = dep_delay>0, arr_delayed = arr_delay>0)  ]**|---|
|---|---|
|**Combined Operation**|---|
|flights[ carrier == "AA", .N, by = origin ]<br/>flights[ carrier == "AA", .N, by = .(origin,dest) ]<br/>flights[ carrier == "AA",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.(m_arr = mean(arr_delay), m_dep = range(dep_delay)),<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;by = .(origin, dest) ]|---|
|---|---|
|**'keyby' operation**|---|
|flights[  carrier == "AA",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.(mean(arr_delay), mean(dep_delay)),<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;by = .(origin, dest, month)  ]<br/><br/>_original order is preserved in 'by', whereas 'keyby' reorders_<br/>_as per the grouping variables_<br/><br/>flights[  carrier == "AA",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.(mean(arr_delay), mean(dep_delay)),<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;keyby = .(origin, dest, month) ]|---|
|---|---|
|**chaining operations**|---|
|_chaining operations does not form intermediate results_<br/>flights[  carrier == "AA",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.N,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;by = .(origin, dest)  ][  order(origin, -dest)  ]<br/><br/>flights[  carrier == "AA",<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.N,<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;by=.(origin, dest)  ][  order(-N)  ]|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|
|---|---|










|SELECT|UNSELECT|
|---|---|
|Rows||
|flights[origin == "JFK" & month == 6L]<br/>flights[order(origin, -dest)]<br/>flights[  ,  .(m_arr = mean(arr_delay), m_dep = range(dep_delay))   ]||
|Columns|---|
|_gives vector: flights[  ,  arr_delay   ]_<br/>flights[  ,  .(arr_delay)   ]# gives data.table, not vector<br/><br/>flights[  ,  .(arr_delay)   ]<br/>flights[  ,  .(arr_delay, dep_delay)  ]||
|flights[  ,  .(m_arr = mean(arr_delay), m_dep = range(dep_delay))   ]||
















#### SELECT Rows
- flights[1:4]
- flights[origin == "JFK" & month == 6L]
- flights[order(origin, -dest)]
- _order(...) within the frame of a data.table uses data.table’s internal fast radix order forder()_

#### SELECT Columnns
- flights[  ,  .(arr_delay)   ]# gives data.table, not vector
- _gives vector: flights[  ,  arr_delay   ]_
- flights[  ,  .(arr_delay, dep_delay)  ]
- flights[  ,  -c("arr_delay", "dep_delay")  ]
- flights[  ,  -(arr_delay : dep_delay)  ]
- flights[  ,  .(m_arr = mean(arr_delay), m_dep = range(dep_delay))   ]
- flights[  ,  sum((arr_delay + dep_delay) < 0)  ]

#### 
```R
> # SELECT Rows
> flights[1:4]
   year month day dep_delay arr_delay carrier origin dest air_time distance hour
1: 2014     1   1        14        13      AA    JFK  LAX      359     2475    9
2: 2014     1   1        -3        13      AA    JFK  LAX      363     2475   11
3: 2014     1   1         2         9      AA    JFK  LAX      351     2475   19
4: 2014     1   1        -8       -26      AA    LGA  PBI      157     1035    7
> flights[origin == "JFK" & month == 6L]
      year month day dep_delay arr_delay carrier origin dest air_time distance hour
   1: 2014     6   1        -9        -5      AA    JFK  LAX      324     2475    8
   2: 2014     6   1       -10       -13      AA    JFK  LAX      329     2475   12
   3: 2014     6   1        18        -1      AA    JFK  LAX      326     2475    7
   4: 2014     6   1        -6       -16      AA    JFK  LAX      320     2475   10
   5: 2014     6   1        -4       -45      AA    JFK  LAX      326     2475   18
  ---                                                                              
8418: 2014     6  30        -3        -6      MQ    JFK  PIT       62      340   14
8419: 2014     6  30        -5       -32      MQ    JFK  RDU       65      427   14
8420: 2014     6  30        -3       -16      MQ    JFK  DCA       39      213   17
8421: 2014     6  30        -2         7      MQ    JFK  DCA       52      213    7
8422: 2014     6  30        -7       -18      MQ    JFK  RDU       67      427    8
> flights[order(origin, -dest)]
        year month day dep_delay arr_delay carrier origin dest air_time distance hour
     1: 2014     1   5         6        49      EV    EWR  XNA      195     1131    8
     2: 2014     1   6         7        13      EV    EWR  XNA      190     1131    8
     3: 2014     1   7        -6       -13      EV    EWR  XNA      179     1131    8
     4: 2014     1   8        -7       -12      EV    EWR  XNA      184     1131    8
     5: 2014     1   9        16         7      EV    EWR  XNA      181     1131    8
    ---                                                                              
253312: 2014    10  31        -1       -22      WN    LGA  ATL      112      762    9
253313: 2014    10  31        -5       -23      WN    LGA  ATL      112      762   20
253314: 2014     4   6        -6        -1      EV    LGA  AGS      110      678   10
253315: 2014     4   7         2         1      EV    LGA  AGS      111      678   11
253316: 2014     4  11         0       -19      EV    LGA  AGS      102      678   10
> # order(...) within the frame of a data.table uses
> # data.table’s internal fast radix order forder()
> 
> # SELECT Columnns
> flights[  ,  .(arr_delay)   ]# gives data.table, not vector
        arr_delay
     1:        13
     2:        13
     3:         9
     4:       -26
     5:         1
    ---          
253312:       -30
253313:       -14
253314:        16
253315:        15
253316:         1
> flights[  ,  list(arr_delay)   ]# gives data.table, not vector
        arr_delay
     1:        13
     2:        13
     3:         9
     4:       -26
     5:         1
    ---          
253312:       -30
253313:       -14
253314:        16
253315:        15
253316:         1
> flights[  ,  c("arr_delay")   ]# gives data.table, not vector
        arr_delay
     1:        13
     2:        13
     3:         9
     4:       -26
     5:         1
    ---          
253312:       -30
253313:       -14
253314:        16
253315:        15
253316:         1
> # gives vector    flights[  ,  arr_delay   ]
> 
> 
> 
> flights[  ,  .(arr_delay, dep_delay)  ]
        arr_delay dep_delay
     1:        13        14
     2:        13        -3
     3:         9         2
     4:       -26        -8
     5:         1         2
    ---                    
253312:       -30         1
253313:       -14        -5
253314:        16        -8
253315:        15        -4
253316:         1        -5
> flights[  ,  list(arr_delay, dep_delay)  ]
        arr_delay dep_delay
     1:        13        14
     2:        13        -3
     3:         9         2
     4:       -26        -8
     5:         1         2
    ---                    
253312:       -30         1
253313:       -14        -5
253314:        16        -8
253315:        15        -4
253316:         1        -5
> flights[  ,  c("arr_delay", "dep_delay")  ]
        arr_delay dep_delay
     1:        13        14
     2:        13        -3
     3:         9         2
     4:       -26        -8
     5:         1         2
    ---                    
253312:       -30         1
253313:       -14        -5
253314:        16        -8
253315:        15        -4
253316:         1        -5
> 
> select_cols = c("arr_delay", "dep_delay")
> flights[  ,  ..select_cols  ]
        arr_delay dep_delay
     1:        13        14
     2:        13        -3
     3:         9         2
     4:       -26        -8
     5:         1         2
    ---                    
253312:       -30         1
253313:       -14        -5
253314:        16        -8
253315:        15        -4
253316:         1        -5
> flights[  ,  select_cols, with=FALSE  ]
        arr_delay dep_delay
     1:        13        14
     2:        13        -3
     3:         9         2
     4:       -26        -8
     5:         1         2
    ---                    
253312:       -30         1
253313:       -14        -5
253314:        16        -8
253315:        15        -4
253316:         1        -5
> 
> 
> 
> flights[  ,  -(arr_delay : dep_delay)  ]
        year month day carrier origin dest air_time distance hour
     1: 2014     1   1      AA    JFK  LAX      359     2475    9
     2: 2014     1   1      AA    JFK  LAX      363     2475   11
     3: 2014     1   1      AA    JFK  LAX      351     2475   19
     4: 2014     1   1      AA    LGA  PBI      157     1035    7
     5: 2014     1   1      AA    JFK  LAX      350     2475   13
    ---                                                          
253312: 2014    10  31      UA    LGA  IAH      201     1416   14
253313: 2014    10  31      UA    EWR  IAH      189     1400    8
253314: 2014    10  31      MQ    LGA  RDU       83      431   11
253315: 2014    10  31      MQ    LGA  DTW       75      502   11
253316: 2014    10  31      MQ    LGA  SDF      110      659    8
> flights[  ,  c("arr_delay", "dep_delay")  ]# automatically sets with=FALSE
        arr_delay dep_delay
     1:        13        14
     2:        13        -3
     3:         9         2
     4:       -26        -8
     5:         1         2
    ---                    
253312:       -30         1
253313:       -14        -5
253314:        16        -8
253315:        15        -4
253316:         1        -5
> flights[  ,  !c("arr_delay", "dep_delay")  ]# automatically sets with=FALSE
        year month day carrier origin dest air_time distance hour
     1: 2014     1   1      AA    JFK  LAX      359     2475    9
     2: 2014     1   1      AA    JFK  LAX      363     2475   11
     3: 2014     1   1      AA    JFK  LAX      351     2475   19
     4: 2014     1   1      AA    LGA  PBI      157     1035    7
     5: 2014     1   1      AA    JFK  LAX      350     2475   13
    ---                                                          
253312: 2014    10  31      UA    LGA  IAH      201     1416   14
253313: 2014    10  31      UA    EWR  IAH      189     1400    8
253314: 2014    10  31      MQ    LGA  RDU       83      431   11
253315: 2014    10  31      MQ    LGA  DTW       75      502   11
253316: 2014    10  31      MQ    LGA  SDF      110      659    8
> flights[  ,  -c("arr_delay", "dep_delay")  ]# automatically sets with=FALSE
        year month day carrier origin dest air_time distance hour
     1: 2014     1   1      AA    JFK  LAX      359     2475    9
     2: 2014     1   1      AA    JFK  LAX      363     2475   11
     3: 2014     1   1      AA    JFK  LAX      351     2475   19
     4: 2014     1   1      AA    LGA  PBI      157     1035    7
     5: 2014     1   1      AA    JFK  LAX      350     2475   13
    ---                                                          
253312: 2014    10  31      UA    LGA  IAH      201     1416   14
253313: 2014    10  31      UA    EWR  IAH      189     1400    8
253314: 2014    10  31      MQ    LGA  RDU       83      431   11
253315: 2014    10  31      MQ    LGA  DTW       75      502   11
253316: 2014    10  31      MQ    LGA  SDF      110      659    8
> 
> 
> 
> # returns from year to day
> flights[, year:day  ]
        year month day
     1: 2014     1   1
     2: 2014     1   1
     3: 2014     1   1
     4: 2014     1   1
     5: 2014     1   1
    ---               
253312: 2014    10  31
253313: 2014    10  31
253314: 2014    10  31
253315: 2014    10  31
253316: 2014    10  31
> # returns from day to year
> flights[, day:year  ]
        day month year
     1:   1     1 2014
     2:   1     1 2014
     3:   1     1 2014
     4:   1     1 2014
     5:   1     1 2014
    ---               
253312:  31    10 2014
253313:  31    10 2014
253314:  31    10 2014
253315:  31    10 2014
253316:  31    10 2014
> # returns all columns except from year till day
> flights[, -(year:day)  ]
        dep_delay arr_delay carrier origin dest air_time distance hour
     1:        14        13      AA    JFK  LAX      359     2475    9
     2:        -3        13      AA    JFK  LAX      363     2475   11
     3:         2         9      AA    JFK  LAX      351     2475   19
     4:        -8       -26      AA    LGA  PBI      157     1035    7
     5:         2         1      AA    JFK  LAX      350     2475   13
    ---                                                               
253312:         1       -30      UA    LGA  IAH      201     1416   14
253313:        -5       -14      UA    EWR  IAH      189     1400    8
253314:        -8        16      MQ    LGA  RDU       83      431   11
253315:        -4        15      MQ    LGA  DTW       75      502   11
253316:        -5         1      MQ    LGA  SDF      110      659    8
> flights[, !(year:day)  ]
        dep_delay arr_delay carrier origin dest air_time distance hour
     1:        14        13      AA    JFK  LAX      359     2475    9
     2:        -3        13      AA    JFK  LAX      363     2475   11
     3:         2         9      AA    JFK  LAX      351     2475   19
     4:        -8       -26      AA    LGA  PBI      157     1035    7
     5:         2         1      AA    JFK  LAX      350     2475   13
    ---                                                               
253312:         1       -30      UA    LGA  IAH      201     1416   14
253313:        -5       -14      UA    EWR  IAH      189     1400    8
253314:        -8        16      MQ    LGA  RDU       83      431   11
253315:        -4        15      MQ    LGA  DTW       75      502   11
253316:        -5         1      MQ    LGA  SDF      110      659    8
> 
> 
> 
> 
> 
> # OPERATIONS on Columns
> flights[  ,  .(m_arr = mean(arr_delay))   ]
      m_arr
1: 8.146702
> 
> flights[  ,  .(m_arr = mean(arr_delay), m_dep = mean(dep_delay))   ]
      m_arr    m_dep
1: 8.146702 12.46526
> flights[  ,  .(m_arr = mean(arr_delay), m_dep = mean(dep_delay)),  by=.(origin,dest)  ]
     origin dest      m_arr     m_dep
  1:    JFK  LAX   3.301332  8.359718
  2:    LGA  PBI   4.939315 10.168617
  3:    EWR  LAX   8.425461 15.882631
  4:    JFK  MIA   1.630909 10.008364
  5:    JFK  SEA   3.168595 10.858953
 ---                                 
217:    LGA  AVL -31.500000 -6.500000
218:    LGA  GSP   6.000000  6.000000
219:    LGA  SBN -15.500000  5.000000
220:    EWR  SBN  -2.333333 -1.500000
221:    LGA  DAL -16.000000 -6.266667
> 
> flights[  ,  .(m_arr = mean(arr_delay), m_dep = range(dep_delay))   ]
      m_arr m_dep
1: 8.146702  -112
2: 8.146702  1498
> flights[  ,  .(m_arr = mean(arr_delay), m_dep = range(dep_delay)),  by=.(origin,dest)  ]
     origin dest      m_arr m_dep
  1:    JFK  LAX   3.301332   -17
  2:    JFK  LAX   3.301332   848
  3:    LGA  PBI   4.939315   -17
  4:    LGA  PBI   4.939315   384
  5:    EWR  LAX   8.425461   -27
 ---                             
438:    LGA  SBN -15.500000    13
439:    EWR  SBN  -2.333333   -10
440:    EWR  SBN  -2.333333    20
441:    LGA  DAL -16.000000   -13
442:    LGA  DAL -16.000000    -2
> 
> 
> flights[  ,  .(delay_arr = arr_delay, delay_dep = dep_delay)  ]
        delay_arr delay_dep
     1:        13        14
     2:        13        -3
     3:         9         2
     4:       -26        -8
     5:         1         2
    ---                    
253312:       -30         1
253313:       -14        -5
253314:        16        -8
253315:        15        -4
253316:         1        -5
> flights[  ,  sum((arr_delay + dep_delay) < 0)  ]
[1] 141814
> 
> 
> 
> 
> 
> # 'by' section
> flights[  ,  .N  ]# gives vector
[1] 253316
> flights[  ,  .(.N)  ]# gives data-table
        N
1: 253316
> 
> flights[origin == "JFK" & month == 6L, .N]# use this instead of below
[1] 8422
> flights[origin == "JFK" & month == 6L, length(dest)]
[1] 8422
> flights[origin == "JFK" & month == 6L, .(.N)]# gives data-table
      N
1: 8422
> 
> 
> 
> flights[  ,  .N,  by=origin  ]
   origin     N
1:    JFK 81483
2:    LGA 84433
3:    EWR 87400
> flights[  ,  .N,  by=.(origin)  ]
   origin     N
1:    JFK 81483
2:    LGA 84433
3:    EWR 87400
> flights[  ,  .N,  by=.(origin, dest)  ]
     origin dest     N
  1:    JFK  LAX 10208
  2:    LGA  PBI  2307
  3:    EWR  LAX  4226
  4:    JFK  MIA  2750
  5:    JFK  SEA  1815
 ---                  
217:    LGA  AVL     2
218:    LGA  GSP     3
219:    LGA  SBN     2
220:    EWR  SBN     6
221:    LGA  DAL    15
> 
> 
> 
> flights[carrier == "AA", .N, by = origin]
   origin     N
1:    JFK 11923
2:    LGA 11730
3:    EWR  2649
> flights[carrier == "AA", .N, by = "origin"]
   origin     N
1:    JFK 11923
2:    LGA 11730
3:    EWR  2649
> flights[carrier == "AA", .N, by = .(origin,dest)]
    origin dest    N
 1:    JFK  LAX 3387
 2:    LGA  PBI  245
 3:    EWR  LAX   62
 4:    JFK  MIA 1876
 5:    JFK  SEA  298
 6:    EWR  MIA  848
 7:    JFK  SFO 1312
 8:    JFK  BOS 1173
 9:    JFK  ORD  432
10:    JFK  IAH    7
11:    JFK  AUS  297
12:    EWR  DFW 1618
13:    LGA  ORD 4366
14:    JFK  STT  229
15:    JFK  SJU  690
16:    LGA  MIA 3334
17:    LGA  DFW 3785
18:    JFK  LAS  595
19:    JFK  MCO  597
20:    JFK  EGE   85
21:    JFK  DFW  474
22:    JFK  SAN  299
23:    JFK  DCA  172
24:    EWR  PHX  121
    origin dest    N
> flights[carrier == "AA", .N, by = list(origin,dest)]
    origin dest    N
 1:    JFK  LAX 3387
 2:    LGA  PBI  245
 3:    EWR  LAX   62
 4:    JFK  MIA 1876
 5:    JFK  SEA  298
 6:    EWR  MIA  848
 7:    JFK  SFO 1312
 8:    JFK  BOS 1173
 9:    JFK  ORD  432
10:    JFK  IAH    7
11:    JFK  AUS  297
12:    EWR  DFW 1618
13:    LGA  ORD 4366
14:    JFK  STT  229
15:    JFK  SJU  690
16:    LGA  MIA 3334
17:    LGA  DFW 3785
18:    JFK  LAS  595
19:    JFK  MCO  597
20:    JFK  EGE   85
21:    JFK  DFW  474
22:    JFK  SAN  299
23:    JFK  DCA  172
24:    EWR  PHX  121
    origin dest    N
> flights[carrier == "AA", .N, by = c("origin", "dest")]
    origin dest    N
 1:    JFK  LAX 3387
 2:    LGA  PBI  245
 3:    EWR  LAX   62
 4:    JFK  MIA 1876
 5:    JFK  SEA  298
 6:    EWR  MIA  848
 7:    JFK  SFO 1312
 8:    JFK  BOS 1173
 9:    JFK  ORD  432
10:    JFK  IAH    7
11:    JFK  AUS  297
12:    EWR  DFW 1618
13:    LGA  ORD 4366
14:    JFK  STT  229
15:    JFK  SJU  690
16:    LGA  MIA 3334
17:    LGA  DFW 3785
18:    JFK  LAS  595
19:    JFK  MCO  597
20:    JFK  EGE   85
21:    JFK  DFW  474
22:    JFK  SAN  299
23:    JFK  DCA  172
24:    EWR  PHX  121
    origin dest    N
> 
> 
> 
> 
> 
> flights[  carrier == "AA",
+           .(mean(arr_delay), mean(dep_delay)),
+           by = .(origin, dest, month)  ]
     origin dest month         V1         V2
  1:    JFK  LAX     1   6.590361 14.2289157
  2:    LGA  PBI     1  -7.758621  0.3103448
  3:    EWR  LAX     1   1.366667  7.5000000
  4:    JFK  MIA     1  15.720670 18.7430168
  5:    JFK  SEA     1  14.357143 30.7500000
 ---                                        
196:    LGA  MIA    10  -6.251799 -1.4208633
197:    JFK  MIA    10  -1.880184  6.6774194
198:    EWR  PHX    10  -3.032258 -4.2903226
199:    JFK  MCO    10 -10.048387 -1.6129032
200:    JFK  DCA    10  16.483871 15.5161290
> # original order is preserved in 'by', whereas
> # 'keyby' reorders as per the grouping
> flights[  carrier == "AA",
+           .(mean(arr_delay), mean(dep_delay)),
+           keyby = .(origin, dest, month)]
     origin dest month         V1         V2
  1:    EWR  DFW     1   6.427673 10.0125786
  2:    EWR  DFW     2  10.536765 11.3455882
  3:    EWR  DFW     3  12.865031  8.0797546
  4:    EWR  DFW     4  17.792683 12.9207317
  5:    EWR  DFW     5  18.487805 18.6829268
 ---                                        
196:    LGA  PBI     1  -7.758621  0.3103448
197:    LGA  PBI     2  -7.865385  2.4038462
198:    LGA  PBI     3  -5.754098  3.0327869
199:    LGA  PBI     4 -13.966667 -4.7333333
200:    LGA  PBI     5 -10.357143 -6.8571429
> 
> 
> 
> 
> 
> # chaining operations does not form intermediate results
> flights[  carrier == "AA",
+           .N,
+           by = .(origin, dest)  ][  order(origin, -dest)  ]
    origin dest    N
 1:    EWR  PHX  121
 2:    EWR  MIA  848
 3:    EWR  LAX   62
 4:    EWR  DFW 1618
 5:    JFK  STT  229
 6:    JFK  SJU  690
 7:    JFK  SFO 1312
 8:    JFK  SEA  298
 9:    JFK  SAN  299
10:    JFK  ORD  432
11:    JFK  MIA 1876
12:    JFK  MCO  597
13:    JFK  LAX 3387
14:    JFK  LAS  595
15:    JFK  IAH    7
16:    JFK  EGE   85
17:    JFK  DFW  474
18:    JFK  DCA  172
19:    JFK  BOS 1173
20:    JFK  AUS  297
21:    LGA  PBI  245
22:    LGA  ORD 4366
23:    LGA  MIA 3334
24:    LGA  DFW 3785
    origin dest    N
> 
> flights[  carrier == "AA",
+           .N,
+           by=.(origin, dest)  ][  order(-N)  ]
    origin dest    N
 1:    LGA  ORD 4366
 2:    LGA  DFW 3785
 3:    JFK  LAX 3387
 4:    LGA  MIA 3334
 5:    JFK  MIA 1876
 6:    EWR  DFW 1618
 7:    JFK  SFO 1312
 8:    JFK  BOS 1173
 9:    EWR  MIA  848
10:    JFK  SJU  690
11:    JFK  MCO  597
12:    JFK  LAS  595
13:    JFK  DFW  474
14:    JFK  ORD  432
15:    JFK  SAN  299
16:    JFK  SEA  298
17:    JFK  AUS  297
18:    LGA  PBI  245
19:    JFK  STT  229
20:    JFK  DCA  172
21:    EWR  PHX  121
22:    JFK  EGE   85
23:    EWR  LAX   62
24:    JFK  IAH    7
    origin dest    N
> 
> 
> 
> #  how many flights started late but arrived early (or on time), started and arrived late etc…
> flights[  ,  .N,  .(dep_delay>0, arr_delay>0)  ]
   dep_delay arr_delay      N
1:      TRUE      TRUE  72836
2:     FALSE      TRUE  34583
3:     FALSE     FALSE 119304
4:      TRUE     FALSE  26593
> # columns can be renamed as below
> flights[  ,  .N,  .(dep_delayed = dep_delay>0, arr_delayed = arr_delay>0)  ]
   dep_delayed arr_delayed      N
1:        TRUE        TRUE  72836
2:       FALSE        TRUE  34583
3:       FALSE       FALSE 119304
4:        TRUE       FALSE  26593
> DT[  ,  .N,  by = .(a, b>0)  ]
Error: object 'DT' not found
> 
> 
> 
> 
> 
> # data.table is internally a list as well with all its columns
> # of equal length. .SD stands for Subset of Data. It by itself
> # is a data.table that holds the data for the current group
> # defined using by. .SD contains all the columns except the
> # grouping columns by default. .SD preserves the original order
> DT
Error: object 'DT' not found
> DT[, print(.SD), by = ID]
Error: object 'DT' not found
> 
> 
> 
> DT[  ,  lapply(.SD, mean),  by = ID  ]
Error: object 'DT' not found
> 
> 
> 
> flights[  ,  lapply(.SD, mean),  by=.(origin,dest,carrier)  ]
     origin dest carrier year     month      day  dep_delay  arr_delay  air_time distance      hour
  1:    JFK  LAX      AA 2014  5.938589 15.85651  5.9359315   2.418660 329.70121     2475 12.621494
  2:    LGA  PBI      AA 2014  2.673469 15.10612 -0.2122449  -8.951020 153.02449     1035 11.073469
  3:    EWR  LAX      AA 2014  1.596774 14.32258  4.8709677   4.806452 344.00000     2454 18.096774
  4:    JFK  MIA      AA 2014  5.696695 15.86514  9.9381663   1.096482 152.34115     1089 11.158849
  5:    JFK  SEA      AA 2014  5.587248 15.69463 17.2684564  10.003356 336.60738     2422 17.869128
 ---                                                                                               
359:    EWR  MSN      UA 2014  9.000000 16.66667 -7.0000000   1.666667 118.33333      799 13.666667
360:    EWR  SBN      EV 2014 10.000000 28.50000 -1.5000000  -2.333333  98.16667      637 18.666667
361:    LGA  CLE      F9 2014 10.000000 28.50000 12.0000000   5.000000  69.16667      419  8.333333
362:    LGA  DAL      VX 2014 10.000000 29.60000 -6.2666667 -16.000000 196.60000     1381 12.933333
363:    LGA  ATL      EV 2014 10.000000 29.00000 -1.0000000  -3.000000 119.00000      762 10.000000
> 
> 
> 
> 
> 
> # .SDcols accepts either column names or column indices.
> # For example, .SDcols = c("arr_delay", "dep_delay") ensures
> # that .SD contains only these two columns for each group.
> # .SDcols = -(arr_delay:dep_delay) deselects the range
> 
> flights[carrier == "AA",                       ## Only on trips with carrier "AA"
+         lapply(.SD, mean),                     ## compute the mean
+         by = .(origin, dest, month),           ## for every 'origin,dest,month'
+         .SDcols = c("arr_delay", "dep_delay")] ## for just those specified in .SDcols
     origin dest month  arr_delay  dep_delay
  1:    JFK  LAX     1   6.590361 14.2289157
  2:    LGA  PBI     1  -7.758621  0.3103448
  3:    EWR  LAX     1   1.366667  7.5000000
  4:    JFK  MIA     1  15.720670 18.7430168
  5:    JFK  SEA     1  14.357143 30.7500000
 ---                                        
196:    LGA  MIA    10  -6.251799 -1.4208633
197:    JFK  MIA    10  -1.880184  6.6774194
198:    EWR  PHX    10  -3.032258 -4.2903226
199:    JFK  MCO    10 -10.048387 -1.6129032
200:    JFK  DCA    10  16.483871 15.5161290
> 
> 
> 
> 
> 
> # return the first two rows for each month
> flights[  ,  head(.SD, 2),  by = month  ]
    month year day dep_delay arr_delay carrier origin dest air_time distance hour
 1:     1 2014   1        14        13      AA    JFK  LAX      359     2475    9
 2:     1 2014   1        -3        13      AA    JFK  LAX      363     2475   11
 3:     2 2014   1        -1         1      AA    JFK  LAX      358     2475    8
 4:     2 2014   1        -5         3      AA    JFK  LAX      358     2475   11
 5:     3 2014   1       -11        36      AA    JFK  LAX      375     2475    8
 6:     3 2014   1        -3        14      AA    JFK  LAX      368     2475   11
 7:     4 2014   1        -8       -23      MQ    LGA  BNA      113      764   18
 8:     4 2014   1        -8       -11      MQ    LGA  RDU       71      431   18
 9:     5 2014   1        43         5      AA    JFK  LAS      288     2248   17
10:     5 2014   1        -1       -38      AA    JFK  SFO      330     2586    7
11:     6 2014   1        -9        -5      AA    JFK  LAX      324     2475    8
12:     6 2014   1       -10       -13      AA    JFK  LAX      329     2475   12
13:     7 2014   1        -3        -2      VX    JFK  SFO      343     2586    7
14:     7 2014   1        -6       -14      VX    EWR  LAX      320     2454    9
15:     8 2014   1        -3        -5      AA    JFK  BOS       40      187    8
16:     8 2014   1        19        55      AA    JFK  SFO      373     2586   15
17:     9 2014   1        -9       -26      AA    JFK  LAX      325     2475    8
18:     9 2014   1        -7       -21      AA    JFK  LAX      320     2475   12
19:    10 2014   1        -5       -15      EV    EWR  MEM      140      946   14
20:    10 2014   1        -7       -21      EV    EWR  BNA      109      748   12
> 
> 
> # concatenate columns a and b for each group in ID, like melt()
> DT[  ,  .(c(a,b)),  by = ID  ]
Error: object 'DT' not found
> DT[  ,  .(val = c(a,b)),  by = ID  ]
Error: object 'DT' not found
> 
> # all the values of column a and b concatenated, but returned as a list column
> DT[, .(val = .(c(a,b))), by = ID]
Error: object 'DT' not found
> # almost same as above
> 
> 
> 
> 
> 
> DT[, print(c(a,b)), by = ID]
Error: object 'DT' not found
> DT[, print(list(c(a,b))), by = ID]
Error: object 'DT' not found
> 
> 
> 
> flights[origin == "JFK" & month == 6L, .(m_arr = mean(arr_delay), m_dep = mean(dep_delay))]
      m_arr    m_dep
1: 5.839349 9.807884
> # We first subset in i to find matching row indices
> # We do not subset the entire data.table corresponding to those rows yet.
> # Now, we look at j and find that it uses only two columns.
> # Therefore we subset just those columns corresponding to the matching rows, and compute their mean().
> # Because the three main components of the query (i, j and by) are together inside, we are able to
> # therefore avoid the entire subset (i.e., subsetting the columns besides arr_delay and dep_delay),
> # for both speed and memory efficiency.


```
