---
title: "source_rmd"
author: "Connie Kot"
date: "12/3/2018"
output:
  html_document:
      keep_md: TRUE
---



## R Markdown

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:


```read
library(dplyr)
library(readr)
library(fields)
library(lme4)
library(ggplot2)

# Read in data
# positional data about the RV Kahuna
# kahuna <- read_csv('../../../presentations/2018/2018-12-03_DUML-RepResearch/data/2018-11-26_2017-Cape-Hatteras-BRS-kahuna-CEE.csv') 
setwd("~/Documents/data/Class/Repro/data")
kahuna <- read_csv('2018-11-26_2017-Cape-Hatteras-BRS-kahuna-CEE.csv')
kStart <- kahuna %>% 
  filter(status == 'start')

# Read in Gm182 Data: 100 estimated positions of Gm182, augmented with focal follow data
gm182UP <- read_csv('2018-11-27_Gm182-UserPoints-Start-CEE-Locations-Kahuna.csv') %>% 
  mutate(status = 'userPoints')

# Read in Gm182 Data: 100 estimated positions of Gm182
gm182 <- read_csv('2018-11-27_Gm182-Start-CEE-Locations-Kahuna.csv') %>% 
  mutate(status = 'noUserPoints')

# Minimal Wrangling of the data
gmpts <- bind_rows(gm182, gm182UP)
colnames(gmpts) <- c('trackNum', 'time', 'longitude', 'latitude', 'status')
setwd("~/Documents/data/Class/Repro/src")
```

## Including Plots

You can also embed plots, for example:


```plot
# Plot the points out:
ggplot(gmpts, aes(longitude, latitude, group = status))+
  scale_fill_gradient(low = "grey70", high = "grey30", guide = "none") +
  xlab("Longitude") +
  ylab("Latitude") +
  theme_minimal()+
  facet_grid(~ status, labeller = label_value) +
  stat_density_2d(aes(fill = ..level..), geom = "polygon", color = "black", size = 0.2, alpha = 0.5) +
  geom_point(color = "navy", size = .5, alpha = .4)

```

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.
