---
layout: post
title: Texas' Energy- A National Curiosity
subtitle: Texas stands alone compared to the rest of the US; it's energy market is deregulated allowing for high levels of competition between providers and greater innovation.
gh-repo: beverast/Notebooks
gh-badge: [star, fork, follow]
bigimg:
tags: [energy, markets, analysis, open source]
keywords: energy, electrical grid, markets, energy markets, renewable energy, ERCOT, Texas, open data, EIA, data storytelling, interconnection, energy analysis, data science, python, pandas, numpy, jupyter, seaborn, matplotlib, choropleth, geodata, jupyter notebook, open science, data analytics, Lambda School, DS3, beverast, jekyll
comments: true
---

![A National Curiosity](/img/all_combined_transparent3.png)
Jupyter Notebook: [https://github.com/beverast/Notebooks/blob/master/EIA_refactor.ipynb](https://github.com/beverast/Notebooks/blob/master/EIA_refactor.ipynb)

Data source: [https://www.eia.gov/opendata/](https://www.eia.gov/opendata/)

Note: Unrepresented states in the following visualizations have no energy data available from the EIA.

## Historical Background
There are three distinct, separate power grids, or “interconnections”, in the US:

![US Interconnections](http://thestatedtruth.com/wp-content/uploads/2017/04/US-Electrical-Grid.png)

Did you notice Texas? **Texas has its own state-level interconnection known as ERCOT: the Electric Reliability Council of Texas**. It covers the majority of Texas, but two small areas to the northwest and northeast of Texas, including El Paso, are split between the Western Interconnection and the Eastern Interconnection. These two interconnections are split along the Rocky Mountains. ERCOT services 75% of Texas land, and manages 85% of all the power transmitted in the state.[[1]](https://www.texastribune.org/2013/10/29/texplainer-how-texas-grid-managed/)

Historically, utility companies, the main power generating entities in the early 20th century, worked independently and supplied electrical energy to consolidated local areas. After Edison laid the foundation for the transport of electrical energy these companies tended to pop up sporadically. During World War 1 these companies began connecting their regional power grids, and during World War 2 the Texas Interconnect System was founded. This allowed hydroelectric facilities along rivers and dams in Texas to be able to send energy to many of the Texas factories that were aiding the war effort. 

Due to the state-level nature of the Texas Interconnect System they were adamant about retaining Texas’ state rights over their energy systems- **they worked hard to keep federal regulation at bay**. By not crossing state lines, these unified utility companies managed to subvert the Federal Power Act by Franklin Roosevelt. This is the historical precedent that paved the path for Texas’ growth in energy infrastructure. Self reliance was made easier by the fact that Texas is a state abundant in natural resources: plentiful river access, fossil fuel stocks, and area for harnessing wind energy in the future.

ERCOT formed in 1970 after a major blackout in the northeast in November 1965, which happened to be the worst blackout in national history. ERCOT was tasked with grid reliability in accordance with national standards, and to reiterate; ERCOT is not under the jurisdiction of the Federal Energy Regulatory Commission. **This “single-state jurisdiction” has allowed Texas to more rapidly expand and improve its infrastructure compared to the rest of the country**. This fact will continue to pop up as I explain Texas’ energy statistics and growth.[[2]](https://www.texastribune.org/2011/02/08/texplainer-why-does-texas-have-its-own-power-grid/)

## Visualization and Analysis

![Net Coal Consumption](/img/coal_consumption_combined.png)

In October of 2018 Texas consumed 6.476 million tons of coal for electrical energy generation. In comparison, Indiana came in second at 3.222 million tons. While population size is certainly a factor in the amount consumed, the amount of coal consumed is indicative of the resources available to each state. In terms of coal consumption, these are the top five states:
1. Texas: 6.476 million tons
2. Indiana: 3.222 million tons
3. Missouri: 2.626 million tons
4. Illinois: 2.959 million tons
5. Kentucky: 2.270 million tons

This choropleth visualization shows that Texas really puts it’s natural resources to use, and this helps ERCOT be able to put more focus on investing and growing their renewable sources and other aspects of their energy infrastructure, such as adding 2,300 miles of new power transmission lines.[[3]](https://poweringtexas.com/#revenue-generator)

![Fossil Fuel Stocks](/img/fossil_fuel_stocks_combined.png)

In this visualization fossil fuels refer to coal, natural gas, petroleum gas, and Texas is abundant in all of these fuels. These reserves aid Texas in being able to offer energy in many various forms, allowing the utility companies to be able to rely on a combination of sources whilst expanding their infrastructure and enabling the growth of renewables. 

![Net Wind Generation](/img/wind_generation_combined.png)

Here’s one facet where the deregulation of Texas’ energy markets comes into play. A big push in 1999 by then-governor George W. Bush added even more deregulation by allowing more energy companies to compete for customers and, in the short-term, enforcing a price floor so that large existing monopolies couldn’t undercut companies newly entering the Texas energy market.[[4]](https://www.technologyreview.com/s/602261/george-w-bush-helped-make-texas-a-clean-energy-powerhouse/)

Bush called for 5,880 megawatts of renewable energy capacity by 2015 and 10,000 by 2025, for all renewable energies. Only factoring in the use of wind energy:
* The demand set for 2015 was met in 2005
* The demand for 2025 was met in 2009

Texas continues this streak today, and ERCOT has worked with these deregulated entities to provide even more wind energy to a wider area of the state, through initiatives such as the Competitive Renewable Energy Zones Process. Read more about it here: [https://www.energy.gov/sites/prod/files/2014/08/f18/c_lasher_qer_santafe_presentation.pdf](https://www.energy.gov/sites/prod/files/2014/08/f18/c_lasher_qer_santafe_presentation.pdf)

![Average Retail Price of Electricity](/img/retail_price_combined.png)

From this data, Texas comes in 12th place in offering the lowest retail price of electricity on average for all sectors. **Oklahoma comes in first at 6.93 cents/kWh and Texas isn’t too far away at 8.38 cents/kWh**. This data shows that renewable energy is inexpensive to produce so the retail cost is low when factored in with all energy types, especially in a competitive energy market. Residents of Texas benefit from deregulation in that they have the option of choosing from many different electricity providers to power their homes, and the competition between these companies drives prices as low as possible. Split by sector, Texas’ average retail cost of electricity beats the national average on every front[[5]](https://www.chooseenergy.com/texas/):
* Residential rates are nearly 11% lower than the national average
* Commercial rates are 28% lower than the national average
* Industrial rates are 24% lower than the national average

## Summary

There’s so much more to the workings of electrical grids and Texas’ specifically that I’d love to cover, but I’ll have to save them for later posts. An interesting tale that I didn’t mention in the historical section is about an event called ‘The Midnight Connection’ where Texas sent power across state lines to Oklahoma for an hour which led to Texas nearly being placed under federal regulation. Alas, from the data I’ve examined, Texas is a strong leader in electrical power and the deregulated energy markets. Although, Texas is not the only state with a deregulated energy market; there are currently 15 other states that share this design. In an age where renewable energy is becoming more and more of a central focus for states, businesses, and citizens, Texas again shows that competitive markets and cooperative initiatives can have an amazing impact, bettering the situation for every party.

#### Further Reading:

[https://www.jstor.org/stable/40923434?seq=1#page_scan_tab_contents](https://www.jstor.org/stable/40923434?seq=1#page_scan_tab_contents)

[http://www.ercot.com/about/profile/history/](http://www.ercot.com/about/profile/history/)

[https://tcaptx.com/downloads/THE-STORY-OF-ERCOT.pdf](https://tcaptx.com/downloads/THE-STORY-OF-ERCOT.pdf)

[https://www.texastribune.org/2011/02/08/texplainer-why-does-texas-have-its-own-power-grid/](https://www.texastribune.org/2011/02/08/texplainer-why-does-texas-have-its-own-power-grid/)

[https://www.texastribune.org/2013/10/29/texplainer-how-texas-grid-managed/](https://www.texastribune.org/2013/10/29/texplainer-how-texas-grid-managed/)


