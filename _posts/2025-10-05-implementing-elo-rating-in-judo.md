---
title:  "Implementing ELO rating in Judo"
mathjax: true
layout: post
categories: blog
---
Last summer, I coded a Judo ELO rating using R. My main goal was to quantify skill in judo, something normally only is done using the world ranking list.

## ELO Rating
The [Elo Rating](https://en.wikipedia.org/wiki/Elo_rating_system) works as follows. First, we assign a base ELO rating of $$1500$$ to ever individual judoka. Then, we go through the matches played (using data from [data.ijf.org](data.ijf.org)), and update the elo rating of two players after every match played.

If the winning player $$W$$ has a rating of $$R_W$$ and the losing player $$L$$ a rating of $$R_L$$, we use the following formulae:

<br>
$$E_W = \frac{1}{1+10^{(R_L-R_W)/400}}$$\\
$$E_L = \frac{1}{1+10^{(R_W-R_L)/400}}$$\\
<br>
Here, $$E_W$$ and $$E_L$$ denote the expected score (where 1 denotes a win and 0 denotes a 1) of the winning and losing player respectively. If $$R_L$$ is higher than $$R_L$$, $$E_W$$ will be closer to 1 and $$E_L$$ will be closer to 0.

The ratings are then updated according to the following formulae, where $$R_W'$$ and $$R_L'$$ denoted the updated ratings:
<br>
$$R'_W = R_W + K \cdot (1 - E_W)$$\\
$$R'_L = R_L - K \cdot E_L$$\\
<br>
Here, $$K$$ denotes the so-called $$K$$-factor, which is a parameter of the model that influences how much a single match affects the players' ratings. 

A higher $$K$$-factor means ratings will fluctuate more per match, and that it takes less matches to get to the top of the rankings. For now, I set it to $$K=40$$ after some small testing and seeing what felt right. However, in the future I am planning to formally optimize the $$K$$-factor based on prediction accuracy.

## Visualisation
Here you can view the video, where I visualised the ELO ratings of the -73 weight class over time, using Processing for the visualisation.

<iframe width="560" height="315" src="https://www.youtube.com/embed/DHo23RTPf7Y?si=xtUTTNa-p6zbncKu" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>