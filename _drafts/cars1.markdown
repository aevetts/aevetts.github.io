---
layout: post
title:  "A first look at price prediction: Ford Fiestas"
categories: jekyll update
---


<script type="text/javascript" async
     src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

### The Big Picture
Consider the following (not far-fetched) premise. I'm looking for my next car (used of course, I'm neither made of money nor am I into cars enough to care too much as long as it runs). I've done some rudimentary research and more or less know what I'm after. I find what might be the car for me advertised by a local dealership or a private seller. How can I gauge if the price is fair? What I would like is a tool where I can input the car details and find out how fair the price looks, in terms of the current market conditions.

My plan is to build such a tool, as a (hopefully) fun learning exercise. And blog about it along the way to document my process and to keep myself accountable.

### The First Step
I'm going to start from the middle and build out in all directions. First up is the machine learning. I'm going to build a price predictor using Python (Pandas, TensorFlow, Keras). 

<small>Side note: do we capitalise Python and its packages?</small>

### Data
I need some training data. The first source that came to mind was [Autotrader](https://www.autotrader.co.uk/). However, it turns out that its not so easy to get access via their API and apparently they aren't keen on scrapers. For now, Kaggle user Guanhao Peng has the answer in the form of their dataset [UK Used Car Listing Data](https://www.kaggle.com/datasets/guanhaopeng/uk-used-car-market/data) <small>(which looks suspiciously like autotrader data)</small>.

I'm starting very small and just looking at Ford Fiestas. They're a very popular choice in the secondhand market in the UK (indeed, I own one) so there is plenty of data. I'm hypothesising that mileage, year of registration, and engine volume might be enough info to determine a decent price prediction. There is more to the various kinds of Fiestas than just engine volume, but that is categorical and for this initial step I just want to focus on numerical data.

### Cleaning

Missing year and reg: discard rows. Note: some of these are new cars anyway so not interested.

Into just Fiestas. Some missing engine vols. Only a few, so manually checked and the free text description field has the data in some cases. Discard the others.


### Analysis and visuals
Start with a linear model, look at eval graphs - no good. See also that the loss graph has stabilised so its not a question of more training.

Gradually add hidden layers. Turns out just using the three features gives what looks like a reasonable predictor.

Can we do a table of charts? Rows corresponding to different numbers of epochs, columns are different numbers of layers. Big grid (say 20x20) of MSE values, colour-coded to see how these two variables affect accuracy.
But there are other factors too - single hidden layer is decent but a bit biased to underpredict price.

A nice pic of the evaluation graphs.

### Key Insights
Decent prediction of Ford Fiestas using year, mileage, engine size.
Compare different models

### Next steps
From personal experience, I know that the location of the car can also impact the price. The last time I bought a car, I was working in Manchester, and I found significantly better prices there than I could find at home in Edinburgh. So for one of the next steps in this project I'd like to take this into account. The data includes the location as a string so perhaps I can map that to coordinates and incorporate that into the training.

How to expand to other makes and models? Could train a seperate NN for each type of car, but there are too many to be practical. Clearly, a Ford and a BMW of the same age with matching mileages and engine sizes are not going to cost the same. So we need to include this categorical data as a feature. Since there are so many possible make/model combinations, one-hot encoding is going to give a high-dimensional imput layer, and also not capture any similarities between similar models of car. So an embedding layer might be the way to go. This is a way of teaching the neural network to represent makes/models as points in a smallish-dimensional continuous vector space. As well as keeping the dimensional manageable, this also gives the model a chance to learn that, say, a Ford Fiesta is "close to" a Vauxhall Corsa, but "far away from" a Porsche Cayenne.

### Call to action
Code on github.

If you have any advice for getting up-to-date data on car prices from the real world, I'd love to hear it! You can reach me via [LinkedIn](https://www.linkedin.com/in/alex-evetts-a9a93a1aa/).

Another instalment of this project coming soon.