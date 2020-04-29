---
# Course title, summary, and position.
linktitle: Topic Modeling
summary: Looking at various topics in data sets (music lyrics).
weight: 1

# Page metadata.
title: Overview 3
date: "2018-09-09T00:00:00Z"
lastmod: "2018-09-09T00:00:00Z"
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.

# Add menu entry to sidebar.
# - name: Declare this menu item as a parent with ID `name`.
# - weight: Position of link in menu.
menu:
  example:
    name: Overview 3
    weight: 1
---

## What It Is

The first method that I will use is called topic modeling. There are a number of ways to model topics. I will be looking at a program called *Mallet*. Mallet was developed by Andrew McCallum at the University of Massachusetts, Amherst.

A topic is a group of words likely to appear together in the same document. What a topic modeling program does, is identify clusters of those words which are likely to appear in that document.

{{% alert warning %}} Please note that
topic modeling does not *merely* count the
number of times a word appears in a text,
which is something that may be said of
text mining. Rather, it attempts to
determine which words are *likely* to
appear in a document. {{% /alert %}}

### Some Initial Steps

1. Import the Data

As I have mentioned, for topic modeling I will be using Mallet. After installing Mallet and being confident that it works, use this command to import your data set. In my case, this is a txt file with a single song on it:

```/bin/mallet import-dir --input sample-data/web/en --output tutorial.mallet --keep-sequence --remove-stopwords
```

Briefly, I will point out what each point in the command refers to... 

```/bin/mallet
```

is where all of the important command scripts are.

with 

```import-dir
```

you are telling the program to enter the sample data which in this case is located in a folder in mallet entitle `sample-data/web/en`. 

```
--output tutorial.mallet
```

will be the name of the output file. 

```
--keep-sequence --remove-stopwords
```

