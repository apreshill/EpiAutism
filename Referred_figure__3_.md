# Referred Figure (Fig. 3)
Alison Presmanes Hill  
May 13, 2015  

These figures are updated versions of earlier ones that appeared in a published book chapter:

> Hill, A. P., Zuckerman, K., & Fombonne, E. (2014). Epidemiological studies of autism and pervasive developmental disorders. In F. Volkmar (Ed.), *Handbook of autism and pervasive developmental disorders* (4th edition). New York: Wiley & Sons.

The chapter abstract:

> Since 1966, over 80 epidemiological surveys of autism spectrum disorders (ASDs) have been conducted in more than 20 countries. In this chapter, we review existing prevalence estimates for ASDs and discuss methodological factors impacting the estimation of prevalence and the interpretation of changes in prevalence estimates over time. Possible explanations for an increase in the prevalence of ASD within and across populations are considered. Increases in ASD diagnostic rates cannot currently be attributed to a true increase in the incidence of ASD due to multiple confounding factors. It remains to be seen how changes to diagnostic criteria introduced in the DSM-5 will impact estimates of ASD prevalence going forward.

Key Words: epidemiology, prevalence, incidence, Autism, Autism Spectrum, time trends, DSM-5, cross-cultural comparison

The chapter includes a lot more discussion of the figures. Please note that all figures utilize hypothetical data to illustrate methodological constructs.


```r
# load packages
library(ggplot2)
```

```
## Warning: package 'ggplot2' was built under R version 3.1.3
```

```r
library(plyr)
library(dplyr)
```

```
## Warning: package 'dplyr' was built under R version 3.1.2
```

```
## 
## Attaching package: 'dplyr'
## 
## The following objects are masked from 'package:plyr':
## 
##     arrange, count, desc, failwith, id, mutate, rename, summarise,
##     summarize
## 
## The following object is masked from 'package:stats':
## 
##     filter
## 
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
library(wesanderson)
```

```
## Warning: package 'wesanderson' was built under R version 3.1.2
```

```r
library(tidyr)
```

```
## Warning: package 'tidyr' was built under R version 3.1.2
```

```r
library(scales)
library(grid)
```

Thanks to Jeff Harrison on HelpMeViz.com for this figure! You can see the original figure here: 
http://helpmeviz.com/2015/05/06/combination-chart/


```r
#create hypothetical data for fig3
set.seed(1000)
time1 <- sample(1:105, 105, replace=F)
time2 <- time1
all_3 <- as.data.frame(cbind(time1, time2))

#reshape the fig3 data to looooong
fig3 <- all_3 %>%
  gather(time, estimate, time1:time2) %>%
  mutate(access = ifelse(time == "time1" & estimate <= 30, 1, ifelse(time == "time1" & estimate > 30, 0, ifelse(time == "time2" & estimate <= 60, 1, 0))))

#final fig3
ggplot(fig3, aes(x = factor(time), y = estimate, group = factor(access), colour = factor(access))) +
  
# formatting axes, plot area, + colors
  scale_x_discrete(name="", labels = c("Time 1", "Time 2")) +
  scale_y_continuous(name="ASD Cases per 10,000") +
  coord_cartesian(ylim = c(0,120), xlim = c(.6, 4)) +
  scale_color_manual(name = "ASD cases who are:", values = wes_palette("FantasticFox")[c(2:3)], labels = c("Not accessing\nservices", "Accessing\nservices")) + 

# adding horizontal line for true prevalence
  geom_segment(aes(x = .6, xend = 2.5, y = 105, yend = 105), lty = 3, lwd = .5, colour = "black") +
  
# creating jagged line segments
  geom_segment(aes(x = .6, xend = 1.2, y = 30, yend = 30), lty = 3, lwd = .5, colour = wes_palette("FantasticFox")[c(3)]) +
  geom_segment(aes(x = 1.2, xend = 1.8, y = 30, yend = 60), lty = 3, lwd = .5, colour = wes_palette("FantasticFox")[c(3)]) +
  geom_segment(aes(x = 1.8, xend = 2.5, y = 60, yend = 60), lty = 3, lwd = .5, colour = wes_palette("FantasticFox")[c(3)]) +
  
# adding jittered points
  geom_jitter(position=position_jitter(width=.2, height = 0), alpha = .75) +
  
# adding right text
  annotate("text", label = "Estimates of prevalence based\non population sampling will remain\nstable over time if true prevalence\nis stable.", x = 2.6, y = 105, size = 2.5, hjust = 0) +
  annotate("text", x = 2.6, y = 60, label = "Estimates of prevalence based\non individuals accessing services\ncan create an illusion of an\nincrease in prevalence over time,\nyet still underestimate prevalence\nat both time points.", size=2.5, hjust=0) +

# more formatting
  guides(colour = guide_legend(keywidth = 1, keyheight = 1.1, override.aes = list(alpha = 1))) +
  theme_bw() +
  theme(axis.ticks = element_blank(), panel.border=element_blank(), axis.line = element_blank(), panel.grid.major = element_blank(), panel.grid.minor = element_blank(), legend.justification=c(.9,0), legend.position=c(.82,0), legend.text = element_text(size = 6), legend.title = element_text(size = 6), legend.background = element_rect(fill="gray90", size=.25, linetype="dotted"), axis.title.y = element_text(size=9)) 
```

![](Referred_figure__3__files/figure-html/unnamed-chunk-2-1.png) 

```r
ggsave("fig3_dots.pdf")
```

```
## Saving 7 x 5 in image
```
