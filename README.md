# For-Data-Incubator

This post introduces my analysis on the impact of housing policy on apartment housing prices in Seoul, South Korea.

I will post; 

1) python code for collection of apartment housing transaction data from the Korean government webpage (data.co.kr)

2) cleaned data that I collected (data ready to be used for analysis)

3) summary statistics of the data

4) econometric specification to identify the impact of housing policy on apartment housing prices in Seoul.

6) results and conclusion.



1. Intorduction


2. Brief information about housing market in Seoul, South Korea

I get the Apartment transactoin price from 2006 Januray to 2019 December from Open Data Portal (https://www.data.go.kr/) using personal API and python programming. Below is the part of the data, and in the data, I have apartment name (apt_name), area of unit (area), price of unit (price), address of the apartment (address), coordinates and some other information. I also collect the Apartment price index provided by the KB Bank (https://onland.kbstar.com/quics?page=okbland&QSL=F) and I calculate the adjusted housing price so that the prices over time can be comparable across different time period. 



Apartment housing price in Seoul increases over time. The price in several Gus are especially high compared to the rest of the Gus. For example, when we look at the price in 2016 January, the price in Gangman-Gu is 

(we can see that the price per unit area in Gangam-Gu, pink line, is the highest and it is almost three-times higher compared to the price in  in Gangnam-Gu,Pink line on the upper area). 
![testplot](https://user-images.githubusercontent.com/62204139/79156383-a295ba80-7d87-11ea-8336-f818b9397e67.png)

In 2017 August, Korean government announced that eleven Gus (including Gangnam-Gu) will 


