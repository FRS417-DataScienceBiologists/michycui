---
title: "Lab 5 Homework"
author: "Michelle Cui"
date: "Winter 2019"
output:
  html_document:
    theme: spacelab
    keep_md: true
    toc: no
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code, keep track of your versions using git, and push your final work to our [GitHub repository](https://github.com/FRS417-DataScienceBiologists). I will randomly select a few examples of student work at the start of each session to use as examples so be sure that your code is working to the best of your ability.

## Load the tidyverse
```{r message=FALSE, warning=FALSE}
library(tidyverse)
```

## Mammals Life History
Let's revisit the mammal life history data to practice our `ggplot` skills. Some of the tidy steps will be a repeat from the homework, but it is good practice. The [data](http://esapubs.org/archive/ecol/E084/093/) are from: *S. K. Morgan Ernest. 2003. Life history characteristics of placental non-volant mammals. Ecology 84:3402.*

1. Load the data.

```{r}
mammals <- readr::read_csv("mammal_lifehistories_v2.csv")
```

2. Use your preferred function to have a look. Do you notice any problems?

```{r}
glimpse(mammals)
```



3. There are NA's. How are you going to deal with them?

```{r}
mammals <-
  mammals %>%
  na_if("-999")
mammals
```



4. Where are the NA's? This is important to keep in mind as you build plots.

```{r}
mammals %>%
  purrr::map_df(~ sum(is.na(.))) %>% 
  tidyr::gather(variables, num_nas) %>% 
  arrange(desc(num_nas))
```


5. Some of the variable names will be problematic. Let's rename them here as a final tidy step.

```{r}
mammals %>%
  dplyr::rename(
          genus        = Genus,
          wean_mass    = `wean mass`,
          max_life     = `max. life`,
          litter_size  = `litter size`,
          litters_yr   = `litters/year`
          )
```

##`ggplot()`
For the questions below, try to use the aesthetics you have learned to make visually appealing and informative plots. Make sure to include labels for the axes and titles.
```{r}
options(scipen=999) #cancels the use of scientific notation for the session
```

6. What is the relationship between newborn body mass and gestation? Make a scatterplot that shows this relationship. 

```{r}
names(mammals)
```

```{r}
mammals %>%
  ggplot(aes(x=newborn, y=gestation))+
  geom_point()
```




7. You should notice that because of the outliers in newborn mass, we need to make some changes. We didn't talk about this in lab, but you can use `scale_x_log10()` as a layer to correct for this issue. This will log transform the y-axis values.

```{r}
mammals %>%
  ggplot(aes(x=newborn, y=gestation))+
  geom_point()+
  scale_x_log10()
```



8. Now that you have the basic plot, color the points by taxonomic order.

```{r}
mammals %>%
  ggplot(aes(x=newborn, y=gestation, color=order))+
  geom_point()+
  scale_x_log10()
```


9. Lastly, make the size of the points proportional to body mass.

```{r}
mammals %>%
  ggplot(aes(x=newborn, y=gestation, color=order, size=mass))+
  geom_point()+
  scale_x_log10()
```


10. Make a plot that shows the range of lifespan by order.

```{r}
mammals %>%
  ggplot(aes(x=order, y=`max. life`, fill=order))+
  geom_boxplot()+
  coord_flip()+
  scale_y_log10()
```



## Push your final code to [GitHub](https://github.com/FRS417-DataScienceBiologists)
Make sure that you push your code into the appropriate folder. Also, be sure that you have check the `keep md` file in the knit preferences.