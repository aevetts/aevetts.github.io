---
layout: post
title:  "Predicting car prices: a fiesta of "
categories: jekyll update
---


<script type="text/javascript" async
     src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

### Context
How can I tell if I'm getting a good price for a used car? I know nothing about cars. First step to some sort of end-to-end project. Conveniently helps me learn some data science. Python (pandas, tensorflow)

### Data
first thoughtL Autotrader. Apparently they don't like scrapers. So Kaggle has the answer (which looks suspiciously like autotrader data...)
Some cleaning issues

### Analysis and visuals
A nice pic of the evaluation graphs.
Trying linear, then adding hidden layers. Play with epochs and learning rate.

Should I try without engine volume? Maybe no

Start with a linear model, look at eval graphs - no good. See also that the loss graph has stabilised so its not a question of more training.

Can we do a table of charts? Rows corresponding to different numbers of epochs, columns are different numbers of layers. Big grid (say 20x20) of MSE values, colour-coded to see how these two variables affect accuracy.
But there are other factors too - single hidden layer is decent but a bit biased to underpredict price.



### Key Insights
Decent prediction of Ford Fiestas using year, mileage, engine size
Compare different models

### Retro
What went wrong, or didn't work? What should I try next time?

### Call to action
Code on github.
Any advice for getting used car data from the real world? Direct to linkedin