---
layout: post
title:  "Stories Hidden In AirBnB Data"
date:   2020-01-25 17:03:22 +0100
categories: jekyll update
---
[GitHub Repository](https://github.com/JMarcan/stories_hidden_in_airbnb_data/)
![Entire Apartments Listed in AirBnB Prague](/assets/stories-hidden-in-airbnb-data/whole_apartments_listed_airbnb_Prague.png)
The picture shows concentration of listed apartments on AirBnB Prague | source: [insideairbnb.com](http://insideairbnb.com/get-the-data.html)

## Intoduction

### Background
As citizen, tourist and potential landlord, 
I was curious what stories AirBnB data reveals.

Specifically, I asked those two questions:
1. How looks the typical guest using AirBnB services? <br/>Is it foreigner or countryman? Will data reveal a clear segment in which landlords could specialize?
2. Is Airbnb used primary as home sharing with tourists?<br/> 
Or as a business where entire flats are dedicated to tourists disturbing local housing market?

### Data used
To find it out, I've took data from [insideairbnb.com](http://insideairbnb.com/get-the-data.html).<br/>
Inside Airbnb is an independent, non-commercial set of data that allows you to explore how Airbnb is being used in cities around the world.

- 'reviews.csv': dataset contains guest reviews (6 columns x 683884 rows for Prague)
- 'listings.csv': dataset contains info about available listings (106 columns x 14184 rows for Prague) 

I've chosen data of the following cities for further analysis:
- Prague
- Munich
- ~~Zurich~~ (Zurich data were temporary unavailable)
- Geneva
- San Francisco

Data published on 2019 were selected.

## 1. What is the story of a typical guest using AirBnB services?
Is it foreigner or countryman? What is his/her typical age?

### Solution steps:
Here I intended to use unsupervised machine learning to identify guests segments as I did in [this project](https://github.com/JMarcan/unsupervised_learning_customer_segments). However, the only data available about guests seems to be their name in 'reviews.csv'. 

Therefore, the only thing we can find out is whether guests writing reviews are in majority men or women. 

When singer from my hometown Jaromir Nohavica opened his cabaret club, he mentioned that "The secret is to make this place nice for women. When the place is nice for women, they will bring men with them.". Let's check whether it's applicable even here.

As training my machine learning model for this task would be an overkill,
I decided to use available python library [gender_guesser](https://pypi.org/project/gender-guesser/) that can predict whether a provided name is male or female.

### Results:
| city | male_% | female_% | unknown_% | count_of_records |
| Prague | 38.1% | 37.3% | 24.5% | 683884 |
| Munich | 41.4% | 35.3% | 23.2% | 175562 |
| Geneva | 38.6% | 36.5% | 24.8% | 68163 |
| San Francisco | 38.8% | 37.8% | 23.2% | 382156 |

### Conclusion:
You can see that the library identified in each city gender for more than 75% of inputs.

The table does not show significant difference between ratio of males and females writing reviews. Maximum deviation of 6% has Munich where 41% of authors were identified as males and 35% as females. Taking into consideration unknown gender for authors of 23% reviews for Munich, we will assume that the ratio is close to 50:50 as is case for other cities.

Seems the Wisdom "The secret is to make this place nice for women." is not applicable here. If host wants to have good reviews, he needs to take care about both genders equally. Which he should do anyway.

## 2. Is AirBnB used primary as home sharing? Or as business disturbing local housing?

### Solution steps:
To find it out, I've analyzed dataset 'listings.csv'.

To analyze impact on local housing market, I've excluded houses that could be potentially only vacation houses.
Only records with property_type 'Apartment' were taken into consideration.

The ratio of following accommodation types was analyzed:
- 'Entire home/apt' 
- 'Private room' 
- 'Shared room']

### Results:

| city | count_of_entire_homes | entire_home_% | private_room_% | shared_room_% |  total_count_of_records |
| Prague | 9947 | 84.0% | 15.2% | 0.8% | 11842 |
| Munich | 5811 | 57.9% | 40.6% | 1.4% | 10028 |
| San Francisco | 2522 | 72.8% | 24.1% | 3.1% | 3466 |
| Geneva | 1875 | 69.2% | 29.9% | 0.9% | 2711 |

![Pie Chart for the Ratio of Entire Apartments](/assets/stories-hidden-in-airbnb-data/ratio_whole_apartment_rented.png)
- Brown in the pie chart is % of entire homes being rented on AirBnB
- Light grey is % of private rooms
- Dark Grey is % of shared rooms

### Conclusion:
From the table and charts above is clear that on Airbnb are listed mostly entire apartments.<br/>
- Good for tourists as they have many choices where to stay in place of their own.<br/> 
- Not so good for local citizens thought. 
Houses dedicated for short term rentals are often houses not available for long-term rental, making housing in a city less affordable.
This represents problem even for cities as they receive money based on how many people live in a given city.
- The highest number of entire flats available for rent is in Prague where were listed 9947 apartments,
representing 84% of total AirBnB offering there. Seems AirBnB is becoming more professional business platform, than platform for sharing unused spaces with visitors.

Further reading for this topic you can find on [Airbnb vs Berlin](http://airbnbvsberlin.com/).<br/> 
The code to reproduce results and add your own data processing is published on [my GitHub](https://github.com/JMarcan/stories_hidden_in_airbnb_data).

