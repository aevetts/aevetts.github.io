---
layout: post
title:  "Renewable Energy in Scotland"
date:   2025-06-30 16:24:36 +0100
categories: jekyll update
---

Intro

Something about source and methodology

Questions
1. How has renewable energy generationg changed in Scotland over the last 10 years?
2. What is the biggest renewable energy source?
3. How does the renewable energy generated in each Local Authority area differ?

Basic plot: RE consumption vs generation over 10 years (omit percentage plot)

Energy split (redo plot with bucketed small things)

    Offshore wind drop-off?

Differing generation between LAs. Better to split countyside vs city?

Net usage by region. Rounding/multiples etc. 

Net usage by region by area...
...and by population. Here, probably want mean over the 10 years? Or just 2023.

Tentative conclusions, answers to the questions

Include references to source of data







I have a Garmin watch which generates a lot of data which is straightforward to access, so seemed like a good place to start with some exploratory data analysis. I posed a few basic questions to answer, as a starting point:
1. Are daily steps and (kilo)calories burned broadly correlated?
2. When in the week do I burn the most calories?
3. When running, how is pace related to distance? I expect my longer runs to be slower, on average.

Power BI
==
My first port of call was PowerBI, a tool that comes up a lot in job descriptions. Unfortunately, I don't have a corporate account, so don't have access to the 'PowerBI Service', which would allow me to publish a report online. So I settle for the desktop version. Data on recorded activites (runs, walks, etc., anything deliberately recorded) comes as a .csv file. One can also download a much larger JSON file of all the daily collected data, such as steps, heartrate, stress, "body battery"... I set up a workflow to extract what I was interested in from these two data sources and combine them with a join into a single useful table. From there, its a case of playing with PowerBI's extensive GUI-based visualisation tools to create a few plots.

![PowerBI plots](/images/Running.jpg)

From the first two plots, it is easy to answer the first two questions.

1. Yes, daily steps and calories very much follow the same pattern. I'm sure that the calculation Garmin uses to estimate the calories burned factors in number of steps as a significant component, so no surprises here.
3. It seems that Thursday has been my top calorie-burning day, with Sunday the laziest. Although the difference is pretty small.

The third plot shows my running since 2022, using monthly running calories as a good measure of general activity. It looks like I peaked at the beginning of 2023 and have been declining since! Finally, another unsurprising observation is that my daily maximum average heart rate tends to be highest when I run.

To address question 3, I plotted pace against distance on a scatter plot. Here I ran into some problems. Pace needs to be stored as a numerical data type so that PowerBI can properly use it to plot, of course. But I want pace to be displayed as mm:ss, to make it useful to understand. PowerBI won't interpret this is a number, and I could not convince it to plot using the numerical value for pace but display mm:ss increments on the axis. This is representative of PowerBI's more fixed, GUI-based, more user-friendly and slightly less flexible setup. Seeking full control, I decided to learn pandas.

Pandas
==
As someone with a smattering of python as well as sagemath experience, pandas was pretty straightforward to pick up. It seems to be widely used too which means there are a plethora of online tutorials and stackexchange questions and answers. And of course ChatGPT is helpful. It is easy to read in .csv and JSON files, and bend the data to your will. I also used seaborn, a shiny plotting package built on mathplotlib. It was straightforward to replicate the plots produced by PowerBI, albeit a slightly more manual task.

More to the point, I was able to produce the desired scatterplot, with a function that converted numerical values of 'ticks' on the axis to the more readable mm:ss format. A nice feature is the ability to plot a third variable using colour, in this instance the calories burned per run.

![Distance against Pace](/images/pandas.png)

So, what is the answer to the third question? It seems that there isn't a straightforward correlation between pace and distance after all. Seaborn lets us do a linear regression plot, and we get a pretty much flat gradient (see below). Although it is still true that the very fastest paces only occur for short distances. The colour gradient is pretty and consistent with earlier plots showing calories and steps (and hence distance) going hand-in-hand. There are also clearly a lot of 5k runs!

![Regression](/images/regplot.png)

