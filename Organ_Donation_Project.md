Organ_Donation_Project
================

## GitHub Documents

This is an R Markdown format used for publishing markdown documents to
GitHub. When you click the **Knit** button all R code chunks are run and
a markdown file (.md) suitable for publishing to GitHub is generated.

## Including Code

You can include R code in the document as follows:

``` r
#I loaded my data-set into R to work on it
organ_donation <- read.csv("C:/Users/gayat/Downloads/Organ Donation.csv")

#I erased the names to make it anonymous
organ_donation$Name <- NULL

library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
#I wanted to get unique values, but nothing really changed here 
#because the data was already unique as it has a lot of variables.
clean <- distinct(organ_donation)

#I removed the "years" in Age to allow it to perform other actions 
organ_donation$Age <- gsub("years", "", organ_donation$Age)
class(organ_donation$Age)
```

    ## [1] "character"

``` r
#I couldn't convert it into numeric as it had a range, so I changed it to factor.
organ_donation$Age <- as.factor(organ_donation$Age)

library(ggplot2)

#Is Age related to the willingness of donating organs? I stacked it based on gender.
#I preferred a bar chart here to showcase the significant nominal comparision
#in my variables.
organ_donation %>% 
  group_by(Age, Are.you.willing.to.donate.organs., Gender) %>% 
  summarize(count = n()) %>% 
  ggplot(aes(Age, count, fill = Gender)) +
  geom_col(position = "dodge") +
  facet_wrap(vars(Are.you.willing.to.donate.organs.)) +
  theme_minimal()
```

    ## `summarise()` has grouped output by 'Age', 'Are.you.willing.to.donate.organs.'.
    ## You can override using the `.groups` argument.

![](Organ_Donation_Project_files/figure-gfm/cars-1.png)<!-- -->

``` r
#Does knowledge about organ donation depend on education?
#I used a mosaic plot here, thanks to you, now I know which plot to go to
#when I have 2 categorical variables. 
#the mosaic plot is really efficient in that sense.
mosaicplot(table(organ_donation$Education, organ_donation$Are.you.aware.of.the.process.to.become.an.organ.donor.),
           color = c("purple", "yellow"),  # Custom colors for Education levels
           main = "Mosaic Plot: Awareness of Organ Donation Process by Education",
           xlab = "Education",
           ylab = "Are.you.aware.of.the.process.to.become.an.organ.donor.",
           legend = TRUE)
```

    ## Warning: In mosaicplot.default(table(organ_donation$Education, organ_donation$Are.you.aware.of.the.process.to.become.an.organ.donor.), 
    ##     color = c("purple", "yellow"), main = "Mosaic Plot: Awareness of Organ Donation Process by Education", 
    ##     xlab = "Education", ylab = "Are.you.aware.of.the.process.to.become.an.organ.donor.", 
    ##     legend = TRUE) :
    ##  extra argument 'legend' will be disregarded

![](Organ_Donation_Project_files/figure-gfm/cars-2.png)<!-- -->

``` r
#Do people prefer to die at home than donating organs?
#It is the question I wanted to answer
ggplot(organ_donation, aes(x = I.prefer.to.die.at.home.and.therefore.choose.not.to.donate.organs.))+
geom_density() + 
theme_minimal()
```

![](Organ_Donation_Project_files/figure-gfm/cars-3.png)<!-- -->
