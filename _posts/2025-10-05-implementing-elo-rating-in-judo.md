---
title:  "Implementing the Elo rating in judo"
mathjax: true
layout: post
categories: blog
---
Last summer, I used contest data from the IJF to calculate Elo ratings for every judoka. My main goal was to quantify skill in judo, something normally only is done using the world ranking list. This can lead to some interesting insights, and it makes it possible to visualise which judoka was good during what time period.

## Elo rating
The [Elo Rating](https://en.wikipedia.org/wiki/Elo_rating_system) is a "method for calculating the relative skill of levels of players in zero-sum games", invented by chess master Arpad Elo. The rating works as follows. First, we assign a base ELO rating of $$1500$$ to ever individual judoka. Then, we go through the matches played (using data from [data.ijf.org](data.ijf.org)), and update the elo rating of two players after every match played.

## How it works
If the winning player $$W$$ has a rating of $$R_W$$ and the losing player $$L$$ a rating of $$R_L$$, we first determine the expected score of each player. The expected score $$E_i$$ for player $$i$$ can be interpreted as the probability that player $$i$$ will win the match. The expected scores for players $$W$$ and $$L$$ are given by:

<br>
$$E_W = \frac{1}{1+10^{(R_L-R_W)/400}}$$\\
$$E_L = \frac{1}{1+10^{(R_W-R_L)/400}}$$\\
<br>

By looking at the expressions, you can deduct that if $$R_L$$ is higher than $$R_L$$, $$E_W$$ will be closer to 1 and $$E_L$$ will be closer to 0. In other words: the player with the higher Elo rating has a higher chance to win.

Now we know the expressions for the expected scores of the two players, we can show the formulas that are used to update the ratings, where $$R'_i$$ denotes the updated rating for player $$i$$ after the match. If player $$i$$ has not yet played a match in the data set, we initialise their rating at $$R_i = 1500$$.

<br>
$$R'_W = R_W + K \cdot (1 - E_W)$$\\
$$R'_L = R_L - K \cdot E_L$$\\
<br>

## $$K$$-factor
Here, $$K$$ denotes the so-called $$K$$-factor, which is a parameter of the model that influences how much a single match affects the players' ratings. 

A higher $$K$$-factor means ratings will fluctuate more per match, and that it takes less matches to get to the top of the rankings. For now, I set it to $$K=40$$ after some small testing and seeing what felt right. However, in the future I am planning to formally optimize the $$K$$-factor based on prediction accuracy.

## Visualisation
In my code, I stored the Elo rating for every judoka after each competition day. This made it possible to visualise a top 10 using a simple moving bar graph. Here you can view the video, where I visualised the judokas with the highest Elo ratings in the -73 weight class over time, using Processing for the visualisation. It has become a great representation of who was dominant during which years, if I may say so myself.

<iframe width="560" height="315" src="https://www.youtube.com/embed/DHo23RTPf7Y?si=xtUTTNa-p6zbncKu" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

Later on, I will go further into quantifying skill in judo, using different methods, and validating these methods. This will allow us to get even more insights from the data.