---
layout: post
title: "Challenge 4 Fletcher Project"
description: "Unsupervised learning: clustering of NYT technical news."
categories: [project]
tags: [clustering, NLP, worldcloud, MongoDB, jekyll]
redirect_from:
  - /2017/08/06/
---

# Project 4 Fletcher Project --- 

## Story
The generation of technologies is rapidly changing. The hotness in the new technology attracts people and capital, meanwhile, some area is losing attentions, like physics or biology. When Dolly was born, genetic engineering was hot topic and students were willing to study it, however, there were not enough positions to absorb so many new graduates, quite a lot of them need to tune majors to find jobs after graduation. At this time, computer science or data science become popular, it is common to hear that the market needs millions of professionals in data science. But no exception, data sciensts some day will have to face the similar situation like biologists now. 

With this motivation, I want to read history, see the trend of a frontier technology or science subject in the past decades from New York Times. With more data, like statistics of labor in different position, maybe we can see when a new tech generates, how many unemployment it will bring to the market. Futher with more source or analysis, maybe we can predict the year that AI occupies some positions.
<br>

## Goal
In this project, I start the journey by finding the keywords abstracting from New York Times leading paragraphs in technology and science yearly from 1945 to 2017. Because the unemployment is related to companies, I did a sample of name entitites of Google using Word2Vec.

## Data
Using New York Times Developers API, I got more than 8,350,000 daily documents. Each document includes information of publication time, section, keywords, author, leading paragraph and etc., then using MongoDB mask to choose 3,755,000 leading paragraph related to science and technology.

At first, I created a mask for section in science & technology, but after I checked the data after mask, I found that before Oct.10, 1980, there was no section of science & technology. As a reslut, I added keywords of science and technology to the mask, so if the document has anything related to science and technology, it will appear in my database.

## Approach 1, the wordcloud of science&technology for decades.

Take year 2015 for example, with the science & technology related leading paragraphs, first applied word token, separating paragraphs to words, then with my own stopwords bag, deleting the words that could not represent the frontier technology. After the process, I tried stemming, which is to abstract the stem of the word, like take went, gone and goes all as go. But the attempt to make a word cloud with stemming gave strage result, appearing uncomplete words due to over-stem, besides, counts of word was reduced to 9k compared to 33k after stopwords.

Sci-Tech Leading paragraphs: 2,994
Total words: 157,000
After stopwords: 33,000
After stemming: 9,600
Word2Vect: 14,800

Word Cloud over Time

LDA (min_df=1, max_df=0.1)


<br>
Figure 1. The correlation between rating and difficulty
<br>
![rating vs difficulty]({{ site.url }}/images/rating-difficultylevel.png){: .center-image}{:height="40%" width="40%"}
<br>
Let's look into this set of data. The model is simple, created as Rating vs. difficulty and number of rating. But the number of rating is negatively skewed, so firstly any rating with less than 2 ratings were dropped, then squareroot was applied also with log. This transform raises R-squared 0.001.

<br>
Figure 2. The transform of number of rating
<br>
![transform rating numbers]({{site.url}}/images/transform.png)
<br>

<br>
Figure 3. Fitting the model of rating and difficulty
<br>
![rating difficulty]({{site.url}}/images/rating-difficulty.png){:height="600px" width="450px"}
<br>


## Answer 2： Reputation is the most important factor when students rate their university.

And following is facilities, reputation, ..., last one is safety.

At first, I built a model with overall rating with all other 10 features, and it was perfect, but we should not use it, because everything just too linearly correlated. 

<br>
Figure 4. The features to the overall quality rating
<br>
![campus rating]({{site.url}}/images/sch-rating_feature.png){:height="300px" width="300px"}
<br>

So I changed to another model,
<br>
 *happiness ~ opportunity + facilities + reputation + social + clubs + food + location + internet + safety*	
<br>
I tried removing any feature from this model, it will not as good as with all feature together, as a result, this model is the final model to predict the happiness. R-squared is 0.791, and train score is 0.830, while test score is 0.749. After plooting mean squared error, degree is better to stay at 1 and Ridge cross validation score is 0.828.

<br>
Figure 5. New model: the features to Happiness
<br>
![new model campus rating]({{site.url}}/images/new sch-rating_featrue.png){:height="300px" width="300px"}
<br>

## Answer 3： Teaching quality will not influence the rating of universities.

To my surprise, the rating of professor will not correlated to the rating of universities. After thinking about it, the expaination is that adjunct professors usually take most of the teaching, who is less bonded to the universities or the students, also, the rating of professor ususally focused on one course, students will not take consider when they rate the university.

<br>
Figure 6. Correlation with happiness and averaged professor rating
<br>
![happiness teacher rating]({{site.url}}/images/happiness-t_rating.png)
<br>
<br>
Table. Correlation and evaluation in two models
<br>
List of Events

| Year | Key Words | What happend |
|-------|--------|---------|
| 1945 |			atomic, war|
| 1947 |			war, photographic|
| 1949 |			wiener | Mr. Wiener: an American mathematician and philosopher, published| a book Cybernetics: Or Control and Communication in the Animal and the Machine in 1948.|
| 1950 | 			norbert|
| 1953 |			battery, storage|
| 1955 |			aeronautical|
| 1956 |			enthnological|
| 1958 | 			alamos, computer|
| 1960 |			cell, rabbits|
| 1961 | 			radio, Schlesinger | Mr. Schlesinger: son and father, American historian, social critic, and public intellectual.|
| 1963 |			stars|
| 1964 |			Artucio | Artucio: Uruguayan architect and architectural historian.|
| 1966 |			mathematics|
| 1970 | 			teleprompter|
| 1971 |			monoxide|
| 1974 |			infrared, television|
| 1976 |			bacteria|
| 1980 | 			hopkins|
| 1982 |			egg, diary|
| 1983 |			saturn, computer|
| 1984 |			particle|
| 1985 |			comet, disk, binoculars|
| 1986 |			genetic,biotchnology|
| 1988 |			Kodak|
| 1991 |			computer, printer|
| 1993 |			windows|
| 1994 |			chip, computer, optic, communications|
| 1995 |			www, http|
| 1999 |			mars, simon, HP|
| 2000 |			fiber, optic, camera, dell, alamos|
| 2001 |			stem, embryos, cell, amazon, dell|
| 2003 |			gene|
| 2008 |			carbon|
| 2009 |			app, car, telescope, plant, ray|
| 2010 |			ipod, chip|
| 2011 |			smart, silicon, dioxide|
| 2012 |			web, sandy|
| 2014 |			Albert Einstein, psychology|
| 2016 |			cloud, car, trump|
| 2017 |			car, vehicle, Elon Musk, nasa|
                    

## Future work, how to improve





 
                    

                    
word2vec on 2015

PCA n_components=2

 w2v

Clustering Models:

KMeans versus WARD

n_clusters = 6

 kmeans  ward

Possible groups

ward_section

1: UPenn, NYU, Cornell, Yale, date

2: Navy, NJIT, Evangelist Catholic, locations

3: IPO, Exxon Mobil, Elon Musk, Mark Zuckerberg, gmail

4: biologist, excellence, Zajfman(Physicist), some first names

5: China, Paris, silicon, NASA, Amazon, Uber, Yahoo

6: economics, foundation, father, daughter-in-law


Google relationship extraction on 2017

document: 1215

words: 27,000




### PostScript
As a PhD in Materials Science and Condensed Matters Physics, I struggled in the job markets. It is hard to find industrial positions as my experience was with high energy X-ray characterization. At first, I thought it is my communication problem not to describe clearly what I have done in my research to people who interviewed me, but after a year of teaching Physics in university, comparing to my peers teaching lectures, I have better reputation among students. Then I read a statistic report, the figure is below, and with a better understanding that this market is in saturation, it is just harder to get a job in Physics.

#### Reference
https://en.wikipedia.org/wiki/List_of_years_in_science
https://www.codeschool.com/blog/2016/03/25/machine-learning-working-with-stop-words-stemming-and-spam/
https://github.com/smilli/py-corenlp
http://lab.hakim.se/reveal-js/#/