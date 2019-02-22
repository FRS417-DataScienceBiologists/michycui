---
title: "Midterm Exam"
date: "Winter 2019"
output:
  html_document:
    theme: spacelab
    toc: no
    toc_float: no
  pdf_document:
    toc: yes
---

## Instructions
This exam is designed to show me what you have learned and where there are problems. You may use your notes and anything from the `class_files` folder, but please no internet searches. You have 35 minutes to complete as many of these exercises as possible on your own, and 10 minutes to work with a partner.  

At the end of the exam, upload the complete .Rmd file to your GitHub repository.  

1. Load the tidyverse.

```{r}
library(tidyverse)
```


2. For these questions, we will use data about California colleges. Load the `ca_college_data.csv` as a new object called `colleges`.

```{r}
colleges <- 
  readr::read_csv("data/ca_college_data.csv")
```


3. Use your preferred function to have a look at the data and get an idea of its structure.

```{r}
glimpse(colleges)
```

4. What are the column names?

```{r}
names(colleges)
```


5. Are there any NA's in the data? If so, how many are present and in which variables?

```{r}
colleges %>% 
  summarize(number_nas= sum(is.na(colleges)))
```

```{r}
colleges %>%
  purrr::map_df(~sum(is.na(.)))
```


6. Which cities in California have the highest number of colleges?

LA, SD, SF, SAC
```{r}
colleges %>%
  count(CITY, sort=TRUE)
```


7. The column `COSTT4_A` is the annual cost of each institution. Which city has the highest cost?

Claremont
```{r}
colleges %>%
  select(COSTT4_A, CITY) %>%
  arrange(desc(COSTT4_A))
```



8. The column `ADM_RATE` is the admissions rate by college and `C150_4_POOLED` is the four-year completion rate. Use a scatterplot to show the relationship between these two variables. What does this mean?

```{r}
ggplot(data=colleges, mapping=aes(x=ADM_RATE, y=C150_4_POOLED))+
  geom_point()+
  labs(title="Admissions and 4-Year Completion Rates",
       x="Admissions Rate by College",
       y="Four-year completion rate")
```



9. The column titled `INSTNM` is the institution name. We are only interested in the University of California colleges. Run the code below and look at the output. Are all of the columns tidy? Why or why not?

Yes; columns are not variables. Each observation has its own row. Each value has its own cell.
```{r}
univ_calif <-
  colleges %>%
  filter_all(any_vars(str_detect(., pattern = "University of California")))
univ_calif
```


10. Use `separate()` to separate institution name into two new columns "UNIV" and "CAMPUS".

```{r}
colleges %>%
  separate(INSTNM, into=c("UNIV","CAMPUS"), sep="_")
```


11. As a final step, remove `Hastings College of Law` and `UC San Francisco` and store the final data frame as a new object `univ_calif_final`.

```{r}
colleges %>%
  transmute(UNIV=Hastings College of Law)
```

12. The column `ADM_RATE` is the admissions rate by campus. Which UC has the lowest and highest admissions rates? Please use a barplot.

```{r}
univ_calif %>%
  ggplot(aes(x=INSTNM, y=ADM_RATE))+
  geom_bar(stat="identity")
```



## Knit Your Output and Post to [GitHub](https://github.com/FRS417-DataScienceBiologists)