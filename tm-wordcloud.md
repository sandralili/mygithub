<br> <br>

Introduction: The Power of Visuals
==================================

According to marketing industry experts, the human brain can process
images up to 60,000 times faster than words. They say that we don’t get
a second chance to make a first impression. This code-through explores
the opportunity to demonstrate that visuals are better than words. We
can imagine presenting information in long/boring reports with just
numbers and numbers. However, we can easily present our data with simple
graphics using the library Word Clouds 2. We will be using the tm
library (text mining) to analyze and clean our data.

<br> <br>

![The Human Brain](C:\Users\sandr\OneDrive\Pictures\brain.jpg) <br> <br>

Content Overview
----------------

The following analysis uses information of the COVID-19 data. The
information contains data recorded daily, with variables as continent,
location (country), new and total cases, new and total deaths,
population, etc.

Learning Objectives
-------------------

Specifically, with this code-through tutorial, we will learn how to
visualizing and communicating statistical models with effect plots using
R Markdown to blend exposition, R code and R output into nice reports
and presentations creating easy-to-read graphics.

Our Data
--------

This table show some of the content of the dataset we are using. We are
going to use the 2019 ASU salaries database.

    URL <- 'https://docs.google.com/spreadsheets/d/1RoiO9bfpbXowprWdZrgtYXG9_WuK3NFemwlvDGdym7E/export?gid=1948400967&format=csv'
    datos <- read.csv( URL )
    head (datos[c(1,2,3,4)],10) %>% knitr::kable()

<table>
<thead>
<tr class="header">
<th style="text-align: right;">Calendar.Year</th>
<th style="text-align: left;">Full.Name</th>
<th style="text-align: left;">Job.Description</th>
<th style="text-align: left;">Department.Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: right;">2019</td>
<td style="text-align: left;">Abadjivor,Enyah</td>
<td style="text-align: left;">Coordinator</td>
<td style="text-align: left;">Research Division 2 Tempe</td>
</tr>
<tr class="even">
<td style="text-align: right;">2019</td>
<td style="text-align: left;">Abbas,James</td>
<td style="text-align: left;">Assoc Professor</td>
<td style="text-align: left;">Sch Biological &amp; Hlth Sys Engr</td>
</tr>
<tr class="odd">
<td style="text-align: right;">2019</td>
<td style="text-align: left;">Abbaszadegan,Morteza</td>
<td style="text-align: left;">Professor</td>
<td style="text-align: left;">Sch Sustain Engr &amp; Built Envrn</td>
</tr>
<tr class="even">
<td style="text-align: right;">2019</td>
<td style="text-align: left;">Abbe,Scott</td>
<td style="text-align: left;">Tech Support Analyst Coord</td>
<td style="text-align: left;">Engineering Technical Services</td>
</tr>
<tr class="odd">
<td style="text-align: right;">2019</td>
<td style="text-align: left;">Abbl,Norma</td>
<td style="text-align: left;">Sr HR Consultant</td>
<td style="text-align: left;">HR Partners</td>
</tr>
<tr class="even">
<td style="text-align: right;">2019</td>
<td style="text-align: left;">Abbott,David</td>
<td style="text-align: left;">Assoc Professor</td>
<td style="text-align: left;">Shesc</td>
</tr>
<tr class="odd">
<td style="text-align: right;">2019</td>
<td style="text-align: left;">Abbott,Joshua</td>
<td style="text-align: left;">Assoc Professor</td>
<td style="text-align: left;">School of Sustainability</td>
</tr>
<tr class="even">
<td style="text-align: right;">2019</td>
<td style="text-align: left;">Abbott,Joshua</td>
<td style="text-align: left;">Dir College of Law</td>
<td style="text-align: left;">College Of Law</td>
</tr>
<tr class="odd">
<td style="text-align: right;">2019</td>
<td style="text-align: left;">Abdalla,Mohamed</td>
<td style="text-align: left;">Senior Program Manager</td>
<td style="text-align: left;">ASU Wrigley Institute</td>
</tr>
<tr class="even">
<td style="text-align: right;">2019</td>
<td style="text-align: left;">Abdollahi,Amir</td>
<td style="text-align: left;">Web Application Developer (IT)</td>
<td style="text-align: left;">DEV Architect</td>
</tr>
</tbody>
</table>

<br> <br>

Libray of Packages
------------------

We will be needing the following libraries:

    # library ( dplyr )  # helps organize our data
    # library ( tm )  # data mining
    # library ( wordcloud )  # creates the wordclouds
    # library ( wordcloud2 )  # creates the wordclouds
    # library ( RColorBrewer ) #  palette colors for cloud

TM: Text Mining
===============

TM Package
----------

The tm package, created by Ingo Feinerer, offers a powerful text data
mining language for depurate complex text information to a nice clean
ready-to-use data. ggplot2 is a plotting package that makes it simple to
create complex plots from data in a data frame. It provides a more
programmatic interface for specifying what variables to plot, how they
are displayed, and general visual properties.

Installing the Package
----------------------

For the tm library, you can type in the R console:
install.packages(“tm”) or you can follow these steps:

![Install Button](C:\Users\sandr\OneDrive\Pictures\tm\tm1.png) ![Type
Name](C:\Users\sandr\OneDrive\Pictures\tm\tm2.png) ![Click
install](C:\Users\sandr\OneDrive\Pictures\tm\tm3.png) <br> <br>

Corpus Transformations
----------------------

TM contains the following functions:

    getTransformations()

    ## [1] "removeNumbers"     "removePunctuation" "removeWords"      
    ## [4] "stemDocument"      "stripWhitespace"

<br> <br>

Preparing data with the TM function
-----------------------------------

Send the information of the “Full Name” column to the vector “words” to
start the data cleaning

    words <- getwd()
    words <- VCorpus(VectorSource(datos$Department.Description))
    words [[1]][[1]]

    ## [1] "Research Division 2 Tempe"

    words [[2]][[1]]

    ## [1] "Sch Biological & Hlth Sys Engr"

    dataframe <- data.frame(text=unlist(sapply(words, `[`, "content")), 
        stringsAsFactors=F)

<br> <br>

This function removes any punctuation that we do not need for our data
transformation

    words1 <- tm_map(words, removePunctuation)
    words1 [[1]][[1]] 

    ## [1] "Research Division 2 Tempe"

    words1 [[2]][[1]] 

    ## [1] "Sch Biological  Hlth Sys Engr"

<br> <br>

This function removes numbers from our data

    words2 <- tm_map(words1, removeNumbers)
    words2 [[1]][[1]]

    ## [1] "Research Division  Tempe"

    words2 [[2]][[1]]

    ## [1] "Sch Biological  Hlth Sys Engr"

<br> <br>

This function change our data to lower caps

    words3 <- tm_map(words2, content_transformer(tolower))
    words3 [[1]][[1]]

    ## [1] "research division  tempe"

    words3 [[2]][[1]]

    ## [1] "sch biological  hlth sys engr"

<br> <br>

This function returns various kinds of stopwords with support for
different languages, in this case “English”

    words4 <- tm_map(words3, removeWords, stopwords ("english"))
    words4 [[1]][[1]]

    ## [1] "research division  tempe"

    words4 [[2]][[1]]

    ## [1] "sch biological  hlth sys engr"

<br> <br>

This function removes white spaces from our data

    words6 <- tm_map(words4, stripWhitespace)
    words6 [[1]][[1]]

    ## [1] "research division tempe"

    words6 [[2]][[1]]

    ## [1] "sch biological hlth sys engr"

<br> <br>

This function changes our data structure. Text mining packages work with
a document-term matrix (or DTM)

    dtm <- DocumentTermMatrix(words6)
    dtm [[1]][[1]]

    ## [1] 1

<br> <br>

Now, our data is ready for our word cloud graphic. <br> <br>

Word Cloud
==========

Wordcloud Package
-----------------

The wordcloud package, created by Ian Fellows, creates pretty word
clouds, visualize differences and similarity between documents, and
avoid over-plotting in scatter plots with text.

Installing the Package
----------------------

For the tm library, you can type in the R console:
install.packages(“wordcloud”) or you can follow these steps:

![Install Button](C:\Users\sandr\OneDrive\Pictures\wc\wc1.png) ![Type
Name](C:\Users\sandr\OneDrive\Pictures\wc\wc2.png) ![Click
install](C:\Users\sandr\OneDrive\Pictures\wc\wc3.png)

Arguments
---------

words - the words

freq - their frequencies

scale - A vector of length 2 indicating the range of the size of the
words.

min.freq - words with frequency below min.freq will not be plotted

max.words - Maximum number of words to be plotted. least frequent terms
dropped

random.order - plot words in random order. If false, they will be
plotted in decreasing frequency

random.color - choose colors randomly from the colors. If false, the
color is chosen based on the frequency

rot.per - proportion words with 90 degree rotation

colors - color words from least to most frequent

ordered.colors - if true, then colors are assigned to words in order

use.r.layout - if false, then c++ code is used for collision detection,
otherwise R is used

fixed.asp - if TRUE, the aspect ratio is fixed. Variable aspect ratio
only supported if rot.per==0

… - Additional parameters to be passed to text (and strheight,strwidth).

Colors
------

The brewer function will help us to choose a palette to be used in out
wordcloud

    color1 = brewer.pal(5, "BrBG")
    color2 = brewer.pal(5, "Set2")
    color3 = brewer.pal(5, "Paired")
    display.brewer.all((colorblindFriendly=TRUE))

    ## [1] "TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE"

![](CPP-527---Code-Through-Perez-2_files/figure-markdown_strict/unnamed-chunk-12-1.png)

Word Cloud Creation,
--------------------

Figure 1 - All words horizontal

    wordcloud(words6, 
              scale=c(5.0, 1), 
              random.order = FALSE, 
              max.words = 75,
              rot.per = 0,
              colors = color1)
    title(main= "Department Description Popular Words",
          cex.main=2)

![](CPP-527---Code-Through-Perez-2_files/figure-markdown_strict/unnamed-chunk-13-1.png)

Figure 2 - Horizontal and Vertical words

    wordcloud(words6, 
              scale=c(5.0,1), 
              random.order = FALSE, 
              max.words = 100,
              rot.per = 0.50,
              colors = color2)
    title(main= "Department Description Popular Words",
          cex.main=2)

![](CPP-527---Code-Through-Perez-2_files/figure-markdown_strict/unnamed-chunk-14-1.png)

Figure 3 - All Words Vertical

    wordcloud(words6, 
              scale=c(5.0, 1), 
              random.order = FALSE, 
              max.words = 150,
              rot.per = 100,
              colors = color3)
    title(main= "Department Description Popular Words",
          cex.main=2)

![](CPP-527---Code-Through-Perez-2_files/figure-markdown_strict/unnamed-chunk-15-1.png)

Further Resources
=================

Learn more about tm() with the following link: [wordcloud: Plot a word
cloud](https://www.rdocumentation.org/packages/wordcloud/versions/2.6/topics/wordcloud)

Learn more about wordcloud() with the following link: [wordcloud: Plot a
word
cloud](https://www.rdocumentation.org/packages/wordcloud/versions/2.6/topics/wordcloud)
<br> <br>

Works Cited
===========

This code through references and cites the following sources:

-   tm package (ND). Source I. [Text Mining
    Package](https://cran.r-project.org/web/packages/tm/index.html)

-   wordcloud package (ND). Source II. [Word
    Clouds](https://cran.r-project.org/web/packages/wordcloud/index.html)

-   Wikipedia (ND). Source III.
    [Wikipedia](https://en.wikipedia.org/wiki/Text_mining)

-   Wikipedia (ND). Source IV.
    [Wikipedia](https://en.wikipedia.org/wiki/Tag_cloud)

-   Linkedin (2021). Source V.
    [Linkedin](https://www.linkedin.com/pulse/whats-60000-times-faster-than-reading-subject-line-alec-sharples/)

<br> <br>
