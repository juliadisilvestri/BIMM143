# Class05: Data Vis with ggplot
Julia (PID: A16950824)

# Graphics systems in R

There are many graphics systems in R for making plots and figures.

We have already played a little bit with **“base R”** graphics and the
`plot()` function.

Today we will start learning about a popular graphics package called
`ggplot2()`

This is an add package – i.e. we need to install it. I install it (like
I install any package) with the `install.packages()` function.

``` r
plot(cars)
```

![](Class05_files/figure-commonmark/unnamed-chunk-1-1.png)

Before I can use the functions from a package I have to look up the
package from my “library”. We use the `library(ggplot2)` command to load
it up.

``` r
#install.packages(ggplot2)
library("ggplot2")
ggplot(cars)
```

![](Class05_files/figure-commonmark/unnamed-chunk-2-1.png)

Every ggplot is made up of at least 3 things: - data (the numbers etc.
that will go into your plot) - aes (how the columns of data map to the
plot aesthetics) - geoms (how the plot actually looks, points, bars,
lines, ect.)

``` r
ggplot(cars) + aes(x=speed, y=dist) + geom_point()
```

![](Class05_files/figure-commonmark/unnamed-chunk-3-1.png)

For simple plots ggplot is more verbose - it takes more code - than base
R plot.

Add some more layers to our ggplot:

``` r
ggplot(cars) + aes(x=speed, y=dist) + geom_point() + geom_smooth(se=F) + labs(title="Stopping Distance of Old Cars", subtitle = "A Silly Example Plot") + theme_bw()
```

    `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](Class05_files/figure-commonmark/unnamed-chunk-4-1.png)

Calling up **Gene Data Set** from class hand-out

``` r
url <- "https://bioboot.github.io/bimm143_S20/class-material/up_down_expression.txt"
genes <- read.delim(url)
head(genes)
```

            Gene Condition1 Condition2      State
    1      A4GNT -3.6808610 -3.4401355 unchanging
    2       AAAS  4.5479580  4.3864126 unchanging
    3      AASDH  3.7190695  3.4787276 unchanging
    4       AATF  5.0784720  5.0151916 unchanging
    5       AATK  0.4711421  0.5598642 unchanging
    6 AB015752.4 -3.6808610 -3.5921390 unchanging

Getting information from data set

``` r
nrow(genes)
```

    [1] 5196

``` r
ncol(genes)
```

    [1] 4

``` r
colnames(genes)
```

    [1] "Gene"       "Condition1" "Condition2" "State"     

``` r
num_upreg<-sum(genes$State == "up")
num_upreg
```

    [1] 127

``` r
(num_upreg) / (nrow(genes)) * 100
```

    [1] 2.444188

Making a simple plot of “Genes”

``` r
ggplot(genes) + aes(x=Condition1, y=Condition2) + geom_point()
```

![](Class05_files/figure-commonmark/unnamed-chunk-7-1.png)

Adding some complexities to the plot

``` r
gene_plot<-ggplot(genes) + aes(x=Condition1, y=Condition2, col=State) + geom_point()
gene_plot + scale_colour_manual( values=c("purple", "gray", "green") )
```

![](Class05_files/figure-commonmark/unnamed-chunk-8-1.png)

Adding Labels to graph

``` r
gene_plot + labs(title="Gene Expression Changes Upon Drug Treatment", x="Control (no drug)", y="Drug Treatment") + scale_colour_manual( values=c("purple", "gray", "green") )
```

![](Class05_files/figure-commonmark/unnamed-chunk-9-1.png)

Installed `gapminder` and `dplyr` packages

``` r
# install.packages(gapminder)
# install.packages(dplyr)
library(gapminder)
library(dplyr)
```


    Attaching package: 'dplyr'

    The following objects are masked from 'package:stats':

        filter, lag

    The following objects are masked from 'package:base':

        intersect, setdiff, setequal, union

Loading in **Gapminder Data Set**

``` r
gapminder_2007 <- gapminder %>% filter(year==2007)
gapminder_2007
```

    # A tibble: 142 × 6
       country     continent  year lifeExp       pop gdpPercap
       <fct>       <fct>     <int>   <dbl>     <int>     <dbl>
     1 Afghanistan Asia       2007    43.8  31889923      975.
     2 Albania     Europe     2007    76.4   3600523     5937.
     3 Algeria     Africa     2007    72.3  33333216     6223.
     4 Angola      Africa     2007    42.7  12420476     4797.
     5 Argentina   Americas   2007    75.3  40301927    12779.
     6 Australia   Oceania    2007    81.2  20434176    34435.
     7 Austria     Europe     2007    79.8   8199783    36126.
     8 Bahrain     Asia       2007    75.6    708573    29796.
     9 Bangladesh  Asia       2007    64.1 150448339     1391.
    10 Belgium     Europe     2007    79.4  10392226    33693.
    # ℹ 132 more rows

Making a basic scatter plot with data

``` r
ggplot(gapminder_2007) + aes(x=gdpPercap, y=lifeExp) + geom_point()
```

![](Class05_files/figure-commonmark/unnamed-chunk-12-1.png)

Making points slightly transparent because many of them are overlapping

``` r
ggplot(gapminder_2007) + aes(x=gdpPercap, y=lifeExp) + geom_point(alpha=0.5)
```

![](Class05_files/figure-commonmark/unnamed-chunk-13-1.png)

Adding more variables

``` r
ggplot(gapminder_2007) + aes(x=gdpPercap, y=lifeExp, col=continent, size=pop) + geom_point(alpha=0.5)
```

![](Class05_files/figure-commonmark/unnamed-chunk-14-1.png)
