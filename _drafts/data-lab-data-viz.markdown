---
layout: post
title:  "Climate Action: Co-benefits and Deprivation"
categories: jekyll update
---


<script type="text/javascript" async
     src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

Intro: the competition, the co-benefits atlas, Luigi, the initial idea

The Data Lab have recently closed submissions for their latest [Data Visualisation Competition](https://thedatalab.com/data-visualisation-competition-2025/). This year's competition centred on data behind the [UK Co-benefits Atlas](https://ukcobenefitsatlas.net/). The challenge was "to uncover and visualise a story from the data that has the potential to shift perspectives and influence beliefs".

What are co-benefits? 

Co-benefits are essentially side-effects of action to reduce greenhouse gas emissions. For example, reducing fuel use by reducing car travel might "accidentally" increase physical activity (from, say, cycling to work). Or a move towards more plant-based diets for environmental reasons may also result in a positive impact on health. On the other hand, some co-benefits are not benefits at all. For example, more active travel and public transport might result in longer commute times. The UK Cobenefits Atlas is based on modelling from the University of Edinburgh, based on climate action recommended by the [Climate Change Committee](https://www.theccc.org.uk/) in its Seventh Carbon Budget. Co-benefits are converted into a monetary value across 45,000 geographical areas covering the whole of the UK. This value is split between 11 different co-benefits (including physical activity increase, dietary improvements, road repairs impact...). The website already has some pretty comprehensive interactive visualisations where users can play around with different areas and combinations of co-benefits.

Luigi Creazzo (link) and I collaborated on a joint entry. Our initial broad idea was to see if we could connect co-benefits related to travel and green space with local government active travel plans and similar initiatives. In particular, we aimed to focus on Fife and Edinburgh as areas we are both familiar with. As it happens, we have both moved house recently, and in the course of house-hunting we've used the [Scottish Index of Multiple Deprivation](https://simd.scot), a useful resource that combines socio-economic measures including average income, housing, crime into a single deprivation index. Since both this and the co-benefits data is available at the level of quite small areas, we decided to incorporate the SIMD data to gain insight into how co-benefits are distributed across socio-economic brackets. Perhaps climate-change mitigation actions could help level the playing field. Or will they magnify existing inequalities?



The co-benefit data was provided via the Data Lab, with individual projections for the next 25 years. For simplicity we restricted outselves to the total over the time period.


Restrict to Scotland.
New cols with per-capita versions of data
Join with SIMD data
Join with shape file

Allows us to plot, e.g. total co-benefits heatmap for Fife. (insert image) Compare with the SIMD heatmap (insert image).

So are they correlated? Scatter plot. Well, kinda! Breaks down differently across the 11 co-benefits. Note excess_cold and dampness. And some co-benefits are clearly calculated per-capita and have just a few options.


Discussion of combined coben and SIMD ideas. Plot coben against SIMD.

Co-benefits are (very loosely) correlated positively with SIMD rank: the higher socio-economic bracket a person is in, the higher the ceiling on total co-benefits. Note that the minimum possible co-benefit is uniform across SIMD ranks. We wanted some way to combine the total per-capita co-benefit value with the SIMD ranking to get a 'normalised' co-benefit figure. The idea was to have a qualitative notion of the relative co-benefit to an area, which already takes socio-economic conditions into account, so areas could be compared like-for-like. We settled on simply looking at "co-benefit per SIMD rank", and defined the Relative Normalised Individual Co-benefit (RNIC) as

$$\mathrm{RNIC} = \log\left(\frac{\text{total per-capita co-benefit}}{\text{SIMD rank}}\right).$$

The $\log$ was added simply to make the values fall closer to a linear function so that they are more easily comparable, especially on heatmaps. It should be emphasised that this is purely qualitative and not necessarily backed up by strong theory.


RNIC plots. Discussion of colour schemes.

Final product: link poster (upload the pdf to github)





11 co-benefits:
air_quality: Benefits of reduced air pollution due to lower use of carbon-intensive heating and transport, valued in improved health and reduced building damages.
congestion: Impacts of reduced or increased traffic congestion resulting from changes in road use.
dampness: Benefits from reductions in damp housing, reflected in improved health and wellbeing.
diet_change: Health improvements from increased plant-based dietary shifts.
excess_cold: Benefits of warmer homes through insulation and efficient heating systems, valued through avoided health and wellbeing damages.
excess_heat: Benefits of improved ventilation and cooling reducing overheating risk in homes.
hassle_costs: Costs associated with increased journey durations where shifts away from private vehicle use occur. Referred to as 'longer travel times' in UK Co-Benefits Atlas.
noise: Benefits of reduced noise pollution from modal shift or quieter vehicles.
physical_activity: Health and longevity benefits from increased active travel (walking and cycling).
road_repairs: Impacts on road maintenance requirements due to changes in road usage.
road_safety: Impacts on collision incidence and risk associated with changes in vehicle travel.


Sudmant, A., Higgins-Lavery, R. (2025). The Co-Benefits of Reaching Net-Zero in the UK. Edinburgh Climate Change Institute, University of Edinburgh. https://doi.org/10.7488/ds/7978