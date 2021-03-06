---
layout: page
title: R for Data Analysis
subtitle: Subsetting data
minutes: 45
---



> ## Learning Objectives {.objectives}
>
> * To be able to subset vectors, factors, matrices, lists, and data frames
> * To be able to extract individual and multiple elements:
>     * by index,
>     * by name,
>     * using comparison operations
> * To be able to skip and remove elements from various data structures.
>

R has many powerful subset operators and mastering them will allow you to
easily perform complex operations on any kind of dataset.

There are six different ways we can subset any kind of object, and three
different subsetting operators for the different data structures.

Let's start with the workhorse of R: atomic vectors.


~~~{.r}
x <- c(5.4, 6.2, 7.1, 4.8, 7.5)
names(x) <- letters[1:5]
x
~~~



~~~{.output}
  a   b   c   d   e 
5.4 6.2 7.1 4.8 7.5 

~~~

So now that we've created a dummy vector to play with, how do we get at its
contents?

## Accessing elements using their indices

To extract elements of a vector we can give their corresponding index, starting
from one:


~~~{.r}
x[1]
~~~



~~~{.output}
  a 
5.4 

~~~


~~~{.r}
x[4]
~~~



~~~{.output}
  d 
4.8 

~~~

The square brackets operator is just like any other function. For atomic vectors
(and matrices), it means "get me the nth element".

We can ask for multiple elements at once:


~~~{.r}
x[c(1, 3)]
~~~



~~~{.output}
  a   c 
5.4 7.1 

~~~

Or slices of the vector:


~~~{.r}
x[1:4]
~~~



~~~{.output}
  a   b   c   d 
5.4 6.2 7.1 4.8 

~~~

the `:` operator just creates a sequence of numbers from the left element to the right.
I.e. `x[1:4]` is equivalent to `x[c(1,2,3,4)]`.

We can ask for the same element multiple times:


~~~{.r}
x[c(1,1,3)]
~~~



~~~{.output}
  a   a   c 
5.4 5.4 7.1 

~~~

If we ask for a number outside of the vector, R will return missing values:


~~~{.r}
x[6]
~~~



~~~{.output}
<NA> 
  NA 

~~~

This is a vector of length one containing an `NA`, whose name is also `NA`.

If we ask for the 0th element, we get an empty vector:


~~~{.r}
x[0]
~~~



~~~{.output}
named numeric(0)

~~~

> ##Vector numbering in R starts at 1 {.callout} 
> 
> In many programming languages (C and python, for example), the first
> element of a vector has an index of 0. In R, the first element is 1.

## Skipping and removing elements

If we use a negative number as the index of a vector, R will return
every element *except* for the one specified:


~~~{.r}
x[-2]
~~~



~~~{.output}
  a   c   d   e 
5.4 7.1 4.8 7.5 

~~~


We can skip multiple elements:


~~~{.r}
x[c(-1, -5)]  # or x[-c(1,5)]
~~~



~~~{.output}
  b   c   d 
6.2 7.1 4.8 

~~~

> ## Tip: Order of operations {.callout}
>
> A common trip up for novices occurs when trying to skip
> slices of a vector. Most people first try to negate a
> sequence like so:
>
> 
> ~~~{.r}
> x[-1:3]
> ~~~
> 
> 
> 
> ~~~{.error}
> Error in x[-1:3]: only 0's may be mixed with negative subscripts
> 
> ~~~
>
> This gives a somewhat cryptic error:
>
> But remember the order of operations. `:` is really a function, so
> what happens is it takes its first argument as -1, and second as 3,
> so generates the sequence of numbers: `c(-1, 0, 1, 2, 3)`.
>
> The correct solution is to wrap that function call in brackets, so
> that the `-` operator applies to the results:
>
> 
> ~~~{.r}
> x[-(1:3)]
> ~~~
> 
> 
> 
> ~~~{.output}
>   d   e 
> 4.8 7.5 
> 
> ~~~
>

To remove elements from a vector, we need to assign the results back
into the variable:


~~~{.r}
x <- x[-4]
x
~~~



~~~{.output}
  a   b   c   e 
5.4 6.2 7.1 7.5 

~~~

> ## Challenge 1 {.challenge}
>
> Given the following code:
>
> 
> ~~~{.r}
> y <- c(5.4, 6.2, 7.1, 4.8, 7.5)
> names(y) <- c('a', 'b', 'c', 'd', 'e')
> print(y)
> ~~~
> 
> 
> 
> ~~~{.output}
>   a   b   c   d   e 
> 5.4 6.2 7.1 4.8 7.5 
> 
> ~~~
>
> Come up with at least 3 different commands that will produce the following output:
>
> 
> ~~~{.output}
>   b   c   d 
> 6.2 7.1 4.8 
> 
> ~~~
>

## Subsetting through logical operations

We can subset through logical operations:


~~~{.r}
x[c(TRUE, TRUE, FALSE, FALSE)]
~~~



~~~{.output}
  a   b 
5.4 6.2 

~~~


Since comparison operators evaluate to logical vectors, we can also
use them to succinctly subset vectors:


~~~{.r}
x[x > 7]
~~~



~~~{.output}
  c   e 
7.1 7.5 

~~~

> ## Tip: Chaining logical operations {.callout}
>
> There are many situations in which you will wish to combine multiple conditions.
> To do so several logical operations exist in R:
>
>  * `|` logical OR (elementwise comparison): returns `TRUE`, if either the left or right are `TRUE`.
>  * `&` logical AND (elementwise comparison): returns `TRUE` if both the left and right are `TRUE`
>  * `!` logical NOT: converts `TRUE` to `FALSE` and `FALSE` to `TRUE`
>  * `&&` and `||` evaluates left to right looking at the first element only. 
>

Any function that returns a logical vector can be used for subsetting:

~~~{.r}
is.na(c(x, NA))
~~~



~~~{.output}
    a     b     c     e       
FALSE FALSE FALSE FALSE  TRUE 

~~~
Similarly other functions for identifying special values, e.g.`is.nan` (`Nan` values), `is.infinite` (`Inf` values), `is.finite` (values that are not `NA`, `NaN`, `Inf`).


> ## Challenge {.challenge}
>
> Given the following code:
>
> 
> ~~~{.r}
> x <- c(5.4, 6.2, 7.1, 4.8, 7.5)
> names(x) <- c('a', 'b', 'c', 'd', 'e')
> print(x)
> ~~~
> 
> 
> 
> ~~~{.output}
>   a   b   c   d   e 
> 5.4 6.2 7.1 4.8 7.5 
> 
> ~~~
>
> Write a subsetting command to return the values in x that are greater than 4 and less than 7.
>


## Subsetting by name

We can extract elements by using their name, instead of index:


~~~{.r}
x[c("a", "c")]
~~~



~~~{.output}
  a   c 
5.4 7.1 

~~~

This is usually a much more reliable way to subset objects: the
position of various elements can often change when chaining together
subsetting operations, but the names will always remain the same!

Unfortunately we can't skip or remove elements so easily.

To skip (or remove) a single named element we have to find the index of the corresponding column, say "a":

~~~{.r}
names(x) == "a"
~~~



~~~{.output}
[1]  TRUE FALSE FALSE FALSE FALSE

~~~
The condition operator is applied to every name of the vector `x`. Only the
first name is "a" so that element is TRUE.

`which` then converts this to an index:


~~~{.r}
which(names(x) == "a")
~~~



~~~{.output}
[1] 1

~~~

Only the first element is `TRUE`, so `which` returns 1. Now that we have indices
the skipping works because we have a negative index!


~~~{.r}
x[-which(names(x) == "a")]
~~~



~~~{.output}
  b   c   d   e 
6.2 7.1 4.8 7.5 

~~~



Skipping multiple named indices is similar, but uses a different comparison
operator:


~~~{.r}
x[-which(names(x) %in% c("a", "c"))]
~~~



~~~{.output}
  b   d   e 
6.2 4.8 7.5 

~~~

The `%in%` goes through each element of its left argument, in this case the
names of `x`, and asks, "Does this element occur in the second argument?".

> ## Tip: Non-unique names {.callout}
>
> You should be aware that it is possible for multiple elements in a
> vector to have the same name. (For a data frame, columns can have
> the same name --- although R tries to avoid this --- but row names
> must be unique.) Consider these examples:

>
>~~~{.r}
> x <- 1:3
> x
>~~~
>
>
>
>~~~{.output}
>[1] 1 2 3
>
>~~~
>
>
>
>~~~{.r}
> names(x) <- c('a', 'a', 'a')  
> x
>~~~
>
>
>
>~~~{.output}
>a a a 
>1 2 3 
>
>~~~
>
>
>
>~~~{.r}
> x['a']  # only returns first value
>~~~
>
>
>
>~~~{.output}
>a 
>1 
>
>~~~
>
>
>
>~~~{.r}
> x[which(names(x) == 'a')]  # returns all three values
>~~~
>
>
>
>~~~{.output}
>a a a 
>1 2 3 
>
>~~~



> ## Tip: Getting help for operators {.callout}
>
> Remember you can search for help on operators by wrapping them in quotes:
> `help("%in%")` or `?"%in%"`.
>

So why can't we use `==` like before? That's an excellent question.

Let's take a look at just the comparison component:


~~~{.r}
names(x) == c('a', 'c')
~~~



~~~{.error}
Warning in names(x) == c("a", "c"): longer object length is not a multiple
of shorter object length

~~~



~~~{.output}
[1]  TRUE FALSE  TRUE

~~~

Obviously "c" is in the names of `x`, so why didn't this work? `==` works
slightly differently than `%in%`. It will compare each element of its left argument
to the corresponding element of its right argument.

Here's a mock illustration:


~~~{.r}
c("a", "b", "c", "e")  # names of x
   |    |    |    |    # The elements == is comparing
c("a", "c")
~~~

When one vector is shorter than the other, it gets *recycled*:


~~~{.r}
c("a", "b", "c", "e")  # names of x
   |    |    |    |    # The elements == is comparing
c("a", "c", "a", "c")
~~~

In this case R simply repeats `c("a", "c")` twice. If the longer
vector length isn't a multiple of the shorter vector length, then
R will also print out a warning message:


~~~{.r}
names(x) == c('a', 'c', 'e')
~~~



~~~{.output}
[1]  TRUE FALSE FALSE

~~~

This difference between `==` and `%in%` is important to remember,
because it can introduce hard to find and subtle bugs!


## Matrix subsetting

Matrices are also subsetted using the `[` function. In this case
it takes two arguments: the first applying to the rows, the second
to its columns:


~~~{.r}
set.seed(1)
m <- matrix(rnorm(6*4), ncol=4, nrow=6)
m[3:4, c(3,1)]
~~~



~~~{.output}
            [,1]       [,2]
[1,]  1.12493092 -0.8356286
[2,] -0.04493361  1.5952808

~~~

You can leave the first or second arguments blank to retrieve all the
rows or columns respectively:


~~~{.r}
m[, c(3,4)]
~~~



~~~{.output}
            [,1]        [,2]
[1,] -0.62124058  0.82122120
[2,] -2.21469989  0.59390132
[3,]  1.12493092  0.91897737
[4,] -0.04493361  0.78213630
[5,] -0.01619026  0.07456498
[6,]  0.94383621 -1.98935170

~~~

If we only access one row or column, R will automatically convert the result
to a vector:


~~~{.r}
m[3,]
~~~



~~~{.output}
[1] -0.8356286  0.5757814  1.1249309  0.9189774

~~~

If you want to keep the output as a matrix, you need to specify a *third* argument;
`drop = FALSE`:


~~~{.r}
m[3, , drop=FALSE]
~~~



~~~{.output}
           [,1]      [,2]     [,3]      [,4]
[1,] -0.8356286 0.5757814 1.124931 0.9189774

~~~

Unlike vectors, if we try to access a row or column outside of the matrix,
R will throw an error:


~~~{.r}
m[, c(3,6)]
~~~



~~~{.error}
Error in m[, c(3, 6)]: subscript out of bounds

~~~

It is useful to note that matrices
are laid out in *column-major format* by default. That is the elements of the
vector are arranged column-wise:


~~~{.r}
matrix(1:6, nrow=2, ncol=3)
~~~



~~~{.output}
     [,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6

~~~

If you wish to populate the matrix by row, use `byrow=TRUE`:


~~~{.r}
matrix(1:6, nrow=2, ncol=3, byrow=TRUE)
~~~



~~~{.output}
     [,1] [,2] [,3]
[1,]    1    2    3
[2,]    4    5    6

~~~

Matrices can also be subsetted using their rownames and column names
instead of their row and column indices.

> ## Tip: Higher dimensional arrays {.callout}
>
> When dealing with multi-dimensional arrays, each argument to `[`
> corresponds to a dimension. For example, a 3D array, the first three
> arguments correspond to the rows, columns, and depth dimension.
>

~~~{.r}
a <- array(1:12, dim=c(2,3,2))
dim(a)
~~~



~~~{.output}
[1] 2 3 2

~~~



~~~{.r}
a[,,2]
~~~



~~~{.output}
     [,1] [,2] [,3]
[1,]    7    9   11
[2,]    8   10   12

~~~

> ## Challenge 2 {.challenge}
>
> Given the following code:
>
> 
> ~~~{.r}
> m <- matrix(1:18, nrow=3, ncol=6)
> print(m)
> ~~~
> 
> 
> 
> ~~~{.output}
>      [,1] [,2] [,3] [,4] [,5] [,6]
> [1,]    1    4    7   10   13   16
> [2,]    2    5    8   11   14   17
> [3,]    3    6    9   12   15   18
> 
> ~~~
>
> 1. Which of the following commands will extract the values 11 and 14?
>
> A. `m[2,4,2,5]`
>
> B. `m[2:5]`
>
> C. `m[4:5,2]`
>
> D. `m[2,c(4,5)]`
>


## List subsetting

Now we'll introduce some new subsetting operators. There are three functions
used to subset lists. `[`, as we've seen for atomic vectors and matrices,
as well as `[[` and `$`.

Using `[` will always return a list. If you want to *subset* a list, but not
*extract* an element, then you will likely use `[`.


~~~{.r}
xlist <- list(a = "PSRC", b = 1:10, data = head(hh))
xlist[1]
~~~



~~~{.output}
$a
[1] "PSRC"

~~~

This returns a *list with one element*.

We can subset elements of a list exactly the same was as atomic
vectors using `[`. Comparison operations however won't work as
they're not recursive, they will try to condition on the data structures
in each element of the list, not the individual elements within those
data structures.


~~~{.r}
xlist[1:2]
~~~



~~~{.output}
$a
[1] "PSRC"

$b
 [1]  1  2  3  4  5  6  7  8  9 10

~~~

To extract individual elements of a list, you need to use the double-square
bracket function: `[[`.


~~~{.r}
xlist[[1]]
~~~



~~~{.output}
[1] "PSRC"

~~~

Notice that now the result is a vector, not a list.

You can't extract more than one element at once:


~~~{.r}
xlist[[1:2]]
~~~



~~~{.error}
Error in xlist[[1:2]]: subscript out of bounds

~~~

Nor use it to skip elements:


~~~{.r}
xlist[[-1]]
~~~



~~~{.error}
Error in xlist[[-1]]: attempt to select more than one element

~~~

But you can use names to both subset and extract elements:


~~~{.r}
xlist[["a"]]
~~~



~~~{.output}
[1] "PSRC"

~~~

The `$` function is a shorthand way for extracting elements by name:


~~~{.r}
xlist$data
~~~



~~~{.output}
  county_id city_id  hh10  hh20  hh30  hh40     city_name county_name
1         0      56    NA    NA    NA    NA Covington PAA        <NA>
2         0     140    NA    NA    NA    NA      Enumclaw        <NA>
3         1     109 14853 17874 18051 18542  Everett MUGA   Snohomish
4         1     113  9488 12157 13841 15639  Lake Stevens   Snohomish
5         1     110  4836  5808  6218  6688        Monroe   Snohomish
6         1     108  4464  4986  5398  5873 Mukilteo MUGA   Snohomish

~~~

> ## Challenge 3 {.challenge}
> Given the following list:
>
> 
> ~~~{.r}
> xlist <- list(a = "PSRC", b = 1:10, data = head(hh))
> ~~~
>
> Using your knowledge of both list and vector subsetting, extract the number 2 from xlist. 
> Hint: the number 2 is contained within the "b" item in the list.


## Subsetting data frames

Data frames are lists underneath the hood, so similar rules
apply. However they are also two dimensional objects:

`[` with one argument will act the same was as for lists, where each list
element corresponds to a column. The resulting object will be a data frame:


~~~{.r}
head(hh[1])
~~~



~~~{.output}
  county_id
1         0
2         0
3         1
4         1
5         1
6         1

~~~

Similarly, `[[` will act to extract *a single column*:


~~~{.r}
head(hh[["city_id"]])
~~~



~~~{.output}
[1]  56 140 109 113 110 108

~~~

And `$` provides a convenient shorthand to extract columns by name:


~~~{.r}
head(hh$city_name)
~~~



~~~{.output}
[1] "Covington PAA" "Enumclaw"      "Everett MUGA"  "Lake Stevens" 
[5] "Monroe"        "Mukilteo MUGA"

~~~

With two arguments, `[` behaves the same way as for matrices:


~~~{.r}
hh[1:3,]
~~~



~~~{.output}
  county_id city_id  hh10  hh20  hh30  hh40     city_name county_name
1         0      56    NA    NA    NA    NA Covington PAA        <NA>
2         0     140    NA    NA    NA    NA      Enumclaw        <NA>
3         1     109 14853 17874 18051 18542  Everett MUGA   Snohomish

~~~

If we subset a single row, the result will be a data frame (because
the elements are mixed types):


~~~{.r}
hh[3,]
~~~



~~~{.output}
  county_id city_id  hh10  hh20  hh30  hh40    city_name county_name
3         1     109 14853 17874 18051 18542 Everett MUGA   Snohomish

~~~

But for a single column the result will be a vector (this can
be changed with the third argument, `drop = FALSE`).

Another way of subsetting data frames is using the `subset` command:

~~~{.r}
subset(hh, city_name == "Seattle")
~~~



~~~{.output}
   county_id city_id   hh10   hh20   hh30   hh40 city_name county_name
55         2       9 286525 335516 363492 396033   Seattle        King

~~~



~~~{.r}
pierce <- subset(hh, county_name == "Pierce")
dim(pierce)
~~~



~~~{.output}
[1] 26  8

~~~



~~~{.r}
head(pierce)
~~~



~~~{.output}
    county_id city_id  hh10  hh20   hh30   hh40    city_name county_name
117         4      29 57428 64486  65252  66392 Pierce-Rural      Pierce
118         4      59 78892 95102 117175 141291       Tacoma      Pierce
119         4      53 67417 85491  93594  93893           UU      Pierce
120         4      76  4355  4276   4936   5648       DuPont      Pierce
121         4      77  3419  4429   5295   6069   Gig Harbor      Pierce
122         4      78   318   370    423    463          Roy      Pierce

~~~

> ## Challenge 5 {.challenge}
>
> Fix each of the following common data frame subsetting errors:
>
> 1. Extract observations for cities in county 1
>
> 
> ~~~{.r}
> hh[hh$county_id = 1,]
> ~~~
>
> 2. Extract all columns except one through five
>
> 
> ~~~{.r}
> hh[,-1:5]
> ~~~
>
> 3. Extract the rows where the number of households in 2010 is larger than 50,000
>
> 
> ~~~{.r}
> hh[hh$hh10 > 50000]
> ~~~
>
> 4. Extract the first row, and the sixth and seventh columns
>   (`city_name` and `county_id`).
>
> 
> ~~~{.r}
> hh[1, 6, 7]
> ~~~
>
> 5. Advanced: extract rows that contain information for counties  3 and 4
>
> 
> ~~~{.r}
> hh[hh$county_id == 3 | 4,]
> ~~~
>

> ## Challenge 6 {.challenge}
>
> 1. Why does `hh[1:20]` return an error? How does it differ from `hh[1:20, ]`?
>
>
> 2. Create a new `data.frame` called `hh_small` that only contains rows 1 through 9
> and 19 through 23.
>

## Challenge solutions

> ## Solution to challenge 1 {.challenge}
>
> Given the following code:
>
> 
> ~~~{.r}
> y <- c(5.4, 6.2, 7.1, 4.8, 7.5)
> names(y) <- c('a', 'b', 'c', 'd', 'e')
> print(y)
> ~~~
> 
> 
> 
> ~~~{.output}
>   a   b   c   d   e 
> 5.4 6.2 7.1 4.8 7.5 
> 
> ~~~
>
> 1. Come up with at least 3 different commands that will produce the following output:
>
> 
> ~~~{.output}
>   b   c   d 
> 6.2 7.1 4.8 
> 
> ~~~
>
> 
> ~~~{.r}
> y[2:4] 
> y[-c(1,5)]
> y[c("b", "c", "d")]
> y[c(2,3,4)]
> ~~~
>
>

> ## Solution to challenge 2 {.challenge}
>
> Given the following code:
>
> 
> ~~~{.r}
> m <- matrix(1:18, nrow=3, ncol=6)
> print(m)
> ~~~
> 
> 
> 
> ~~~{.output}
>      [,1] [,2] [,3] [,4] [,5] [,6]
> [1,]    1    4    7   10   13   16
> [2,]    2    5    8   11   14   17
> [3,]    3    6    9   12   15   18
> 
> ~~~
>
> 1. Which of the following commands will extract the values 11 and 14?
>
> A. `m[2,4,2,5]`
>
> B. `m[2:5]`
>
> C. `m[4:5,2]`
>
> D. `m[2,c(4,5)]`
>
> Answer: D

> ## Solution to challenge 3 {.challenge}
> Given the following list:
>
> 
> ~~~{.r}
> xlist <- list(a = "PSRC", b = 1:10, data = head(hh))
> ~~~
>
> Using your knowledge of both list and vector subsetting, extract the number 2 from xlist. 
> Hint: the number 2 is contained within the "b" item in the list.
>
> 
> ~~~{.r}
> xlist$b[2]
> xlist[[2]][2]
> xlist[["b"]][2]
> ~~~


> ## Solution to challenge 5 {.challenge}
>
> Fix each of the following common data frame subsetting errors:
>
> 1. Extract observations collected for the year 1957
>
> 
> ~~~{.r}
> # hh[hh$county_id = 1,]
> hh[hh$county_id == 1,]
> ~~~
>
> 2. Extract all columns except 1 through to 4
>
> 
> ~~~{.r}
> # hh[,-1:4]
> hh[,-(1:5)]
> ~~~
>
> 3. Extract the rows where the number of households in 2010 is larger than 50,000
>
> 
> ~~~{.r}
> # hh[hh$hh2010 > 50000]
> hh[hh$hh10 > 50000, ]
> ~~~
>
> 4. Extract the first row, and the sixth and seventh columns
>   (`city_name` and `county_id`).
>
> 
> ~~~{.r}
> # hh[1, 6, 7]
> hh[1, c(6, 7)]
> ~~~
>
> 5. Advanced: extract rows that contain information for counties  3 and 4
>
> 
> ~~~{.r}
> # hh[hh$county_id == 3 | 4,]
> hh[hh$county_id == 3 | hh$county_id == 4,]
> hh[hh$county_id %in% c(3, 4),]
> hh[hh$county_id %in% 3:4,]
> ~~~
>

> ## Solution to challenge 6 {.challenge}
>
> 1. Why does `hh[1:20]` return an error? How does it differ from `hh[1:20, ]`?
>
> Answer: `hh` is a data.frame so needs to be subsetted on two dimensions. `hh[1:20, ]` subsets the data to give the first 20 rows and all columns.
>
> 2. Create a new `data.frame` called `hh_small` that only contains rows 1 through 9
> and 19 through 23. You can do this in one or two steps.
>
> 
> ~~~{.r}
> hh_small <- hh[c(1:9, 19:23),]
> ~~~
>
