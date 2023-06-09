---
title: "Assignment 8"
author: "Emmanuel Sowunmi"
date: "`r Sys.Date()`"
output: word_document
---

```{r, warning=FALSE, message=FALSE}
library(dplyr)
library(HSAUR2)
library(MASS)
library(tidyverse)
library(factoextra)
```

Exercise 3.1
PCA with scale = TRUE
```{r, warning=FALSE}
heptathlon_new = subset(heptathlon, select = -c(score))

heptathlon_pca_T <- prcomp(heptathlon_new, scale = TRUE)
heptathlon_pca_T
summary(heptathlon_pca_T)
```
Summary
From our Result above, we can observe that the first two PC's (PC1 and PC2) account for about 80% of the total variation.

PCA with scale = FALSE
```{r}
heptathlon_pca_F <- prcomp(heptathlon_new, scale = FALSE)
heptathlon_pca_F
summary(heptathlon_pca_F)
```
Summary
From the result above, we can see that the first two PC's account for about 97% of the total variance.

PCA bi plot for both scale = TRUE and scale = False
```{r, warning=FALSE}
fviz_pca_biplot(heptathlon_pca_T, repel=TRUE, pointsize=1, pointshape=4, col.var="red", arrowsize=0.6, labelsize=3, palette=c("green2", "gold", "skyblue2"), title = 'Heptathlon PCA Bi plot (scale = TRUE)')



fviz_pca_biplot(heptathlon_pca_F, repel=TRUE, pointsize=1, pointshape=4, col.var="red", arrowsize=0.6, labelsize=3, palette=c("green2", "gold", "skyblue2"), title = 'Heptathlon PCA Bi plot (scale = FALSE)')
```

SUMMARY

From both PCA's and the the graph, we can see that if we set the scale to False,the variance will be very high. This is because of the nature of some of the data. eg the distance of javelin vs long jump.
...

Exercise 3.2

```{r}
nba <- read.table('C:/Users/esowu/Downloads/2013_nba.txt', row.names=NULL)

nba_clean = nba %>% 
  subset( select = -c(Player,Team,row.names))
rownames(nba_clean) = paste(c(nba$row.names), c(nba$Player))
head(nba_clean)
```

```{r}
nba_pca <- prcomp(nba_clean, scale = TRUE)
nba_pca
summary(nba_pca)
```


```{r}
fviz_pca_biplot(nba_pca, repel=TRUE, pointsize=1, pointshape=4, col.var="red", arrowsize=0.6, labelsize=3, palette=c("green2", "gold", "skyblue2"), title = 'NBA PCA Bi plot')
```

Summary.

From the PCA test and the biplots, we can see that players with the same position will cluster together.

Exercise 3.3


```{r}
mileage <- read.table('C:/Users/esowu/Downloads/Mileage.txt', row.names=NULL)
mileage
mileage = mileage %>% 
  subset( select = -c(row.names)) 

fit = cmdscale(dist(mileage))

x <- fit[, 1]*-1
y <- fit[, 2]*-1
plot(x, y, 
     main = 'Mileage PLot',
     xlab = "Coordinate 1", 
     ylab = "Coordinate 2",
     xlim = range(fit[, 1])*.7, type = "n")
     text(x, y, labels = colnames(mileage)
     )
```

Summary

when i initially made the plot i noticed that i got a mirror view of the adjusted plot. This is because the direction is lost upon projection, so i multiplied the points by -1 as seen above and we get an accurate map that shows that the resulting plot is similar to the map of the U.S.
