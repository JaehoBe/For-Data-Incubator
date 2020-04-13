# For-Data-Incubator

This post introduces my analysis on the impact of housing policy on apartment housing prices in Seoul, South Korea.

I will post; 

1) python code for collection of apartment housing transaction data from the Korean government webpage (data.co.kr)

2) cleaned data that I collected (data ready to be used for analysis)

3) summary statistics of the data

4) econometric specification to identify the impact of housing policy on apartment housing prices in Seoul.

6) results and conclusion.



**1. Intorduction**


**2. Brief information about housing market in Seoul, South Korea**

**3. Data** 

I obtain apartment transaction information from Open Data Portal (https://www.data.go.kr/) using personal API and python programming. In the webpage, Korean government provide all types of housing transaction data from January 2006 to now. Below is the part of the data after some cleaning. In the data, apartment name (apt_name), area of unit (area), price of unit (price), address of the apartment (address), coordinates and some other information are included. Because the price is normial, I also collect the Apartment Price Index provided by the KB Bank (https://onland.kbstar.com/quics?page=okbland&QSL=F) to calculate adjusted housing price (price index in 2012 December = 100) so that the prices over time can be comparable across the spaces and over the different time period. Then I calculate adjusted price per square meter (adjusted price / area). I will call this as unit price for the remaining article. Since the price is Korean won, I have to convert won to USD. 1 USD for today (2020-04-13) is around 1,200 Korean won, but for simplicity, I set 1 USD to 1,000 won.

![df](https://user-images.githubusercontent.com/62204139/79158997-281b6980-7d8c-11ea-85e7-6fe1eb551217.png)


In general, the apartment housing price in Seoul increases over time. The price in several Gus are especially high compared to the rest of the Gus. For example, when we look at the price in 2016 January, the unit price in Gangman-Gu (pink line, at the top) is the highest as $1,516 while the unit price in Geumcheon-gu is $499 (green line, at the bottom). It is almost one-third of the price in Gangnam.  

![testplot](https://user-images.githubusercontent.com/62204139/79156383-a295ba80-7d87-11ea-8336-f818b9397e67.png)

Current Korean government, especially the president Moon, Jaein, considers this is a seious problem. The government argues that the high price differences across the Gus are mainly due to speculation and to fight speculation it the Korean government adopt a number of measures such as 2017 Housing Market Stability Measures (so-called 8.2 measures). The measures starts from Auguest 2nd, and it includes policy that restrict the Loan-to-Value (LTV limits) when households purchases apartment housing in 11 Gus (Gangnam-Gu, Seocho-Gu, Songpa-Gu, and 8 more).

Among 25 Gus, 11 Gus are under the LTV limits. So, simplyfy the above graph by dividing Gus into Restricted areas and unrestricted areas.





