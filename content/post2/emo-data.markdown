---
title: "Topic Modeling"
date: 2020-03-13T14:19:53-05:00
draft: false
diagram: true
links:
  - icon_pack: fab
    name: Methods
    url: courses/example2
---


```r
library(graphics)
setwd("~/VimWiki/subjects/digital-humanities/stanford-output-dir/Working-Dir/")
country2 <- read.csv("Country_keys.csv", header = TRUE, sep = ",")
plot(country2$Words, country2$topics)
```

<img src="/post2/emo-data_files/figure-html/unnamed-chunk-1-1.png" width="672" />

```r
plot(country2)

library(plotrix)
country6 <- read.csv("Country_keys.csv", header = TRUE, sep = ",")
plot(country6)
```

<img src="/post2/emo-data_files/figure-html/unnamed-chunk-1-2.png" width="672" />

```r
pie3D(country6$percentage)
```

<img src="/post2/emo-data_files/figure-html/unnamed-chunk-1-3.png" width="672" />

```r
country7 <- read.csv("Country_composition1.csv", header = TRUE, sep = ",")
plot(country7)
```

<img src="/post2/emo-data_files/figure-html/unnamed-chunk-1-4.png" width="672" />

```r
country7$Hope
```

```
##  [1] 0.0009013077 0.0013049412 0.0009137270 0.0212263318 0.0995794691
##  [6] 0.0693817119 0.0383451736 0.1367345193 0.1243791611 0.0091733427
## [11] 0.0010601191 0.0008667410 0.0008717952 0.3467633324 0.0196899458
## [16] 0.0008638357 0.0008135303 0.1247870280 0.0014190480 0.0009249393
```

```r
pie3D(country7$Hope)
```

<img src="/post2/emo-data_files/figure-html/unnamed-chunk-1-5.png" width="672" />

```r
plot(country7$Hope, xlab = "Topics")
```

<img src="/post2/emo-data_files/figure-html/unnamed-chunk-1-6.png" width="672" />

```r
country7$Homemade
```

```
##  [1] 0.1058853923 0.0013049412 0.0009137270 0.0459284693 0.0007709188
##  [6] 0.0014508336 0.0074675016 0.0008727627 0.0008684733 0.2932479247
## [11] 0.0195867223 0.0811486881 0.0008717952 0.0009334065 0.0011633426
## [16] 0.1243745236 0.0625688742 0.0012763402 0.0384722543 0.2108931086
```

```r
pie3D(country7$Homemade)
```

<img src="/post2/emo-data_files/figure-html/unnamed-chunk-1-7.png" width="672" />

```r
View(country7$Homesick)
```

```mermaid 

graph TD;
  A-->B;   
  A-->C; 
  B-->D;
  C-->D;
```
