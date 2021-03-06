Showing off some ggplot2 skills
================
Team 09: Cyrus Tadjiki and Matt McCoy.
</br>11 April 2021

## Goal and requirements

The goal of this assignment is simple: I want you to produce *two*
different figures using **ggplot2**. They don’t have to be identify
causal relationships, or anything like that. I just want you to stretch
your visualization legs and demonstrate any new (or existing)
**ggplot2** skills that you have acquired since our first lecture.
**Important:** Both team members should contribute equally to this
assignment (e.g. repsosible for one figure each).

Some additional points:

-   You are free to use any dataset that comes in-built with base R, or
    bundled together with an external R package. See
    [here](https://vincentarelbundock.github.io/Rdatasets/datasets.html)
    for an impressive list.

-   That being said, I would especially encourage you to use your own
    data.

    -   I know we haven’t gotten to data importation yet, but take a
        look
        [here](https://support.rstudio.com/hc/en-us/articles/218611977-Importing-Data-with-RStudio)
        if you need help. I would recommend that you install the
        **readr**, **read\_excel** and **haven** packages first, though.
    -   If your dataset isn’t proprietary (or isn’t being read directly
        off the web), please save it in the (empty) `data/` folder of
        this repo.

-   You can use the same dataset for all four of your plots. Or you can
    use a new dataset for each plot. Regardless of what you choose, I
    want you to try and use different geoms for each figure.

-   Any other **ggplot2** skills and add-ons like faceting, changing
    aesthetic scales or legends, using different themes (e.g. from the
    **ggthemes** package), animation, etc. are all welcome and
    encouraged.

-   I want to *see* the code that produces the figures. (Don’t use
    `echo=FALSE` in any of the code chunks, if that means anything to
    you.)

### What you will be graded on

-   Are your figures clear? (e.g. lack of chart chunk, non-overlapping
    labels)
-   Are your figures compelling? (e.g. use an appropriate geom for the
    insight that you want to convey)
-   Variation (I don’t want to see four line charts of the same dataset…
    Be creative)
-   Did you read and follow my instructions (e.g. describe your data and
    figures, show the code that produced the figures, include data in
    the `/data` folder, etc)
-   etc.

Lastly, don’t forget to knit the assignment (click the “Knit” button, or
press `Ctrl+Shift+K`) before submitting!

## Start the assignment

Here is a R code chunk for you to load your packages. Feel free to load
as many other packages and insert as many additional code chunks
(`Ctr+Alt+I`) as you need. You can always load additional packages in
the their own chunk blocks, but I recommend loading all of your packages
together at the top.)

``` r
if (!require("pacman")) install.packages("pacman")
#Install Ecdata if not already installed as it contains the dataset.
if (!require("Ecdat")) install.packages("Ecdat")
#Install gridExtra if not already installed as it contains the dataset.
if (!require("gridExtra")) install.packages("gridExtra")
```

    ## Loading required package: gridExtra

    ## Warning: package 'gridExtra' was built under R version 4.0.5

    ## 
    ## Attaching package: 'gridExtra'

    ## The following object is masked from 'package:dplyr':
    ## 
    ##     combine

``` r
## Note: The `p_load()` function from the pacman package is a convenient way to 
## install (if necessary) and load packages all at once. You can think of this
## as an alternative to the normal `library(ggplot2); library(here);...` way of
## loading R packages.
pacman::p_load(ggplot2, here, Ecdat, dplyr, gridExtra)
```

``` r
#load data
data(Mathlevel)
```

``` r
# quick summary statistics

summary(Mathlevel)
```

    ##  mathlevel       sat        language      sex        major       mathcourse   
    ##  170 :164   Min.   :400.0   no :539   male  :373   other:130   Min.   :0.000  
    ##  171a: 49   1st Qu.:590.0   yes: 70   female:236   eco  :209   1st Qu.:1.000  
    ##  172a: 11   Median :630.0                          oss  :103   Median :1.000  
    ##  171b:228   Mean   :624.8                          ns   :126   Mean   :1.427  
    ##  172b: 42   3rd Qu.:660.0                          hum  : 41   3rd Qu.:2.000  
    ##  221a: 31   Max.   :790.0                                      Max.   :3.000  
    ##  221b: 84                                                                     
    ##   physiccourse    chemistcourse  
    ##  Min.   :0.0000   Min.   :0.000  
    ##  1st Qu.:0.0000   1st Qu.:1.000  
    ##  Median :1.0000   Median :1.000  
    ##  Mean   :0.7077   Mean   :1.053  
    ##  3rd Qu.:1.0000   3rd Qu.:1.000  
    ##  Max.   :2.0000   Max.   :2.000  
    ## 

``` r
head(Mathlevel, n=10)
```

    ##    mathlevel sat language    sex major mathcourse physiccourse chemistcourse
    ## 1        170 670       no   male    ns          1            2             1
    ## 2        170 660       no   male other          1            1             1
    ## 3        170 610       no female   eco          1            0             1
    ## 4        170 620      yes   male   eco          1            0             1
    ## 5        170 430       no   male   eco          0            1             1
    ## 6        170 580       no female   oss          2            1             1
    ## 7        170 550      yes female other          1            0             1
    ## 8        170 510       no female   eco          1            1             1
    ## 9        170 560      yes   male   hum          1            0             0
    ## 10       170 670       no   male   oss          1            0             1

``` r
tail(Mathlevel, n=5)
```

    ##     mathlevel sat language    sex major mathcourse physiccourse chemistcourse
    ## 605      221b 580       no female   oss          2            1             1
    ## 606      221b 770       no   male   oss          2            1             1
    ## 607      221b 660       no   male other          2            1             1
    ## 608      221b 710       no female   eco          2            0             1
    ## 609      221b 590       no female   oss          2            0             1

### **Description of Data:**

We found the data from the Vincent Arel-Bundock github, and it is
located in the Ecdat package. This study collected data on the level of
calculus that had been attained by students taking Advanced
Microeconomics from 1983 to 1986. The students are also categorized by
their major. It appears to have been a fairly small cross-sectional
study as there are only 609 observations.

**Brief note on this data set:** It was very frustrating making plots of
data when the data set “MathLevel” and a important variable “mathlevel”
had the same exact spelling and only on variation of the first letter
being capitalized or not. R wasn’t always sure what todo so I had to
specify with the pipeline %&gt;% occasionally. In the future I would’ve
avoided this by making my own .csv and renaming the columns or mutating
the data within R.

### Fiqure 1: Distribution of forgien langue proficiency and sex with Math performance

``` r
Gender_plot <- ggplot(Mathlevel) +
                aes(y = mathlevel, x = sat, color = sex) +
                geom_jitter() + labs(title="Math Performance and Sex", x = "SAT Math Score", y = "Highest Math Class Taken") +
                scale_color_discrete(name = "Sex", labels = c("Male", "Female")) + 
                theme_dark() +
                theme(legend.position="bottom")

Language_plot <- ggplot(Mathlevel) +
                  aes(y = mathlevel, x = sat, color = language) +
                  geom_jitter() + labs(title="Math Performance and Language", x = "SAT Math Score", y = "Highest Math Class Taken") +
                  scale_color_discrete(name = "Foreign Language", labels = c("No", "Yes")) + 
                  theme_dark() +
                  theme(legend.position="bottom")
```

``` r
grid.arrange(Gender_plot, Language_plot, ncol = 2)
```

![](ggplot2_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

## Description of **Figure 1**:

I plotted two side-by-side geom\_jitter() plots to look and see if there
is a trend between SAT Math Scores and highest level of math attained in
College. I further more duplicated the plots and colored sex in the
first one and foreign language proficiency in the other to see if these
binary variables are clustered towards the top or bottom of these
jitters. My parents really pushed me to take Spanish in middle school
and told me it would help me be better rounded at math because the use
the same hemisphere of the brain. I though that was nonsense and my
plots help support that it was nonsense showing no cluster in language.
Additionally, there are no clusters with Sex and math performance. I
believe the reason we get three distributed chunks of observations is
because of the different requirements of math for specific majors.
Non-STEM majors most likely belong in the lowest chunk and Economics and
Natural Science Majors most likely belong in the highest chunk due to
those majors requiring the highest math requirements.

### Figure 2: Distribution of SAT Math scores By Major

``` r
# Plot code here

ggplot(Mathlevel) +
  aes(x = sat, y = major, fill = major) +
  geom_boxplot(outlier.color="red") + labs(title = "SAT Math Score Distribution By Major", x = "SAT Math Score", y = "Major", fill = "Major") 
```

![](ggplot2_files/figure-gfm/fig2-1.png)<!-- -->

## Description of **Figure 2**:

This Boxplot depicts the distribution of SAT math scores within each of
the majors. I was curious to see if there would be any correlation
between specific majors and higher math scores. Some majors are much
more reliant on math than others, so I would expect these majors to put
off students who have historically have not done well in math. It does
not appear that math scores influence what major someone picks as each
major has a very similar distribution and interquartile range. The
Natural Sciences does have an edge on the upper whisker indicating that
their top 25% of scores are slightly higher. The Economics and Other
majors had a slightly worse lower whisker score indicating there are
slightly more low scores. The outliers are highlighted as a red dot.
It’s interesting that there are no outliers in the Humanities major, and
they have the most compact distribution of scores. There are 11 outliers
which makes up 1.8% of observations. I would expect the majors that
don’t require much math to have more people with low scores. The spread
of math scores is fairly symmetrical between group, and the data is not
greatly skewed in either direction. The humanities and econ majors are
slightly skewed left though. All the majors have medians that appear to
be within 10-20 points and the same goes for the interquartile range.
