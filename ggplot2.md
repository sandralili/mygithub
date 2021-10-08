<br> <br>

Content Overview
================

The following analysis uses information of the COVID-19 data. The
information contains data recorded daily, with variables as continent,
location (country), new and total cases, new and total deaths,
population, etc.

![World Map](C:\Users\sandr\OneDrive\Pictures\world-map.png)

<br> <br>

Learning Objectives
-------------------

Specifically, with this code-through tutorial, we will learn how to
visualizing and communicating statistical models with effect plots using
R Markdown to blend exposition, R code and R output into nice reports
and presentations creating easy-to-read graphics.

<br> <br>

Installing the Package
----------------------

The easiest way to get ggplot2 is to install the whole tidyverse:
install.packages(“tidyverse”)

Alternatively, install just ggplot2: install.packages(“ggplot2”)

Or the development version from GitHub: install.packages(“devtools”)

devtools::install\_github(“tidyverse/ggplot2”)

Sometimes, it is easier to follow visual instrucions, therefore there is
a great YouTube video that will guide us to install this package, step
by step: \* [ggplot2
Tutorial](https://www.youtube.com/watch?v=hr2X7rmkprM)

<br> <br>

ggplot2
-------

The ggplot2 package, created by Hadley Wickham, offers a powerful
graphics language for creating elegant and complex plots. ggplot2 is a
plotting package that makes it simple to create complex plots from data
in a data frame. It provides a more programmatic interface for
specifying what variables to plot, how they are displayed, and general
visual properties.

This package contains many functions to create differnt types of
graphics. These ones are very popular: – geom\_bar() - to create bar
plots

– geom\_line() - to create line plots

– geom\_density() - to create a density chart

– geom\_point() - the point geom is used to create scatterplots

Rstudio created Cheat Sheet with information about Data Visualization
with ggplot2. We can visit this page: \*
[ggplot2-cheatsheet](https://rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf)

<br> <br>

Our Data
========

This table show some of the content of the dataset we are using. We will
be using the columns Continent, date, new\_cases, and new\_deaths
mainly.

    head (new.table.2)

    ## # A tibble: 6 x 65
    ##   iso_code continent location    date       total_cases new_cases new_cases_smoot~
    ##   <chr>    <chr>     <chr>       <date>           <dbl>     <dbl>            <dbl>
    ## 1 AFG      Asia      Afghanistan 2020-02-24           5         5           NA    
    ## 2 AFG      Asia      Afghanistan 2020-02-25           5         0           NA    
    ## 3 AFG      Asia      Afghanistan 2020-02-26           5         0           NA    
    ## 4 AFG      Asia      Afghanistan 2020-02-27           5         0           NA    
    ## 5 AFG      Asia      Afghanistan 2020-02-28           5         0           NA    
    ## 6 AFG      Asia      Afghanistan 2020-02-29           5         0            0.714
    ## # ... with 58 more variables: total_deaths <dbl>, new_deaths <dbl>,
    ## #   new_deaths_smoothed <dbl>, total_cases_per_million <dbl>,
    ## #   new_cases_per_million <dbl>, new_cases_smoothed_per_million <dbl>,
    ## #   total_deaths_per_million <dbl>, new_deaths_per_million <dbl>,
    ## #   new_deaths_smoothed_per_million <dbl>, reproduction_rate <dbl>,
    ## #   icu_patients <dbl>, icu_patients_per_million <dbl>, hosp_patients <dbl>,
    ## #   hosp_patients_per_million <dbl>, weekly_icu_admissions <dbl>, ...

<br> <br>

Example 1: Table vs Bars Graphic
--------------------------------

We group our information by continent to find out how many cases per
continent. We use the commands group\_by and summarize.

    # New cases and new deaths per continent
    new.table.2 %>%
      
     group_by (continent) %>%
      
     summarize (new.cases = round(sum(new_cases, na.rm = TRUE)), 
                 new.deaths = round(sum(new_deaths, na.rm = TRUE)))

    ## # A tibble: 6 x 3
    ##   continent     new.cases new.deaths
    ##   <chr>             <dbl>      <dbl>
    ## 1 Africa          8374299     213212
    ## 2 Asia           75927962    1138687
    ## 3 Europe         60745285    1242672
    ## 4 North America  53119972    1074040
    ## 5 Oceania          200415       2326
    ## 6 South America  37776098    1146660

<br> <br>

Now the same information but using a visual, no doubt that it is easier
to understand! We will be using the ggplot() and geom\_bar() functions.
Fill will be determined by the continent variable.

    # New cases per continent bars graphic

    new.table.2 %>%

     group_by (continent) %>%
      
     summarize(total.cases = sum(new_cases, na.rm = TRUE), 
                total.deaths = sum(new_deaths, na.rm = TRUE), .groups = 'drop')

    ## # A tibble: 6 x 3
    ##   continent     total.cases total.deaths
    ##   <chr>               <dbl>        <dbl>
    ## 1 Africa            8374299       213212
    ## 2 Asia             75927962      1138687
    ## 3 Europe           60745285      1242672
    ## 4 North America    53119972      1074040
    ## 5 Oceania            200415         2326
    ## 6 South America    37776098      1146660

     ggplot (new.table.2, 
             aes(x = continent, 
             fill = continent)) + 
             geom_bar()

![](ggplot2_files/figure-markdown_strict/unnamed-chunk-4-1.png)

<br> <br>

Example 2: Table vs Plot
------------------------

We group our information by date and continent to find out how many each
continenthad every day. We use the commands group\_by and summarize
again.

    # New cases per date and continent lines graphic 

    new.table.2 %>%
      
      group_by (date, continent) %>%
      
      summarize (cases.per.date  = mean (new_cases), .groups = 'drop') 

    ## # A tibble: 3,790 x 3
    ##    date       continent     cases.per.date
    ##    <date>     <chr>                  <dbl>
    ##  1 2020-01-01 North America             NA
    ##  2 2020-01-01 South America             NA
    ##  3 2020-01-02 North America             NA
    ##  4 2020-01-02 South America             NA
    ##  5 2020-01-03 North America             NA
    ##  6 2020-01-03 South America             NA
    ##  7 2020-01-04 Asia                      NA
    ##  8 2020-01-04 North America             NA
    ##  9 2020-01-04 South America             NA
    ## 10 2020-01-05 Asia                      NA
    ## # ... with 3,780 more rows

<br> <br>

Again the same information from the table but using a visual. We will be
using the ggplot() again, the geom\_line(), and the geom\_point()
functions. The color of the lines and points will be determined by the
continent variable.

    # New cases per date and continent lines graphic 

    new.table.2 %>%
      
      group_by (date, continent) %>%
      
      summarize (cases.per.date  = mean (new_cases), .groups = 'drop') %>%

      ggplot (aes(x = date, 
                  y = cases.per.date, 
                  color = continent)) +
      
      geom_line (size=1) + 
       
      geom_point (size=1.5)

![](ggplot2_files/figure-markdown_strict/unnamed-chunk-6-1.png)

<br> <br>

A Dynamic Graphic
-----------------

The ggplot() allows us to make a dynamic visualization. We just need to
use the transition\_reveal() function with the date variable. Using the
ggplot() will make an elegant graphic!

    # New cases per date and continent lines dynamic graphic

    new.table.2 %>%
      
      group_by (date, continent) %>%
      
      summarize (cases.per.date  = mean (new_cases), .groups = 'drop') %>%

      ggplot (aes(x = date, 
                  y = cases.per.date, 
                  color = continent)) +
      
      geom_line (size=1) + 
      
      transition_reveal(date)

![](ggplot2_files/figure-markdown_strict/unnamed-chunk-7-1.gif)

    #  geom_point (size=1.5)

<br> <br>

Example 3: Another Cool Graphic
-------------------------------

Another way to present information using the ggplot() is by using the
function geom\_density(). We want to find the average of the population
older than 65 years per continent. We group the information and display
it using a table. Then we create the graphic and we can notice that the
visualization is very simple and easier to understand!

    # density graphic 


    new.table.2 %>%
      
      group_by (continent, aged_65_older) %>%
      
      summarize (average.aged.65.older  = mean (aged_65_older), .groups = 'drop') 

    ## # A tibble: 194 x 3
    ##    continent aged_65_older average.aged.65.older
    ##    <chr>             <dbl>                 <dbl>
    ##  1 Africa             2.17                  2.17
    ##  2 Africa             2.34                  2.34
    ##  3 Africa             2.40                  2.40
    ##  4 Africa             2.41                  2.41
    ##  5 Africa             2.48                  2.48
    ##  6 Africa             2.49                  2.49
    ##  7 Africa             2.52                  2.52
    ##  8 Africa             2.54                  2.54
    ##  9 Africa             2.55                  2.55
    ## 10 Africa             2.56                  2.56
    ## # ... with 184 more rows

    # density graphic 


    new.table.2 %>%
      
      group_by (continent, aged_65_older) %>%
      
      summarize (average.aged.65.older  = mean (aged_65_older), .groups = 'drop') %>%

      ggplot (aes(x = average.aged.65.older, 
               fill = continent)) +
        
      geom_density(alpha=0.2)

![](ggplot2_files/figure-markdown_strict/unnamed-chunk-9-1.png)

<br> <br>

Further Resources
=================

Learn more about ggplot() with the following link: [ggplot2: Create
Elegant Data Visualisations Using the Grammar of
Graphics](https://cran.r-project.org/web/packages/ggplot2/index.html)

<br> <br>

-   Resource I [Coronavirus Source
    Data](https://ourworldindata.org/coronavirus-source-data) <br> <br>

Works Cited
===========

This code through references and cites the following sources:

<br> <br>

-   DeltaDNA (2015). Source I. [Gorgeous graphs with
    ggplot2](https://www.youtube.com/watch?v=rsG-GgR0aEY)

-   Earth Lab (2021). Source II. [Earth Data Analytics Online
    Certificate](https://www.earthdatascience.org/courses/earth-analytics/document-your-science/add-images-to-rmarkdown-report/)

<br> <br>
