---
title: "Natural Language Processing"
date: 2020-03-13T14:19:53-05:00
draft: false
links:
  - icon_pack: fab
    name: Docs
    url: courses/example1
  - icon_pack: fab
    name: Slides
    url: slides/example
  - icon_pack: fab
    name: Project
    url: project1/internal-project

---

Moral knowledge is knowledge about right and wrong. Sources of moral knowledge may include moral testimony. Consider an ethics professional giving a public address to the American public; “lying is wrong” she says. Assuming that the American public comes to believe that lying is wrong on the basis of this professional’s testimony, and it turns out, that in fact it is true that “lying is wrong,” then the American public now have moral knowledge on the basis of this moral testimony. However, this kind of moral knowledge only speaks to one kind of approach which epistemologists may take in understanding sources of moral knowledge. For instance, moral testimony may not be the most salient source of moral knowledge. 

Alison Hills (2010) argues that while it is plausible to get moral knowledge from moral testimony, she does not think that we have any reason to trust it. Regarding moral knowledge, Hills thinks that what we care about is moral understanding and not moral knowledge. Hills argues that what we care about truly is whether someone can reliably do the right thing, justify their actions in doing the right thing, and whether someone will be disposed to do the right thing. All of this is explained by someone’s character, and moral testimony is not sufficient for building someone’s moral character. Briefly, to understand the difference between moral understanding and moral knowledge, consider the following example by Paulina Sliwa:

> Suit: Sam is standing at the shore of a lake when he sees a child beginning to drown. He believes that saving the child would be a good thing to do but it would involve ruining his new expensive suit. He cannot decide what to do and there is no one else at the lake, so he decides to call a friend whom he takes to be reliable. His friend tells him that he should save the child, and he believes him and saves the child.

While we might consider it to have been a good thing that Sam did save the child, there is something unsettling about his approach for doing so. We might expect that Sam should not need to rely on testimony in this case, that he should already know what the moral facts in this situation are. By the time that Sam is old enough to be concerned with the money he paid, he should already have enough knowledge regarding the facts in this this situation which is a question about his character.

Analytic philosophy, long thought to be a method used by epistemologists to understand varying kinds of knowledge, rests on a habit of categorizing by means of sets of abstract properties and labels. But according to @stum10a, there is another method available to epistemologists attempting to understand moral knowledge. This other method involves moral knowledge that we can get from moral narratives.


```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
library(tidytext)
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────────────────────── tidyverse 1.3.0 ──
```

```
## ✓ ggplot2 3.3.0     ✓ purrr   0.3.3
## ✓ tibble  2.1.3     ✓ stringr 1.4.0
## ✓ tidyr   1.0.2     ✓ forcats 0.5.0
## ✓ readr   1.3.1
```

```
## ── Conflicts ────────────────────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(graphics)
library(ggplot2)
setwd("~/VimWiki/subjects/digital-humanities/stanford-output-dir/Working-Dir/")
hext <- read.csv("Country_keys.csv")
hext4 <- read.csv("Country_composition.csv")
hext
```

```
##    Topics percentage
## 1       0    0.14595
## 2       1    0.21131
## 3       2    0.14796
## 4       3    0.43717
## 5       4    0.12483
## 6       5    0.23493
## 7       6    0.20921
## 8       7    0.14133
## 9       8    0.14063
## 10      9    0.48543
## 11     10    0.17166
## 12     11    0.14035
## 13     12    0.14117
## 14     13    0.15115
## 15     14    0.18838
## 16     15    0.13988
## 17     16    0.13173
## 18     17    0.20668
## 19     18    0.22979
## 20     19    0.14977
##                                                                            Words
## 1                        small road dirt pickup can't singin change supper pair 
## 2                          don't they're line moments whispering swear swinging 
## 3               feeling damn tonight holding tangled half wanna singing dreamed 
## 4    yeah that's morning missing sold door dogs strait things friday lean night 
## 5                          wilder dime makes sparks date babe blur song highway 
## 6                                        you're kiss forget forgive calls ain't 
## 7                                            smile work stay thinking sing hear 
## 8                                feel wildest hand rock put end work guy middle 
## 9                                gonna dreams plans happy free drive miles bout 
## 10                      home made ice crave love good store didn't christ shirt 
## 11                               it's screaming sign porch kitchen lines gettin 
## 12                       make hey turned truck i'll lites wasn't buttoned means 
## 13                               i'm homesick brown eyes kane laughing favorite 
## 14 hope she's wrecks friends felt hanging friend pic shows sparks turns driving 
## 15                  baby heart truth heaven quit bout i've reason songs waiting 
## 16     settle wanna tea sweet mixtape george town miller livin boss sunday grew 
## 17                                              mama cold lord word must've dad 
## 18                                                 cheats end night spend phone 
## 19            town thinkin goodnight front dancing stuff there's greens collard 
## 20                    homemade love windows guess man lights king jeans dressed
```

```r
hext1 <- tibble(line = 1:20, text = hext$Words)
hext1
```

```
## # A tibble: 20 x 2
##     line text                                                                   
##    <int> <fct>                                                                  
##  1     1 "small road dirt pickup can't singin change supper pair "              
##  2     2 "don't they're line moments whispering swear swinging "                
##  3     3 "feeling damn tonight holding tangled half wanna singing dreamed "     
##  4     4 "yeah that's morning missing sold door dogs strait things friday lean …
##  5     5 "wilder dime makes sparks date babe blur song highway "                
##  6     6 "you're kiss forget forgive calls ain't "                              
##  7     7 "smile work stay thinking sing hear "                                  
##  8     8 "feel wildest hand rock put end work guy middle "                      
##  9     9 "gonna dreams plans happy free drive miles bout "                      
## 10    10 "home made ice crave love good store didn't christ shirt "             
## 11    11 "it's screaming sign porch kitchen lines gettin "                      
## 12    12 "make hey turned truck i'll lites wasn't buttoned means "              
## 13    13 "i'm homesick brown eyes kane laughing favorite "                      
## 14    14 "hope she's wrecks friends felt hanging friend pic shows sparks turns …
## 15    15 "baby heart truth heaven quit bout i've reason songs waiting "         
## 16    16 "settle wanna tea sweet mixtape george town miller livin boss sunday g…
## 17    17 "mama cold lord word must've dad "                                     
## 18    18 "cheats end night spend phone "                                        
## 19    19 "town thinkin goodnight front dancing stuff there's greens collard "   
## 20    20 "homemade love windows guess man lights king jeans dressed "
```

```r
hext2 <- data.frame(lapply(hext1, as.character), stringsAsFactors=FALSE)
hext2
```

```
##    line
## 1     1
## 2     2
## 3     3
## 4     4
## 5     5
## 6     6
## 7     7
## 8     8
## 9     9
## 10   10
## 11   11
## 12   12
## 13   13
## 14   14
## 15   15
## 16   16
## 17   17
## 18   18
## 19   19
## 20   20
##                                                                             text
## 1                        small road dirt pickup can't singin change supper pair 
## 2                          don't they're line moments whispering swear swinging 
## 3               feeling damn tonight holding tangled half wanna singing dreamed 
## 4    yeah that's morning missing sold door dogs strait things friday lean night 
## 5                          wilder dime makes sparks date babe blur song highway 
## 6                                        you're kiss forget forgive calls ain't 
## 7                                            smile work stay thinking sing hear 
## 8                                feel wildest hand rock put end work guy middle 
## 9                                gonna dreams plans happy free drive miles bout 
## 10                      home made ice crave love good store didn't christ shirt 
## 11                               it's screaming sign porch kitchen lines gettin 
## 12                       make hey turned truck i'll lites wasn't buttoned means 
## 13                               i'm homesick brown eyes kane laughing favorite 
## 14 hope she's wrecks friends felt hanging friend pic shows sparks turns driving 
## 15                  baby heart truth heaven quit bout i've reason songs waiting 
## 16     settle wanna tea sweet mixtape george town miller livin boss sunday grew 
## 17                                              mama cold lord word must've dad 
## 18                                                 cheats end night spend phone 
## 19            town thinkin goodnight front dancing stuff there's greens collard 
## 20                    homemade love windows guess man lights king jeans dressed
```

```r
hext2 <- tibble(line = 1:20, text = hext2$text)
hext2
```

```
## # A tibble: 20 x 2
##     line text                                                                   
##    <int> <chr>                                                                  
##  1     1 "small road dirt pickup can't singin change supper pair "              
##  2     2 "don't they're line moments whispering swear swinging "                
##  3     3 "feeling damn tonight holding tangled half wanna singing dreamed "     
##  4     4 "yeah that's morning missing sold door dogs strait things friday lean …
##  5     5 "wilder dime makes sparks date babe blur song highway "                
##  6     6 "you're kiss forget forgive calls ain't "                              
##  7     7 "smile work stay thinking sing hear "                                  
##  8     8 "feel wildest hand rock put end work guy middle "                      
##  9     9 "gonna dreams plans happy free drive miles bout "                      
## 10    10 "home made ice crave love good store didn't christ shirt "             
## 11    11 "it's screaming sign porch kitchen lines gettin "                      
## 12    12 "make hey turned truck i'll lites wasn't buttoned means "              
## 13    13 "i'm homesick brown eyes kane laughing favorite "                      
## 14    14 "hope she's wrecks friends felt hanging friend pic shows sparks turns …
## 15    15 "baby heart truth heaven quit bout i've reason songs waiting "         
## 16    16 "settle wanna tea sweet mixtape george town miller livin boss sunday g…
## 17    17 "mama cold lord word must've dad "                                     
## 18    18 "cheats end night spend phone "                                        
## 19    19 "town thinkin goodnight front dancing stuff there's greens collard "   
## 20    20 "homemade love windows guess man lights king jeans dressed "
```

```r
hext2 %>%
  unnest_tokens(word, text)
```

```
## # A tibble: 171 x 2
##     line word  
##    <int> <chr> 
##  1     1 small 
##  2     1 road  
##  3     1 dirt  
##  4     1 pickup
##  5     1 can't 
##  6     1 singin
##  7     1 change
##  8     1 supper
##  9     1 pair  
## 10     2 don't 
## # … with 161 more rows
```

```r
hext2
```

```
## # A tibble: 20 x 2
##     line text                                                                   
##    <int> <chr>                                                                  
##  1     1 "small road dirt pickup can't singin change supper pair "              
##  2     2 "don't they're line moments whispering swear swinging "                
##  3     3 "feeling damn tonight holding tangled half wanna singing dreamed "     
##  4     4 "yeah that's morning missing sold door dogs strait things friday lean …
##  5     5 "wilder dime makes sparks date babe blur song highway "                
##  6     6 "you're kiss forget forgive calls ain't "                              
##  7     7 "smile work stay thinking sing hear "                                  
##  8     8 "feel wildest hand rock put end work guy middle "                      
##  9     9 "gonna dreams plans happy free drive miles bout "                      
## 10    10 "home made ice crave love good store didn't christ shirt "             
## 11    11 "it's screaming sign porch kitchen lines gettin "                      
## 12    12 "make hey turned truck i'll lites wasn't buttoned means "              
## 13    13 "i'm homesick brown eyes kane laughing favorite "                      
## 14    14 "hope she's wrecks friends felt hanging friend pic shows sparks turns …
## 15    15 "baby heart truth heaven quit bout i've reason songs waiting "         
## 16    16 "settle wanna tea sweet mixtape george town miller livin boss sunday g…
## 17    17 "mama cold lord word must've dad "                                     
## 18    18 "cheats end night spend phone "                                        
## 19    19 "town thinkin goodnight front dancing stuff there's greens collard "   
## 20    20 "homemade love windows guess man lights king jeans dressed "
```

```r
hext2
```

```
## # A tibble: 20 x 2
##     line text                                                                   
##    <int> <chr>                                                                  
##  1     1 "small road dirt pickup can't singin change supper pair "              
##  2     2 "don't they're line moments whispering swear swinging "                
##  3     3 "feeling damn tonight holding tangled half wanna singing dreamed "     
##  4     4 "yeah that's morning missing sold door dogs strait things friday lean …
##  5     5 "wilder dime makes sparks date babe blur song highway "                
##  6     6 "you're kiss forget forgive calls ain't "                              
##  7     7 "smile work stay thinking sing hear "                                  
##  8     8 "feel wildest hand rock put end work guy middle "                      
##  9     9 "gonna dreams plans happy free drive miles bout "                      
## 10    10 "home made ice crave love good store didn't christ shirt "             
## 11    11 "it's screaming sign porch kitchen lines gettin "                      
## 12    12 "make hey turned truck i'll lites wasn't buttoned means "              
## 13    13 "i'm homesick brown eyes kane laughing favorite "                      
## 14    14 "hope she's wrecks friends felt hanging friend pic shows sparks turns …
## 15    15 "baby heart truth heaven quit bout i've reason songs waiting "         
## 16    16 "settle wanna tea sweet mixtape george town miller livin boss sunday g…
## 17    17 "mama cold lord word must've dad "                                     
## 18    18 "cheats end night spend phone "                                        
## 19    19 "town thinkin goodnight front dancing stuff there's greens collard "   
## 20    20 "homemade love windows guess man lights king jeans dressed "
```

```r
hext2 %>%
  count(text, sort = TRUE) %>%
  ggplot(aes(text, n)) +
  geom_col() +
  xlab(NULL) +
  coord_flip()
```

<img src="/post3/emo-data_files/figure-html/unnamed-chunk-1-1.png" width="672" />

```r
hext3 <- data.frame(hext2$text, hext2$line)
smoothScatter(hext3)
```

<img src="/post3/emo-data_files/figure-html/unnamed-chunk-1-2.png" width="672" />

```r
str(hext3)
```

```
## 'data.frame':	20 obs. of  2 variables:
##  $ hext2.text: Factor w/ 20 levels "baby heart truth heaven quit bout i've reason songs waiting ",..: 15 3 5 19 18 20 16 4 6 7 ...
##  $ hext2.line: int  1 2 3 4 5 6 7 8 9 10 ...
```

```r
summary(hext3)
```

```
##                                                             hext2.text
##  baby heart truth heaven quit bout i've reason songs waiting     : 1  
##  cheats end night spend phone                                    : 1  
##  don't they're line moments whispering swear swinging            : 1  
##  feel wildest hand rock put end work guy middle                  : 1  
##  feeling damn tonight holding tangled half wanna singing dreamed : 1  
##  gonna dreams plans happy free drive miles bout                  : 1  
##  (Other)                                                         :14  
##    hext2.line   
##  Min.   : 1.00  
##  1st Qu.: 5.75  
##  Median :10.50  
##  Mean   :10.50  
##  3rd Qu.:15.25  
##  Max.   :20.00  
## 
```

```r
hext6 <- as.data.frame(read.csv("Country_keys.csv", header=TRUE))
data(hext6)
```

```
## Warning in data(hext6): data set 'hext6' not found
```
