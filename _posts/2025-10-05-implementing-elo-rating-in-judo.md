---
title:  "Implementing ELO rating in Judo"
mathjax: true
layout: post
categories: blog
---
Last summer, I coded a Judo ELO rating using R. My main goal was to quantify skill in judo, something normally only is done using the world ranking list.

## ELO Rating
The [Elo Rating](https://en.wikipedia.org/wiki/Elo_rating_system) works as follows. First, we assign a base ELO rating of 1500 to ever individual judoka. Then, we go through the matches played (using data from [data.ijf.org](data.ijf.org)), and update the elo rating of two players after every match played.

If the winning player $W$ has a rating of $R_W$ and the losing player $L$ a rating of $R_L$, we use the following formulae:
<br>
$$E_W = \frac{1}{1+10^{(R_L-R_W)/400}}$$\\
$$E_L = \frac{1}{1+10^{(R_W-R_L)/400}}$$\\
<br>
Here, $E_W$ and $E_L$ denote the expected score (where 1 denotes a win and 0 denotes a 1) of the winning and losing player respectively.
The ratings are then updated according to the following formulae, where $R_W'$ and $R_L'$ denoted the updated ratings:
<br>
$$R'_W = R_W + K \cdot (1 - E_W)$$\\
$$R'_L = R_L - K \cdot E_L$$\\
<br>
## Visualisation
Here you can view the video, where I visualised the ELO ratings of the -73 weight class over time, using Processing for the visualisation

{% include embed.html url="https://www.youtube.com/watch?v=DHo23RTPf7Y" %}
