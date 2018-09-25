Gapminder Exploration
================

First, we load the `gapminder` and `tidyverse` packages:

``` r
library(gapminder)
library(tidyverse)
```

    ## Warning: replacing previous import by 'tibble::as_tibble' when loading
    ## 'broom'

    ## Warning: replacing previous import by 'tibble::tibble' when loading 'broom'

    ## ── Attaching packages ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.0.0     ✔ purrr   0.2.5
    ## ✔ tibble  1.4.2     ✔ dplyr   0.7.6
    ## ✔ tidyr   0.8.1     ✔ stringr 1.3.1
    ## ✔ readr   1.1.1     ✔ forcats 0.3.0

    ## ── Conflicts ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

Smell Test:
-----------

Is it a data.frame, a matrix, a vector, a list?

``` r
typeof(gapminder)
```

    ## [1] "list"

As we can see, `gapminder` is stored in a list.

What is its class?

``` r
class(gapminder)
```

    ## [1] "tbl_df"     "tbl"        "data.frame"

`gapminder` belongs to the classes `tibble` and `data.frame`.

How many variables/columns?

``` r
ncol(gapminder)
```

    ## [1] 6

`gapminder` has 6 columns.

How many rows/observations?

``` r
nrow(gapminder)
```

    ## [1] 1704

There are 1704 observations in `gapminder`

Can you get these facts about "extent" or "size" in more than one way? Can you imagine different functions being useful in different contexts?

``` r
names(gapminder)
```

    ## [1] "country"   "continent" "year"      "lifeExp"   "pop"       "gdpPercap"

This can also be used to find the number of columns in a dataset

What data type is each variable?

``` r
str(gapminder)
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    1704 obs. of  6 variables:
    ##  $ country  : Factor w/ 142 levels "Afghanistan",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ continent: Factor w/ 5 levels "Africa","Americas",..: 3 3 3 3 3 3 3 3 3 3 ...
    ##  $ year     : int  1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
    ##  $ lifeExp  : num  28.8 30.3 32 34 36.1 ...
    ##  $ pop      : int  8425333 9240934 10267083 11537966 13079460 14880372 12881816 13867957 16317921 22227415 ...
    ##  $ gdpPercap: num  779 821 853 836 740 ...

Exploring individual variables
------------------------------

Pick at least one categorical variable and at least one quantitative variable to explore.

categorical variable selected: continent quantitative variable selected: lifeExp

What are possible values (or range, whichever is appropriate) of each variable?

``` r
summary(gapminder$continent)
```

    ##   Africa Americas     Asia   Europe  Oceania 
    ##      624      300      396      360       24

The same information, in bar chart form:

``` r
ggplot(gapminder, aes(continent)) + geom_bar(fill = 'dark green')
```

![](gapminder-explore_files/figure-markdown_github/unnamed-chunk-9-1.png)

``` r
summary(gapminder$lifeExp)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   23.60   48.20   60.71   59.47   70.85   82.60

When we have a continuous variable we get more information from a density plot vs. the summary statistics.

``` r
ggplot(gapminder, aes(lifeExp)) + geom_density(bw = 0.01, fill = "orange") + scale_x_log10()
```

![](gapminder-explore_files/figure-markdown_github/unnamed-chunk-11-1.png)

As expected, we have a large group of countries in the 70s for life expectancy, presumably the Western (i.e. developed) countries.

A fancier plot, illustrating the clustering of countries' life expectancy and gdp per capita by the continent.

``` r
plot = ggplot(gapminder, aes(gdpPercap, lifeExp)) + scale_x_log10()
plot + geom_point(aes(colour = factor(continent)), size = 2)
```

![](gapminder-explore_files/figure-markdown_github/unnamed-chunk-12-1.png)

What values are typical? What's the spread? What's the distribution? Etc., tailored to the variable at hand. Feel free to use summary stats, tables, figures. We're NOT expecting high production value (yet).

Typical values of continent are the five continents: Africa, Americas, Asia, Europe, and Oceania. Note that North, Central, and South America are all mixed into one group.

As expected, typical values of the life expectancy range from 20s to 80s. A large proportion of countries range in the 70s and higher.

Life expectancy distribution according to continent:

``` r
a = ggplot(gapminder, aes(continent, lifeExp))
a+geom_violin()+geom_jitter(alpha=0.2)
```

![](gapminder-explore_files/figure-markdown_github/unnamed-chunk-13-1.png)

Use of filter, select, and %&gt;%
---------------------------------

And the following is a plot of Sudan's GDP per capital and life expectancy over time.

``` r
gapminder %>% 
  filter(country == 'Sudan') %>% 
  select(gdpPercap, lifeExp) %>% 
  ggplot(aes(gdpPercap, lifeExp)) + geom_point() + geom_path(arrow=arrow())
```

![](gapminder-explore_files/figure-markdown_github/unnamed-chunk-14-1.png)

But I want to do more!
----------------------

Evaluate this code and describe the result. Presumably the analyst's intent was to get the data for Rwanda and Afghanistan. Did they succeed? Why or why not? If not, what is the correct way to do this?

code: `filter(gapminder, country == c("Rwanda", "Afghanistan"))` (Unfortunately I can't put it in an `R` chunk because then knit will have a problem with it)

``` r
filter(gapminder, country == c("Rwanda", "Afghanistan"))
```

    ## # A tibble: 12 x 6
    ##    country     continent  year lifeExp      pop gdpPercap
    ##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
    ##  1 Afghanistan Asia       1957    30.3  9240934      821.
    ##  2 Afghanistan Asia       1967    34.0 11537966      836.
    ##  3 Afghanistan Asia       1977    38.4 14880372      786.
    ##  4 Afghanistan Asia       1987    40.8 13867957      852.
    ##  5 Afghanistan Asia       1997    41.8 22227415      635.
    ##  6 Afghanistan Asia       2007    43.8 31889923      975.
    ##  7 Rwanda      Africa     1952    40    2534927      493.
    ##  8 Rwanda      Africa     1962    43    3051242      597.
    ##  9 Rwanda      Africa     1972    44.6  3992121      591.
    ## 10 Rwanda      Africa     1982    46.2  5507565      882.
    ## 11 Rwanda      Africa     1992    23.6  7290203      737.
    ## 12 Rwanda      Africa     2002    43.4  7852401      786.

The problem with this code is that we do not get every row in the dataset. To get it, we use the following code:

``` r
filter(gapminder, country %in% c("Rwanda", "Afghanistan"))
```

    ## # A tibble: 24 x 6
    ##    country     continent  year lifeExp      pop gdpPercap
    ##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
    ##  1 Afghanistan Asia       1952    28.8  8425333      779.
    ##  2 Afghanistan Asia       1957    30.3  9240934      821.
    ##  3 Afghanistan Asia       1962    32.0 10267083      853.
    ##  4 Afghanistan Asia       1967    34.0 11537966      836.
    ##  5 Afghanistan Asia       1972    36.1 13079460      740.
    ##  6 Afghanistan Asia       1977    38.4 14880372      786.
    ##  7 Afghanistan Asia       1982    39.9 12881816      978.
    ##  8 Afghanistan Asia       1987    40.8 13867957      852.
    ##  9 Afghanistan Asia       1992    41.7 16317921      649.
    ## 10 Afghanistan Asia       1997    41.8 22227415      635.
    ## # ... with 14 more rows

Additional Exercises:
---------------------

**Exercise 1**: Make a plot of `year` (x) vs `lifeExp` (y), with points coloured by continent. Then, to that same plot, fit a straight regression line to each continent, without the error bars. If you can, try piping the data frame into the `ggplot` function.

``` r
gapminder %>% 
  ggplot(aes(year, lifeExp, color=continent)) + 
  geom_point() + 
  geom_smooth(method='lm', se = FALSE)
```

![](gapminder-explore_files/figure-markdown_github/unnamed-chunk-17-1.png)

**Exercise 2**: Repeat Exercise 1, but switch the *regression line* and *geom\_point* layers. How is this plot different from that of Exercise 1?

``` r
gapminder %>% 
  ggplot(aes(year, lifeExp, color=continent)) + 
  geom_smooth(method='lm', se = FALSE) +
  geom_point()
```

![](gapminder-explore_files/figure-markdown_github/unnamed-chunk-18-1.png)

**Exercise 3**: Omit the `geom_point` layer from either of the above two plots (it doesn't matter which). Does the line still show up, even though the data aren't shown? Why or why not?

``` r
gapminder %>% 
  ggplot(aes(year, lifeExp, color=continent)) +
  geom_smooth(method='lm', se = FALSE)
```

![](gapminder-explore_files/figure-markdown_github/unnamed-chunk-19-1.png)

**Exercise 4**: Make a plot of `year` (x) vs `lifeExp` (y), facetted by continent. Then, fit a smoother through the data for each continent, without the error bars. Choose a span that you feel is appropriate.

``` r
gapminder %>% 
  ggplot(aes(year, lifeExp)) + 
  facet_wrap(~ continent) + 
  geom_point() + 
  geom_smooth(se = F)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](gapminder-explore_files/figure-markdown_github/unnamed-chunk-20-1.png)

**Exercise 5**: Plot the population over time (year) using lines, so that each country has its own line. Colour by `gdpPercap`. Add alpha transparency to your liking.

``` r
gapminder %>% 
  ggplot(aes(year, pop)) + 
  geom_line(aes(group=country, color=gdpPercap), alpha = 0.23) + 
  scale_y_log10()
```

![](gapminder-explore_files/figure-markdown_github/unnamed-chunk-21-1.png)

**Exercise 6**: Add points to the plot in Exercise 5.

``` r
gapminder %>% 
  ggplot(aes(year, pop)) + 
  geom_line(aes(group=country, color=gdpPercap), alpha = 0.23) + 
  geom_point(alpha = 0.1) + 
  scale_y_log10() 
```

![](gapminder-explore_files/figure-markdown_github/unnamed-chunk-22-1.png)
