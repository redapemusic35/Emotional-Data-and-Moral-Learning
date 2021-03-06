---
title: Internal Project
summary: An example of using the in-built project page.
tags:
- Deep Learning
date: "2016-04-27T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
  caption: Photo by rawpixel on Unsplash
  focal_point: Smart

links:
- icon: twitter
  icon_pack: fab
  name: Follow
  url: https://twitter.com/georgecushen
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: example
courses: example
---

This analysis uses a dataset of more than 380,000 songs since 1970 published 
in [kaggle](https://www.kaggle.com/gyani95/380000-lyrics-from-metrolyrics/data).
The main objective is to **develop clusters of music genres by the song lyrics** 
and the steps are the following:

* Data preparation (cleansing, transform etc.)
* Exploratory analysis  
* Topic modelling

Various R libraries were used, but it is mainly based on [#tidytext](https://twitter.com/hashtag/tidytext?src=hash) 
and [#tidyverse](https://twitter.com/hashtag/tidyverse?src=hash) environment.


```{r message=FALSE, warning=FALSE, include=FALSE, paged.print=FALSE}
# Libraries
library(readr)
library(tidyverse)
library(stringr)
library(tidytext)
library(lubridate)
library(wordcloud)
library(topicmodels)
library(tm)
library(stopwords)
library(quanteda)
library(ggthemes)
```

Data preparation was a very important part of the analysis. The first 
step was to exclude all songs, where the text field of lyrics was less than 10 
characters long.  
Then, after looking at the lyrics text field, it seems that there were a lot
of non-English songs. The decision was to include only English 
language songs in the final dataset. So, by using the **cld3** library, the
origin of the songs was detected & added to the dataset. The language detection wasn't perfect, as it 
misclassified a few songs. But because of the large original dataset, i 
decided to use it and remove all non-English songs from the dataset.  
Finally all songs that music genre was either missing or unknown were removed.
Also all songs with invalid year input (less than 1970) were removed.

More details about these steps can be found, in the actual code, at the end of 
the article.


```{r eval=FALSE, message=FALSE, warning=FALSE, include=FALSE, paged.print=FALSE}
# Insert data
songs <- read_csv("/Users/manos/Onedrive/Projects/R/Blog/data/songs.csv")
```


```{r eval=FALSE, message=FALSE, warning=FALSE, include=FALSE, paged.print=FALSE}
# DATA CLEANSING
## Filter data, handle missing values
# Keep songs with 10 or more characters in the lyrics
songs <- 
  songs %>% 
  filter(str_length(lyrics) > 10)
  
# Detect the language of the song
library(cld3)
songs$lang <- detect_language(songs$lyrics)
# Filter the songs that 
songs <- 
  songs %>%
  filter(lang == "en" & is.na(genre) == FALSE & year >= 1970)
  
songs$characters <- str_count(songs$lyrics)
```


```{r eval=FALSE, message=FALSE, warning=FALSE, include=FALSE, paged.print=FALSE}
## _text cleaning
# Create a vector with stopwords
stopwords <- c(stopwords())
  
# Clean text
songs$lyrics <- tolower(songs$lyrics)
songs$lyrics <- removePunctuation(songs$lyrics)
songs$lyrics <- removeNumbers(songs$lyrics)
songs$lyrics <- stripWhitespace(songs$lyrics)
songs$lyrics <- removeWords(songs$lyrics, stopwords)
songs$lyrics <- stemDocument(songs$lyrics)
# Save processed data for future use
saveRDS(songs, file = "/Users/manos/OneDrive/Projects/R/All_Projects/Songs_Lyrics/data/cleaned_data.RDS")
```


```{r message=FALSE, warning=FALSE, include=FALSE, paged.print=FALSE}
## _Insert processed dataset
songs <- readRDS(file = "/Users/manos/OneDrive/Projects/R/All_Projects/Songs_Lyrics/data/cleaned_data.RDS")
```

# MAIN ANALYSIS

Below there is a statistical table and a frequency plot to indicate the 
differences between the music genres.

```{r echo=FALSE, message=FALSE, warning=FALSE, paged.print=FALSE}
# library(DT)
# 
# knit_print.data.frame = function(x, ...) {
#   knit_print(DT::datatable(x), ...)
# }
songs_data <- 
songs %>% 
  group_by(genre) %>% 
  count() %>% 
  ungroup() %>% 
  mutate(`Proportion(%)` = round((n/sum(n))*100, 2)) %>% 
  arrange(-n) %>% 
  rename(Genre = genre,
         `Total songs` = n) 
knitr::kable(songs_data, caption = "Songs per genre")
#  knit_print.data.frame()
```



```{r echo=FALSE, message=FALSE, warning=FALSE, paged.print=FALSE}
songs %>% 
  group_by(genre) %>% 
  count() %>% 
  ungroup() %>% 
  mutate(Freq = n/sum(n)) %>% 
  arrange(-Freq) %>% 
  ggplot() +
  geom_col(aes(reorder(genre, -Freq), Freq), fill = "steelblue", alpha = 0.7) +
  labs(y = "Number of songs", x = "Music Genre", 
       title = "Proportion of songs per music genre", 
       subtitle = "From 1970 to 2016")+
  theme_fivethirtyeight() +
  scale_y_continuous(labels = scales::percent_format()) 
  
```

Most songs belong to the **Rock** genre, almost 50% of all songs 
in this dataset. The top 5 music genres are Rock, Pop, Hip-Hop, Metal & Country.  

It would be interesting to see if this is constant through time, or not. 
Below there is a plot indicates the variation through years.

```{r echo=FALSE, message=FALSE, warning=FALSE, paged.print=FALSE}
songs %>% 
  mutate(date = as_date(paste(as.character(songs$year), "-01", "-01"))) %>% 
  mutate(decade = floor_date(date, years(5))) %>% 
  group_by(decade, genre) %>% 
  summarise(N = n()) %>% 
  mutate(freq = round(N/sum(N), 2)) %>% 
  filter(genre %in% c("Country", "Hip-Hop", "Metal", "Pop", "Rock")) %>% 
  ggplot(aes(decade, freq, colour = genre)) +
  # geom_line() +
  geom_smooth(se = FALSE) +
  labs(y = "Smoothed proportion (%)", x = "Year", 
       title = "Smoothed proportion of total songs per Music Genre", 
       subtitle = "Top 5 genres are selected")+
  theme_fivethirtyeight() +
  scale_y_continuous(labels = scales::percent_format()) 
```

There are a few findings here. At first Rock genre decreases (from 60 % to 25 %)
in overall proportion of songs. On the other hand, hip-hop gradually rises 
(currently near to 20 %). Pop is around to 20 % and fairly steady through years.


Now let's try to figure out which music genre uses more lyrics.


```{r echo=FALSE, message=FALSE, warning=FALSE, paged.print=FALSE}
## number of characters per song
songs %>% 
  ggplot() +
  geom_boxplot(aes(genre, characters), fill = "steelblue", alpha = 0.7) +
  labs(y = "Length of song lyrics (in characters)", x = "Music Genre", 
       title = "Length (in characters) of song lyrics per music genre", 
       subtitle = "From 1970 to 2016")+
  theme_fivethirtyeight() +
  ylim(0, 10000)
songs %>% 
  group_by(genre) %>% 
  summarise(characters = round(mean(characters, na.rm = TRUE), 0)) %>% 
  ggplot(aes(reorder(genre, -characters), characters)) +
  geom_col(fill = "steelblue", alpha = 0.7) +
  labs(y = "Length of song lyrics (in characters)", x = "Music Genre", 
       title = "Average lyrics characters per music genre song", 
       subtitle = "From 1970 to 2016")+
  theme_fivethirtyeight()
```

Hip-Hop seems to be a significantly different music genre, as it uses 
**more than double** lyrics per song than the rest of the genres. 


# WORDCLOUD

It is interesting to see which are the most used words in each music genre. 
Below there are word clouds for the top 5 music genres. 
Word clouds (also known as text clouds or tag clouds) work in a simple way: 
the more a specific word appears in a source of textual data, the bigger 
and bolder it appears in the word cloud.  
Below there are word clouds for the top 5 music genres.


```{r echo=FALSE, message=FALSE, warning=FALSE, paged.print=FALSE}
words <- 
  songs %>%
  unnest_tokens(word, lyrics) %>% 
  group_by(genre, word) %>% 
  count() %>% 
  arrange(-n) %>% 
  group_by(genre) %>% 
  top_n(n = 100, wt = n)
# Select top 5 genres
genres <- c("Rock", "Hip-Hop", "Pop", "Country", "Metal")
for(i in 1:length(genres)){
  temp <- filter(words, genre == genres[i])
  
  # Create a word cloud
  par(bg="grey30")
  wordcloud(words = temp$word, freq = temp$n, col=terrain.colors(length(temp$word), alpha=0.9), random.order=FALSE, rot.per=0.3 )
  title(main =  genres[i] , font.main = 1, col.main = "cornsilk3", cex.main = 1.2)
}
  
```



# TOPIC MODELLING

Consider, for example, a situation in which you are confronted with a large 
collection of documents but have no idea what they are about. One of the first 
things you might want to do is to classify these documents into topics or themes. 
Among other things this would help you figure out if there’s anything interest 
while also directing you to the relevant subsets of the corpus. For small 
collections, one could do this by simply going through each document but this 
is clearly unfeasible for corpuses containing thousands of documents.

**Topic modeling** deals with the problem of automatically
classifying sets of documents into themes. The algorithm chosen is 
**Latent Dirichlet Allocation or LDA**, which essentially is a technique that 
facilitates the automatic discovery of themes in a collection of documents.

The basic assumption behind LDA is that each of the documents in a collection 
consist of a mixture of collection-wide topics. However, in reality we observe 
only documents and words, not topics – the latter are part of the hidden (or latent) 
structure of documents. The aim is to infer the latent topic structure given the
words and document. LDA does this by recreating the documents in the corpus by
adjusting the relative importance of topics in documents and words in topics 
iteratively.

In our case an LDA model with two topics was developed. After computing the topic
probabilities for all songs, we can see if this unsupervised learning, distinguish 
or reveal associations between music genres (regarding their lyrics).  
The box-plot below, reveals the probabilities of each music genre song to belong 
in each of the three topics.


```{r eval=FALSE, message=FALSE, warning=FALSE, include=FALSE, paged.print=FALSE}
# _Build Model ###############################################################
  
# split into words
by_word <- 
  songs %>% 
  unnest_tokens(word, lyrics)
# find document-word counts
word_counts <- 
  by_word %>%
  count(song, word, sort = TRUE) %>%
  ungroup()
# Create document term matrix
songs_dtm <- word_counts %>%
  cast_dtm(song, word, n)
songs_lda <- LDA(songs_dtm, k = 3, control = list(seed = 1234))
# Save for future use
save(songs_lda, file = "/Users/manos/OneDrive/Projects/R/All_Projects/Songs_Lyrics/objects/songs_lda_3.RDA")
``` 


```{r echo=FALSE, message=FALSE, warning=FALSE, paged.print=FALSE}
# Load the model
load(file = "/Users/manos/OneDrive/Projects/R/All_Projects/Songs_Lyrics/objects/songs_lda_2.RDA")
# _Calculate Tables ##########################################################
library(tidytext)
  
ap_topics <- tidy(songs_lda, matrix = "gamma")
  
topics_probs <- right_join(ap_topics, songs[, c("song", "genre")], by = c("document" = "song"))
 
topics_probs %>%
  mutate(genre = reorder(genre, gamma * topic)) %>%
  ggplot(aes(factor(topic), gamma, colour = factor(topic))) +
  geom_boxplot(alpha = 0.7) +
  labs(y = "Probability", x = "Topic", 
       title = "Topic probabilities per music genre", 
       subtitle = "") +
  theme_fivethirtyeight() +
  labs(col="Topics",
       y = "Probabilities") +
  facet_wrap(~ genre)
```

Hip-Hop genre is almost uniquely identified as a single topic (topic 2).
The rest of the music genres seem to be identified as another topic.  
So we can say that **Hip-Hop is definetely a music genre that uses significantly different language**
in the lyrics than the rest of the genres.  
More LDA models were developed (three & four topics) but the outcome of the initial
model (two topics) was more relevant and significant.

**CONCLUSION**  
Finally we can conclude that:  
- There is one music genre, Hip-hop, that is **significantly** different than
the rest of the genres, as it uses much more & different lyrics.  
- Rock genre is gradually decreases in popularity through years, by including less
songs and new genres are emerging. 

**HARDWARE ENVIRONMENT**  
All tasks of the analysis were accomplished on a laptop with 8 GB RAM & Intel 
Core I5 2.1 GHz. Some of the tasks can be demanding (for large datasets with 
text data). e.g. the LDA modelling task took around 7-8 minutes to complete.


[Full R code](https://github.com/mantoniou/Blog/blob/master/content/post/2018-06-text-analytics-song-lyrics.Rmd)
