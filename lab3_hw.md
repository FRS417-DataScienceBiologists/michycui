---
title: "Lab 3 Homework"
author: "Michelle Cui"
date: "Winter 2019"
output:
  html_document:
    theme: spacelab
    toc: no
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code, keep track of your versions using git, and push your final work to our [GitHub repository](https://github.com/FRS417-DataScienceBiologists). I will randomly select a few examples of student work at the start of each session to use as examples so be sure that your code is working to the best of your ability.

## Load the libraries
```{r message=FALSE, warning=FALSE}
library(tidyverse)
```

For this assignment we are going to work with a large dataset from the [United Nations Food and Agriculture Organization](http://www.fao.org/about/en/) on world fisheries. The data are messy, so for this assignment I am going to provide some help. The code I use will likely be useful in the future so keep it handy. First, load the data. **Read** the error message.  

```{r message=TRUE, warning=TRUE}
fisheries <- readr::read_csv(file = "FAO_1950to2012_111914.csv") 
```

1. Do you see any potential problems with the column names? Does the error message now make more sense?  
*Column names are duplicated so R manipulated them to unique names*

```{r}
names(fisheries)
```



2. The `make.names()` command is helpful when there are issues with column names. Notice that although the names are still cumbersome, much of the problemtatic syntax is removed.
```{r}
names(fisheries) = make.names(names(fisheries), unique=T) #changes the column names
names(fisheries)
```

3. Let's rename the columns. Use `rename()` to adjust the names as follows. Double check to make sure the rename worked correctly. Make sure to replace the old fisheries object with a new one so you can keep the column names.
+ country     = Country..Country.  
+ commname    = Species..ASFIS.species.  
+ sciname     = Species..ASFIS.species._2  
+ spcode      = Species..ASFIS.species._1  
+ spgroup     = Species..ISSCAAP.group.  
+ spgroupname = Species..ISSCAAP.group._1  
+ region      = Fishing.area..FAO.major.fishing.area.  
+ unit        = Measure..Measure. 

```{r}
fisheries %>%
  rename( country = Country..Country.,
          commname = Species..ASFIS.species.,
          sciname = Species..ASFIS.species._2,
          spcode = Species..ASFIS.species._1,
          spgroup = Species..ISSCAAP.group.,
          spgroupname = Species..ISSCAAP.group._1,
          region = Fishing.area..FAO.major.fishing.area.,
          unit = Measure..Measure.)
fisheries
```


4. Are these data tidy? Why or why not, and, how do you know?

No because column names are just the values of a variable.


5. We need to tidy the data using `gather()`. The code below will not run because it is commented (#) out. I have added a bit of code that will prevent you from needing to type in each year from 1950-2012 but you need to complete the remainder `QQQ` and remove the `#`.
```{r}
fisheries_tidy <- 
  fisheries %>% 
  gather(num_range('X',1950:2012), key='year', value='catch')
```

6. Use `glimpse()` to look at the categories of the variables. Pay particular attention to `year` and `catch`. What do you notice?  

```{r}
glimpse(fisheries_tidy)
```



7. From question 6 you should see that there are a lot of entries that are missing. In R, these are referred to as NA's but they can be coded in different ways by the scientists in a given study. In order to make the data tidy, we need to deal with them. As a preview to our next lab, run the following code by removing the `#`. It removes the 'X' from the years and changes the `catch` column from a character into a numeric. This forces the blank entries to become NAs. The error "NAs introduced by coercion" indicates their replacement.
```{r}
fisheries_tidy <- 
  fisheries_tidy %>% 
  mutate(
    year= as.numeric(str_replace(year, 'X', '')), #remove the X from year
    catch= as.numeric(str_replace(catch, c(' F','...','-'), replacement = '')) #replace character strings with NA
    )
```

8. Are the data tidy? Why?  

```{r}
fisheries_tidy
```



9. You are a fisheries scientist studying cephalopod catch during 2008-2012. Identify the top five consumers (by country) of cephalopods (don't worry about species for now). Restrict the data frame only to our variables of interest.

```{r}
ff <- fisheries_tidy %>%
  select(Country..Country., Species..ISSCAAP.group._1, Species..ASFIS.species., catch, year) %>%
  filter(year == 2008 & year == 2012 | year > 2008 & year < 2012)

ff
```



10. Let's be more specific. Who consumes the most `Common cuttlefish`? Store this as a new object `cuttle`.

```{r}
cuttle <- ff %>%
  filter(Species..ASFIS.species.=="Common cuttlefish")

cuttle
```



## Push your final code to [GitHub](https://github.com/FRS417-DataScienceBiologists)
Make sure that you push your code into the appropriate folder. Also, be sure that you have check the `keep md` file in the knit preferences.