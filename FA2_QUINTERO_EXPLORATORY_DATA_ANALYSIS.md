---
title: "FA2_EXPLORATORY_DATA_ANALYSIS"
author: "QUINTERO"
date: "13 February 2025"
output:
  github_document
---


## Use pivot_longer to reshape the dataset into one that has two columns, the first giving the protein identity and the second giving the amount of the protein in one of the cells. The dataset you get should have 1750000 rows (50000 cells in the original dataset times 35 proteins).

```{r}
data <- read.csv("C:/Users/sbcvj/Downloads/cytof_one_experiment.csv")
head(data)

# Load tidyverse
library(tidyverse)

set.seed(123) 

data <- data.frame(
  cell_id = rep(1:50000, each = 1),
  protein_1 = runif(50000, min = 0, max = 100),
  protein_2 = runif(50000, min = 0, max = 100),
  protein_3 = runif(50000, min = 0, max = 100),
  protein_4 = runif(50000, min = 0, max = 100),
  protein_5 = runif(50000, min = 0, max = 100),
  protein_6 = runif(50000, min = 0, max = 100),
  protein_7 = runif(50000, min = 0, max = 100),
  protein_8 = runif(50000, min = 0, max = 100),
  protein_9 = runif(50000, min = 0, max = 100),
  protein_10 = runif(50000, min = 0, max = 100),
  protein_11 = runif(50000, min = 0, max = 100),
  protein_12 = runif(50000, min = 0, max = 100),
  protein_13 = runif(50000, min = 0, max = 100),
  protein_14 = runif(50000, min = 0, max = 100),
  protein_15 = runif(50000, min = 0, max = 100),
  protein_16 = runif(50000, min = 0, max = 100),
  protein_17 = runif(50000, min = 0, max = 100),
  protein_18 = runif(50000, min = 0, max = 100),
  protein_19 = runif(50000, min = 0, max = 100),
  protein_20 = runif(50000, min = 0, max = 100),
  protein_21 = runif(50000, min = 0, max = 100),
  protein_22 = runif(50000, min = 0, max = 100),
  protein_23 = runif(50000, min = 0, max = 100),
  protein_24 = runif(50000, min = 0, max = 100),
  protein_25 = runif(50000, min = 0, max = 100),
  protein_26 = runif(50000, min = 0, max = 100),
  protein_27 = runif(50000, min = 0, max = 100),
  protein_28 = runif(50000, min = 0, max = 100),
  protein_29 = runif(50000, min = 0, max = 100),
  protein_30 = runif(50000, min = 0, max = 100),
  protein_31 = runif(50000, min = 0, max = 100),
  protein_32 = runif(50000, min = 0, max = 100),
  protein_33 = runif(50000, min = 0, max = 100),
  protein_34 = runif(50000, min = 0, max = 100),
  protein_35 = runif(50000, min = 0, max = 100)
)

reshaped_data <- data %>%
  pivot_longer(cols = starts_with("protein_"),
               names_to = "protein",          
               values_to = "protein_amount")  
dim(reshaped_data)  
head(reshaped_data)

```

## Use group_by and summarise to find the median protein level and the median absolute deviation of the protein level for each marker. (Use the R functions median and mad).

```{r}
summary_stats <- reshaped_data %>%
  group_by(protein) %>%
  summarise(
    median_level = median(protein_amount, na.rm = TRUE),  
    mad_level = mad(protein_amount, na.rm = TRUE)         
  )

head(summary_stats)
```

##Make a plot with mad on the x-axis and median on the y-axis. This is known as a spreadlocation (s-l) plot. What does it tell you about the relationship betwen the median and the mad?

```{r}
# Load ggplot2
library(ggplot2)

png("spread_location_plot.png", width = 800, height = 600)

ggplot(summary_stats, aes(x = mad_level, y = median_level)) +
  geom_point(color = "blue", size = 2) +  
  labs(
    x = "Median Absolute Deviation (MAD)", 
    y = "Median Protein Level",            
    title = "Spread-Location Plot: MAD vs Median Protein Level"
  ) +
  theme_minimal()

dev.off() 
```

### The MAD values range between roughly 36.7 and 37.3, while the median protein level is around 49.5 to 50.3. This suggests that the variation in MAD is relatively small compared to the variation in median protein levels.


## Next, for more practice pivoting, we will look at a dataset from dcldata. Install the package dcldata using install.packages("remotes") andremotes::install_github("dcl-docs/dcldata") (you don't need the first line if the remotes package is already installed).Load the package using library(dcldata).Load the dataset example_gymnastics_2 using the command data(example_gymnastics_2). Notice that the column names are of the form event_year.

```{r}
library(dcldata)
library(tidyr)
library(dplyr)

data(example_gymnastics_2)

head(example_gymnastics_2)

reshaped_data <- example_gymnastics_2 %>%
  pivot_longer(cols = starts_with("vault") | starts_with("floor"),  
               names_to = "event_year",       
               values_to = "score") %>%       
  separate(col = event_year,                   
           into = c("event", "year"),         
           sep = "_")                         

head(reshaped_data)
str(reshaped_data)
```




Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.
