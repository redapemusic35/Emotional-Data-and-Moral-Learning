---
title: "100 top songs Topic Models"
date: 2020-03-13T14:19:53-05:00
draft: false
diagram: true
links:
  - icon_pack: fab
    name: Methods
    url: courses/example2
bibliography: /home/redapemusic35/VimWiki/Exported-Items.bib
---

"In text mining, we often have collections of documents, such as blog posts or news articles, that we’d like to divide into natural groups so that we can understand them separately. Topic modeling is a method for unsupervised classification of such documents, similar to clustering on numeric data, which finds natural groups of items even when we’re not sure what we’re looking for.

Latent Dirichlet allocation (LDA) is a particularly popular method for fitting a topic model. It treats each document as a mixture of topics, and each topic as a mixture of words. This allows documents to “overlap” each other in terms of content, rather than being separated into discrete groups, in a way that mirrors typical use of natural language" [@robi00, Ch. 6: Topic Modeling]

```{r}

library(corpus)
library(cowplot)
library(graphics)
library(dplyr)
library(tidytext)
library(tidyverse)
library(ggplot2)
library(readtext)
library(tm)
library(quanteda)
library(MASS)
library(topicmodels)
library(gutenbergr)
library(stringr)
library(scales)
library(mallet)
library(tm)
library(gdata)
library(cluster.datasets)
library(tm)
```

```{r}
countrydata <- "/home/redapemusic35/VimWiki/subjects/projects/lyrics/country-lyrics.csv"
hiphopdata <- "/home/redapemusic35/VimWiki/subjects/projects/lyrics/hiphoplyrics.csv"

countrynew <- read.csv(countrydata, header = TRUE, stringsAsFactors = FALSE)
hiphopnew <- read.csv(hiphopdata, header = TRUE, stringsAsFactors = FALSE)

countrylyrics <- countrynew
hiphoplyrics <- hiphopnew



require(tm)

countrycorp <- Corpus(DataframeSource(countrylyrics))
countrydtm <- DocumentTermMatrix(countrycorp)

hiphopcorp <- Corpus(DataframeSource(hiphoplyrics))
hiphopdtm <- DocumentTermMatrix(hiphopcorp)

data(countrydtm)
countrydtm

data(hiphopdtm)
hiphopdtm

countryterms <- Terms(countrydtm)

hiphopterms <- Terms(hiphopdtm)

tidycountry <- tidy(countrydtm)
tidycountry

tidyhiphop <- tidy(hiphopdtm)
tidyhiphop

# set a seed so that the output of the model is predictable
hiphopdtm_lda <- LDA(hiphopdtm, k = 2, control = list(seed = 1234))
hiphopdtm_lda

countrydtm_lda <- LDA(countrydtm, k = 2, control = list(seed = 1234))
# countrydtm_lda

```

```{r}

# Tidy per-topic-per-word probabilities

hiphop_topics <- tidy(hiphopdtm_lda, matrix = "beta")
hiphop_topics
```

```{r}

countrysentiment <- tidycountry %>%
  inner_join(get_sentiments("bing"), by = c(term = "word"))

hiphopsentiment <- tidyhiphop %>%
  inner_join(get_sentiments("bing"), by = c(term = "word"))
hiphopsentiment

countrysentiment %>%
  count(sentiment, term, wt = count) %>%
  ungroup() %>%
  filter(n >= 10) %>%
  mutate(n = ifelse(sentiment == "negative", -n, n)) %>%
  mutate(term = reorder(term, n)) %>%
  ggplot(aes(term, n, fill = sentiment)) +
  geom_bar(stat = "identity") +
  ylab("Contribution to sentiment") +
  coord_flip()

hiphopsentiment %>%
  count(sentiment, term, wt = count) %>%
  ungroup() %>%
  filter(n >= 10) %>%
  mutate(n = ifelse(sentiment == "negative", -n, n)) %>%
  mutate(term = reorder(term, n)) %>%
  ggplot(aes(term, n, fill = sentiment)) +
  geom_bar(stat = "identity") +
  ylab("Contribution to sentiment") +
  coord_flip()
```

```{r}
countrydata2 <- "/home/redapemusic35/VimWiki/subjects/projects/lyrics/country-lyrics.csv"


countrynew2 <- read.csv(countrydata2, header = TRUE, stringsAsFactors = FALSE)
countrylyrics2 <- countrynew2
countrylyrics2

# require(tm)

# countrycorp2 <- Corpus(DataframeSource(countrylyrics2))


# data(countrycorp2, package = "quanteda")

# country_inaugdfm <- quanteda::dfm(countrycorp2, verbose = FALSE)

# country_inaugdfm

```

# Notes
