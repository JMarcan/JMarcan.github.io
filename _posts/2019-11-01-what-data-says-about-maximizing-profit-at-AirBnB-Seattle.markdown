﻿---
layout: post
title:  "What data says about maximizing profit at AirBnB Seattle"
date:   2019-11-01 17:03:22 +0100
categories: jekyll update
---
[GitHub Repository](https://github.com/JMarcan/the-most-profitable-months-airnbnb-seattle/)
![Avaibility](/assets/what-data-says about-maximizing profit-at-AirBnB-Seattle/avaibility.png)

To follow my interest in Data-Science, Digital Platforms and Sharing Economy:
<br/><br/>
I've taken AirBnB data of Seattle city published on [Kaggle](https://www.kaggle.com/airbnb/seattle/data) to answer the following questions:
1. How can landlord increase his income by increasing his activity on AirBnB?

2. When is the most profitable month to be landlord at AirBnB?

3. When is the most profitable month to be tenant at AirBnB?

## So, what data has to say about maximizing your profit at AirBnB Seattle?

### 1. Landlords responding early command higher prices for renting their houses

In comparison with landlords taking for a response a 'few days or more':

- 4% higher rent commanded by landlords who respond 'within an hour'

- 9% higher rent commanded by landlords who respond 'within a few hours'

| host_response_time | price | % |
| a few days or more | 120.8\$ | 100% |
| within a day | 122.1\$ | 101% |
| within a few hours | 131.3\$ | 109% |
| within an hour | 125.1\$ | 104% |

Assumption: Since ‘price’ represents listed data instead of true realized income the statement above is made with the assumption that the axiom of rationality is valid and landlords set pricing in such a way that their income is maximized.

Besides, although being more responsive will not hurt,
I want to stress that the correlation is not necessary the causation as demonstrate the picture below:
 
![correlation is not causation](/assets/what-data-says about-maximizing profit-at-AirBnB-Seattle/correlation_is_not_causation.png)
source: [spurious-correlations](http://tylervigen.com/spurious-correlations)

### 2. Landlords in Seattle should keep their houses rented during summer months
### 3. Visitors of Seattle should consider postponing their visit to winter months

![Prices](/assets/what-data-says about-maximizing profit-at-AirBnB-Seattle/prices.png)
Assumption: data in 'calendar.csv' contains past data for prices.

The most profitable time for landlords (and the most expensive one for tenants) was the summer time.

In contrast, during the winter months are average prices the lowest.

### Do you really need to do reconstruction and pause your rental income during the summer?

The code to reproduce results and add your own data processing is published on [my GitHub](https://github.com/JMarcan/stories_hidden_in_airbnb_data).

You can read more about AirBnB data in my another article [Stories Hidden in AirBnB Data](https://jmarcan.github.io/jekyll/update/2020/01/25/Stories-Hidden-In-AirBnB-Data.html).