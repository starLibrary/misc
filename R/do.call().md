<a href="https://www.stat.berkeley.edu/~s133/Docall.html">original page . . .</a><br/>

The do.call function<br/>
R has an interesting function called do.call. This function allows you to call any R function, but instead of writing out the arguments one by one, you can use a list to hold the arguments of the function. While it may not seem useful on the surface, a simple example will help to show how powerful do.call is.<br/>
Suppose we have three comma-separated text files that have information on the same three variables. Here's a look at the first few lines in one of the files:

- a,b,x,y
- b,B,2.49778634403711,-0.351307445767206
- b,A,-0.594683138631719,1.19629936975021
- b,D,1.14857619580259,0.653315728121014
- c,A,0.957476532595248,0.935608419299617

It's easy to read each one in with read.csv, and then to call the rbind (combine by rows) function to make one big data frame. (Remember that rbind will only work if all the data frames being combined have the same variable names.)

```R
> one = read.csv('1.csv')
> nrow(one)
[1] 21
> two = read.csv('2.csv')
> nrow(two)
[1] 25
> three = read.csv('3.csv')
> nrow(three)
[1] 27
> big = rbind(one,two,three)
> nrow(big)
[1] 73
```

As I said, that was pretty easy. But now suppose we have 20 csv files that we want to read and combine. We could do what we did with the three files, but, not only would it get tiring, there's a chance of making an error when we have to type so many commands.
In the past, when we've had problems like this, sapply was able to help. Let's try it here, by writing a function that will take a number, create a filename by pasting .csv at the end, and reading in the data:

```R
> allframes = sapply(1:20,function(x)read.csv(paste(x,'csv',sep='.')))
> head(allframes)
  [,1]       [,2]       [,3]       [,4]       [,5]       [,6]       [,7]
a factor,21  factor,25  factor,27  factor,25  factor,27  factor,21  factor,24
b factor,21  factor,25  factor,27  factor,25  factor,27  factor,21  factor,24
x Numeric,21 Numeric,25 Numeric,27 Numeric,25 Numeric,27 Numeric,21 Numeric,24
y Numeric,21 Numeric,25 Numeric,27 Numeric,25 Numeric,27 Numeric,21 Numeric,24
  [,8]       [,9]       [,10]      [,11]      [,12]      [,13]      [,14]
a factor,28  factor,23  factor,23  factor,22  factor,26  factor,24  factor,23
b factor,28  factor,23  factor,23  factor,22  factor,26  factor,24  factor,23
x Numeric,28 Numeric,23 Numeric,23 Numeric,22 Numeric,26 Numeric,24 Numeric,23
```

There were no errors, but the result certainly is strange looking! While we haven't talked about it before, the "s" in sapply stands for simplify, and the problem that we've just seen is that sapply tried too hard to simplify our result. It created an odd matrix, where each element represents one of the columns of one of the files we read in. Fortunately, there's a function that's closely related to sapply called lapply. The difference between sapply and lapply is that lapply will never try to simplify its results. It will always return a list, the same length as its first argument, with each element of the list resulting in the function we passed to lapply operating on one element of the first argument. In our case, calling lapply instead of sapply will give us a list of length 20, where each element is the result of calling read.csv on one of the 20 files. This is where do.call comes in. Instead of having to pass 20 data frames to rbind, we can use do.call to pass all 20 of them to rbind, since they are in a list, and that's exactly what do.call is looking for.

```R
> allframes = lapply(1:20,function(x)read.csv(paste(x,'csv',sep='.')))
> sapply(allframes,nrow)
 [1] 21 25 27 25 27 21 24 28 23 23 22 26 24 23 25 29 28 30 27 29
> answer = do.call(rbind,allframes)
> nrow(answer)
[1] 507
```

We can combine all the data frames without storing them separately or passing them individually to rbind.

<hr>
File translated from TEX by TTH, version 3.67.
On 28 Feb 2011, 16:55.
