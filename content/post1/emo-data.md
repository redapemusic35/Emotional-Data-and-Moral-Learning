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

```{r}
library(ggplots)

library(readtext)

lyrics <- system.file("~/VimWiki/subjects/digital-humanities/stanford-input-dir/Country/More-Hearts.txt", package = "readtext")
```

While we might consider it to have been a good thing that Sam did save the child, there is something unsettling about his approach for doing so. We might expect that Sam should not need to rely on testimony in this case, that he should already know what the moral facts in this situation are. By the time that Sam is old enough to be concerned with the money he paid, he should already have enough knowledge regarding the facts in this this situation which is a question about his character.

Analytic philosophy, long thought to be a method used by epistemologists to understand varying kinds of knowledge, rests on a habit of categorizing by means of sets of abstract properties and labels. But according to @stum10a, there is another method available to epistemologists attempting to understand moral knowledge. This other method involves moral knowledge that we can get from moral narratives.

```r
library(ggplot2)
setwd("~/mallet/")
country <- read.csv("country40_keys.csv")
str(country)
ggplot(country, aes(x = per, y = topics)) +
  geom_point()

plot(country)
```
