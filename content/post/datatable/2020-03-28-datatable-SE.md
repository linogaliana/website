---
title: "Variable name in functions, it's easy with datatable"
author: ~
date: "2020-03-28"
slug: datatable-nse
categories: ["R", "datatable"]
tags: ["R", "datatable"]
image:
  caption: Variable names in datatable, it's easy
  focal_point: Smart
draft: no
output:
  html_document:
    keep_md: yes
---

I recently gave my opinion concerning the [never-ending debate](https://stackoverflow.com/questions/21435339/data-table-vs-dplyr-can-one-do-something-well-the-other-cant-or-does-poorly) between `{dplyr}` and `{data.table}` fans ([here](https://gitlab.com/linogaliana/documentationR/-/issues/9)). I listed three arguments in favor of `{data.table}` approach :

1. `{data.table}` is very stable while `{dplyr}` changes a lot.  This makes processes depending on `{dplyr}` more likely to break. 
2. `{data.table}` is really fast and is not very demanding in terms of RAM. This is, of course, the main arguments in favor of `{data.table}`.
3. `{data.table}` grammar is often considered harder to learn than `{dplyr}` equivalent verbs. That's not necessarily true. 

Regarding the first point, I think it is worth mentionning that a `Docker` based infrastructure where version changes can be tested should reduce the risk. 

The second point is clearly in favor of `{data.table}`. Hadley Wickham acknowledges for that : 

> **Memory and performance**

> I've lumped these together, because, to me, they're not that important. Most R users work with well under 1 million rows of data, and dplyr is sufficiently fast enough for that size of data that you're not aware of processing time. We optimise dplyr for expressiveness on medium data; feel free to use data.table for raw speed on bigger data.
>
> Hadley Wickham ([lien](https://stackoverflow.com/questions/21435339/data-table-vs-dplyr-can-one-do-something-well-the-other-cant-or-does-poorly/27840349#27840349))

There exists many benchmark concerning `{data.table}`. I won't go further on that point. 

The third point deserves some development. It is often said that `{data.table}` syntax is not user-friendly. However, once you get the logic of the syntax, you see that it is very powerful !

There is one point where `{dplyr}` logic is really really hard to follow: when you want to use standard evaluation. Have you ever tried to understand the [dplyr vignette](https://cran.r-project.org/web/packages/dplyr/vignettes/programming.html) on the subject ? For some operations you need `!!`, for others `!!!`, not forgetting about `:=` and `rlang::sym` otherwise it won't work... In `{data.table}`, this is very easy: `get` makes quite well the job !

I think it is worth developing a little bit on the subject because for people that create packages (if you don't you should!), this is fundamental. Have you ever tried to use a function that takes a variable name as parameter ? For instance, 


```r
do_something <- function(data, xvar = "x", group_vars = c("a","b")){
  #do something by group
}
```

If yes, you might have been through hell if you used `{dplyr}` :imp:. I think it is a very common operation when you don't want to repeat yourself to create that kind of function. But it is very painful with `{dplyr}` and very easy with `{data.table}`. If you are not interested on `{dplyr}`, you can jump to the [final Section](#datatable). 

:bulb: If you want the executive summary of the following, you can look at the tables below. 

* With `{data.table}`

| Operation                 | Standard Evaluation (SE) | Non Standard Evaluation  (NSE)  |
| --------------------------|--------------------------|---------------------------------|
| Filter                   | `df[x<10]`               | `df[get('x')<10]`               |
| Subset columns           | `df[,x]`                 | `df[,.SD,.SDcols = "x"]`        |
| Create a new column from another one|`df[, xnew := x+1]`       | `df[, c("xnew") := get("x")+1]`   |
| Create new columns using average with several grouping variables | | `df[, newcolnames := lapply(.SD, mean), by = grouping_var, .SDcols = xvars]` |


* With `{dplyr}`

| Operation                 | Standard Evaluation (SE) | Non Standard Evaluation  (NSE)  |
| -------------------------|--------------------------|---------------------------------|
| Filter                   | `df %>% dplyr::filter(x < 10)`  | `df %>% dplyr::filter(!!rlang::sym("x")<10)` |
| Subset columns           |`df %>% dplyr::select(x)`| `df %>% dplyr::select("x")` |
| Create a new column from another one|`df %>% dplyr::mutate(xnew = x+1)`       | `df %>% dplyr::mutate(xnew = !!rlang::sym(x)+1)`   |
| Create a new column using average with several grouping variables | | `df %>% dplyr::group_by(!!!rlang::syms(grouping_var)) %>% dplyr::mutate("x" := mean(!!rlang::sym(xvar)))`




# Non standard evaluation: what are we talking about ? {#NSE}

In `R`, there exists two ways to refer to an object:

* **Standard evaluation** (**SE**): find a value behind a name. `R` starts from `.GlobalEnv` and goes down along the series of namespaces in the `searchpaths()` as long as it does not find a value that matches the name. If `R` does not find an object, it throws an error ;
* **Non standard evaluation** (**NSE**): `R` does not respect that rule because it does not start searching in the global environment. Evaluation uses an implicit context to match a value behind a name. 

<!----
In the context of NSE, the object `x` is interpreted as belonging to a specific environment (for instance a dataframe). Only if the object is not found in this environment, `R` will start from the global environment. 
---->

`{data.table}` and `{dplyr}` are both based on non standard evaluation. Variable names are quoted as `dt[,x]` (`{data.table}`) or  `df %>% mutate(x)` (`{dplyr}`): `x` does not exist in the global environment, only within the dataframe. 


This example ([borrowed from ThinkR](https://thinkr.fr/tidyeval/)) is maybe more explicit


```r
iris %>% dplyr::filter(Species == "setosa")
```

The `Species` symbol is not related to anything when thinking about standard evaluation. No `Species` symbol is associated with a value in the global environment. Here, `Species` must be evaluated in the context of the dataframe `iris`.  Base `R`  would not allow such shortcut because you would need to write `iris$Species`:


```r
iris[iris$Species == "setosa", ]
```

As `{dplyr}`, `{data.table}` works well with NSE. In `{data.table}`, you would write:


```r
data.table::as.data.table(iris)[Species == "setosa"]
```

**NSE must be avoided when you start to use functions or share programs.** Objects in another users environment do not necessarily match yours. And, as you might see with some examples, using string names for variables in argument can lead to very general and very powerful functions. 

Regarding the question of environments, the following two commands will not return the same result :rage: since there is an ambiguity regarding `Species`: are we talking about the variable or the object ?


```r
nrow(
  data.table::as.data.table(iris)[Species == "setosa"]
)
```

```
## [1] 50
```

```r
Species <- "setosa"
nrow(
  data.table::as.data.table(iris)[Species == Species]
)
```

```
## [1] 150
```

# The `{dplyr}` approach {#dplyr}

> :bulb: People allergic to `{dplyr}` can jump to to the [final Section](#datatable). 

With `{dplyr}`, you must use the tools provided by `{rlang}` to be able to use SE. In the past, it was possible to use functions annoted with `*_` (e.g. `group_by_`) but they are now deprecated. 

First, let's use import `{magrittr}` to get the `%>%` available. We won't need to import `{dplyr}`, `{data.table}` or any other package. 


```r
library("magrittr")
```

First, there is a situation where SE is easy to use in `{dplyr}`: in `select` operations


```r
mtcars %>% dplyr::select(c("mpg","cyl")) %>% head()
```

```
##                    mpg cyl
## Mazda RX4         21.0   6
## Mazda RX4 Wag     21.0   6
## Datsun 710        22.8   4
## Hornet 4 Drive    21.4   6
## Hornet Sportabout 18.7   8
## Valiant           18.1   6
```

Difficulties arise when you want to use a variable name in `mutate` or `group_by`. If you don't unquote the name, you will just write the name everywhere :sob: :


```r
xvar <- "mpg"
mtcars %>% dplyr::mutate("x" := xvar) %>% head()
```

```
##    mpg cyl disp  hp drat    wt  qsec vs am gear carb   x
## 1 21.0   6  160 110 3.90 2.620 16.46  0  1    4    4 mpg
## 2 21.0   6  160 110 3.90 2.875 17.02  0  1    4    4 mpg
## 3 22.8   4  108  93 3.85 2.320 18.61  1  1    4    1 mpg
## 4 21.4   6  258 110 3.08 3.215 19.44  1  0    3    1 mpg
## 5 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2 mpg
## 6 18.1   6  225 105 2.76 3.460 20.22  1  0    3    1 mpg
```

So you will need to do the following:

1. `rlang::sym` (or `rlang::syms` if you have several variables)
2. Apply the double bang `!!` operation (triple bang `!!!` with several)


```r
new_variable <- function(data, xvar = "mpg"){
  data %>% dplyr::mutate("x" := !!rlang::sym(xvar))
}
new_variable(mtcars) %>% head()
```

```
##    mpg cyl disp  hp drat    wt  qsec vs am gear carb    x
## 1 21.0   6  160 110 3.90 2.620 16.46  0  1    4    4 21.0
## 2 21.0   6  160 110 3.90 2.875 17.02  0  1    4    4 21.0
## 3 22.8   4  108  93 3.85 2.320 18.61  1  1    4    1 22.8
## 4 21.4   6  258 110 3.08 3.215 19.44  1  0    3    1 21.4
## 5 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2 18.7
## 6 18.1   6  225 105 2.76 3.460 20.22  1  0    3    1 18.1
```

The [dplyr vignette](https://cran.r-project.org/web/packages/dplyr/vignettes/programming.html) is even more obscure :dizzy_face:.

The situation is getting worse when you want to use grouping variables :sweat_smile:. For instance, if you create a new variable by computing mean values by group, you will end up with this complicated piece of code:


```r
new_variable_group <- function(data, grouping_var = c("cyl","vs"), xvar = "mpg"){
  data %>%
    dplyr::group_by(!!!rlang::syms(grouping_var)) %>%
    dplyr::mutate("x" := mean(!!rlang::sym(xvar)))
}
new_variable_group(mtcars) %>% head()
```

```
## # A tibble: 6 x 12
## # Groups:   cyl, vs [4]
##     mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb     x
##   <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
## 1  21       6   160   110  3.9   2.62  16.5     0     1     4     4  20.6
## 2  21       6   160   110  3.9   2.88  17.0     0     1     4     4  20.6
## 3  22.8     4   108    93  3.85  2.32  18.6     1     1     4     1  26.7
## 4  21.4     6   258   110  3.08  3.22  19.4     1     0     3     1  19.1
## 5  18.7     8   360   175  3.15  3.44  17.0     0     0     3     2  15.1
## 6  18.1     6   225   105  2.76  3.46  20.2     1     0     3     1  19.1
```

Note that here we have a complex piece of code for something quite easy to think about :grimacing:. That would be even worse if you wanted to set the name of the output variable (here `x`) programmatically or perform operations on multiple columns :scream:. 

The syntax to subset rows (`filter`) looks like the one to subset columns. For instance, the piece of code that was not working previously to filter species should be written:


```r
Species <- "setosa"
iris %>% dplyr::filter(!!rlang::sym("Species") == Species) %>% head()
```

```
##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
## 1          5.1         3.5          1.4         0.2  setosa
## 2          4.9         3.0          1.4         0.2  setosa
## 3          4.7         3.2          1.3         0.2  setosa
## 4          4.6         3.1          1.5         0.2  setosa
## 5          5.0         3.6          1.4         0.2  setosa
## 6          5.4         3.9          1.7         0.4  setosa
```

End of the journey with `{dplyr}` :triumph:

# `data.table`: standard evaluation made easy {#datatable}

> With `{data.table}`, using string variable names is far easier 
:nerd_face:

There exists several ways to use standard evaluation in `{data.table}`. The first one is based on `with` and comes from base `R` syntax:


```r
dt <- data.table::as.data.table(mtcars)
select_cols = c("mpg", "cyl")
head(dt[ , select_cols, with = FALSE])
```

```
##     mpg cyl
## 1: 21.0   6
## 2: 21.0   6
## 3: 22.8   4
## 4: 21.4   6
## 5: 18.7   8
## 6: 18.1   6
```

`unix terminal` users should be familiar with the second one. With `..` (two dots), we can say to `{data.table}` to search for names one level higher than where `[...]` bind us to be. This is in fact the level of the `data.table` itself:


```r
head(dt[ , ..select_cols])
```

```
##     mpg cyl
## 1: 21.0   6
## 2: 21.0   6
## 3: 22.8   4
## 4: 21.4   6
## 5: 18.7   8
## 6: 18.1   6
```

However, I clearly prefer, in this context equivalent to a `select` in `{dplyr}`, to use `.SDcols` that understands variable names :grin:. `.SDcols` controls the behavior of `.SD` (*Subset of Data*). This is one of the most powerful features of `{data.table}`. 


```r
head(dt[ , .SD, .SDcols = select_cols])
```

```
##     mpg cyl
## 1: 21.0   6
## 2: 21.0   6
## 3: 22.8   4
## 4: 21.4   6
## 5: 18.7   8
## 6: 18.1   6
```


There is a final actor in the place that you need to know: the function `get`. This is a base function that returns the value of a named object. Clearly about evaluation, right ? :wink: Instead of calling `dt[,x]`, we will call `dt[,get("x")]`. For instance,


```r
head(dt[ , .("mpg" = get("mpg"),"cyl" = get("cyl"))])
```

```
##     mpg cyl
## 1: 21.0   6
## 2: 21.0   6
## 3: 22.8   4
## 4: 21.4   6
## 5: 18.7   8
## 6: 18.1   6
```

In the context of selecting subset of columns, I think `.SD` is the best option. 

`{data.table}`: 1 ; `{dplyr}`: 0 :soccer:. `select` was the simplest case in `{dplyr}`. What happens with computations equivalent to `group_by`, `mutate` or `filter` ?


Well, to create new columns, we use `:=` based on modification in place. This is the coolest feature of `{data.table}` making it faster and less demanding in memory than any other solution based on copy (e.g. `dplyr::mutate`). If you don't know the `:=` verb, I recommend you the `{data.table}` vignettes. 

:exclamation: Using `{data.table}` within a function requires an extra-precaution if you don't want to modify the input dataframe. Because `{data.table}` is based on modification in place, a function's environment is not isolated from the global environment. Within a function, you don't modify a copy of the input data but directly the data in the global environment. To avoid that, we will start all functions with a copy of the input object using `data.table::copy`. 

The `{data.table}` version of `new_variable` takes the following form:


```r
new_variable <- function(data, xvar = "mpg", newname = "x"){
  datanew <- data.table::copy(data)
  return(datanew[, c(newname) := get(xvar)])
}
head(new_variable(dt, newname = "newvar"))
```

```
##     mpg cyl disp  hp drat    wt  qsec vs am gear carb newvar
## 1: 21.0   6  160 110 3.90 2.620 16.46  0  1    4    4   21.0
## 2: 21.0   6  160 110 3.90 2.875 17.02  0  1    4    4   21.0
## 3: 22.8   4  108  93 3.85 2.320 18.61  1  1    4    1   22.8
## 4: 21.4   6  258 110 3.08 3.215 19.44  1  0    3    1   21.4
## 5: 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2   18.7
## 6: 18.1   6  225 105 2.76 3.460 20.22  1  0    3    1   18.1
```

Note that I added an argument to also programmatically set the new variable name, which we did not with `{dplyr}` :muscle:. The syntax is more concise and easier to understand than in the `{dplyr}`'s case. By the way, we use `c(newname)` to set the new column name to force SE and ensure that the new column will not be called `newname`. 

> What happens when we want to do that for several variables ? 

We get ambitious now! :nerd_face:. This is not that much harder with the help of `.SD` :


```r
new_variables <- function(data, xvars = c("mpg","cyl"),
                          newnames = c("x1","x2")){
  datanew <- data.table::copy(data)
  return(datanew[, c(newnames) := .SD, .SDcols = xvars])
}
head(new_variables(dt))
```

```
##     mpg cyl disp  hp drat    wt  qsec vs am gear carb   x1 x2
## 1: 21.0   6  160 110 3.90 2.620 16.46  0  1    4    4 21.0  6
## 2: 21.0   6  160 110 3.90 2.875 17.02  0  1    4    4 21.0  6
## 3: 22.8   4  108  93 3.85 2.320 18.61  1  1    4    1 22.8  4
## 4: 21.4   6  258 110 3.08 3.215 19.44  1  0    3    1 21.4  6
## 5: 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2 18.7  8
## 6: 18.1   6  225 105 2.76 3.460 20.22  1  0    3    1 18.1  6
```

Another point in favor of `{data.table}` :basketball:.

> What about `by` operations ? 

`by` argument natively accepts strings. Thus, if we want to translate the `new_variable_group` function in `{data.table}`, we can just add `by = grouping_var` in column names. Let's see that with the `{data.table}` version of new_variable_group :


```r
new_variable_group <- function(data, grouping_var = c("cyl","vs"),
                               xvar = "mpg", newname = "x"){
  
  datanew <- data.table::copy(data)
  return(datanew[, c(newname) := mean(get(xvar)),
                 by = grouping_var])
}
head(new_variable_group(dt, newname = "newvar"))
```

```
##     mpg cyl disp  hp drat    wt  qsec vs am gear carb   newvar
## 1: 21.0   6  160 110 3.90 2.620 16.46  0  1    4    4 20.56667
## 2: 21.0   6  160 110 3.90 2.875 17.02  0  1    4    4 20.56667
## 3: 22.8   4  108  93 3.85 2.320 18.61  1  1    4    1 26.73000
## 4: 21.4   6  258 110 3.08 3.215 19.44  1  0    3    1 19.12500
## 5: 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2 15.10000
## 6: 18.1   6  225 105 2.76 3.460 20.22  1  0    3    1 19.12500
```

> What if we are even more ambitious and want to do that operation for several columns ? 

Once again, `.SD` will help. This time, we will use `lapply` function to call `mean` on several columns:


```r
new_variables_group <- function(data, grouping_var = c("cyl","vs"),
                                xvars = c("mpg", "disp"),
                                newnames = c("x1","x2")){
  
  datanew <- data.table::copy(data)
  return(
    datanew[, c(newnames) := lapply(.SD, mean),
            by = grouping_var,
            .SDcols = xvars]
  )
}
head(new_variables_group(dt))
```

```
##     mpg cyl disp  hp drat    wt  qsec vs am gear carb       x1     x2
## 1: 21.0   6  160 110 3.90 2.620 16.46  0  1    4    4 20.56667 155.00
## 2: 21.0   6  160 110 3.90 2.875 17.02  0  1    4    4 20.56667 155.00
## 3: 22.8   4  108  93 3.85 2.320 18.61  1  1    4    1 26.73000 103.62
## 4: 21.4   6  258 110 3.08 3.215 19.44  1  0    3    1 19.12500 204.55
## 5: 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2 15.10000 353.10
## 6: 18.1   6  225 105 2.76 3.460 20.22  1  0    3    1 19.12500 204.55
```

:tennis: Game, set and match !


# Conclusion

Using variable names is very useful when you do not want to repeat yourself. With `{data.table}`, it is very easy to write concise programs that take variable names as input. You should definitely think about it when writing a package :package: !  

