# For-Data-Incubator

This post introduces my analysis to understand the impact of housing policy (Loan-to-Value limitation) on apartment housing prices in Seoul, the capital city of South Korea.

During the Camp (if I can successfully being accepted, and get a scholarship) I will post; 

1) python code that I will use to find answers to the above question. 

- data collection from the Korean government webpage (data.co.kr) and cleaning

2) provide summary statistics of the data and the econometric model 

3) results and conclusion.

Before starting the analysis, I describe the general idea about this project and what I want to learn from the camp.

**General idea of the project**

In this project, I want to understand the impat of Loan-to-Value limitation on housing prices. I believe there could be two contributions.
- to real estate investors: Let them know whether the invest in housings located in policy target areas will be worth or not.
- to potential residents: Let them know whether the policy increases or decreases the home prices in target area.

In fact, the policy impact is not predictable and it is the empirical question to be answered. It is partly because 
- the policy itselt does not change the supply of homes in the target areas. If there will be no new housing constructions, the home prices in target areas will continue to increase. 
- at the same time, because the policy restrict money borrowing from the bank, households who plan to buy homes in target areas cannot afford to buy the homes unless they have plenty of cash in hands.

Empirically, there are some issues that need to be addressed to find asnwers. 
- the policy target areas are selected because the home prices in the areas increases more than the rest of the areas.
- This sample selection bias can be addresses using instrumental variables (or finding more data to control the omitted variable bias), or using quasi-experimental approaches.

Currently, I plant to compare the chages in home prices before and after the policy implementation between target areas and the rest of the areas.
- One way is to select homes that are located within certain boundaries. For example, suppose A is the target area and B is the non-target areas. And there is a boundary |. This can be decribed as A | B . Then I select homes in A and B that are close to the boudary |. Combined with panel regression methods, I can control factors that affect the home prices but are not observed to me. 
- Another way is to use propensity score matching so that I can construct a sample from A and B that are similar in their apartment characteristics. Then I use the panel regression to identify the policy impact. This method is called as post-matching panel regression, a type of quasi-experimental analysis that are commonly used in economics and social science. 


**What I want to learn from the camp**

I have trained myself as an economist: I studied Microeconomics theories (as well as environmental economics, natural resource economics, sustainble development) and Econometrics and Computational economics. 

Then, I happened to read the book "Python Machine Learning Blueprints" (https://www.amazon.com/Python-Machine-Learning-Blueprints-Intuitive-ebook/dp/B01CID6IGQ). In fact, even before I read this book, I was curious how different econometrics techniques and the analysis tools that data scientiests use (or people from statistics and computer science). After I read the book, I thought economists and data scienties may do the same works for different purposes. My understanding is that I have trained myself to write a thesis that could be published in the journal while data scientists are supposed to work in industries to make profits for firms. 

I believe the ultimate goal of economists and data scientists is same: "Use data to make some advancement" for economists, the results will advance our understanding about the market and the policy impact and for data scientists the results will make the world a better place to live in by providing proper serivices to the market. 


One thing that I want to share before starting the writing is that, since I have been trained as an economist who needs to present a study outcome that are new (from the perspective of academia), my analysis is focused on to enhance the knowledge about the housing policy impact on housing prices. In fact, one of my personal question is, "How different are the data scientists and economists?" They both use data and I guess most of the time they use the same data in their job. 

As far as I understand, data scientiest are people who studies statistics, or computer science and they are interested in predicition. Economists are interested in understanding socail issues using economics theory and econometrics tool. I think one of the difference between the two is, economists try to justify their model choices using economic theory. I am not sure how data scientiests do to justify their model and this is something that I want to know more from the camp.

Anyway, let's get started!


**1. Intorduction**

Understanding determinants of housing prices is one of the popular astudy areas in economics. Acrroding to the economic theory, the housing price is determined by the supply and demand of housing. In some counties, such as South Korea, the government sometimes involves to the housing market since housing is directly affect the quality of life or even for the survival. So, the housing price is also affected by the policy.

In general, it is impossible to dierectl estimate the replationship between price and quantity because they both are determined at the same time. However, some econometric approaches allow economists to identify the determiants of housing prices; using instrument variable, or using hedonic price approach. Among these two approaches, there exists numerous studies that employ hedonic price approach for its simplicity compared to the other approach (finding a good instrument is difficult), and for its simplicity for interpretation. 

In this project, I basically will use hedonic price approach. That is, the housing price is determined by its characteristics such how big it is, how old is the building, where it locates, whether it is a large size apartment complex or not, which the floor the unit is in. Then, I include policy impact on housing price. One thing to note here is that the policy target apartments are selected. That is, there could be a sample selection bias if I fail to address this issue. For now, 
- I am thinking to use panel regression using the apartments that lies within some distance from boundary that seperate areas into target area (treated) or not (control). So, in some sense, my analysis tries to mimic quasi-experimental approach. 
- Another approach I am thinking is to use propensity score matching to construct comparable apartments that locate in policy target areas and that locate outside the target area.

**2. Data** 

**2-1. Apartment prices in Seoul**

I obtain apartment transaction information from Open Data Portal (https://www.data.go.kr/) using personal API and python programming. In the webpage, Korean government provide all types of housing transaction data from January 2006 to now. Below is the part of the data after some cleaning. In the data, apartment name (apt_name), area of unit (area), price of unit (price), address of the apartment (address), coordinates and some other information are included. Because the price is normial, I also collect the Apartment Price Index provided by the KB Bank (https://onland.kbstar.com/quics?page=okbland&QSL=F) to calculate adjusted housing price (price index in 2012 December = 100) so that the prices over time can be comparable across the spaces and over the different time period. Then I calculate adjusted price per square meter (adjusted price / area). I will call this as unit price for the remaining article. Since the price is Korean won, I have to convert won to USD. 1 USD for today (2020-04-13) is around 1,200 Korean won, but for simplicity, I set 1 USD to 1,000 won.

![df](https://user-images.githubusercontent.com/62204139/79158997-281b6980-7d8c-11ea-85e7-6fe1eb551217.png)


In general, the apartment housing price in Seoul increases over time. The price in several Gus are especially high compared to the rest of the Gus. For example, when we look at the price in 2016 January, the unit price in Gangman-Gu (pink line, at the top) is the highest as $1,516 while the unit price in Geumcheon-gu is $499 (green line, at the bottom). It is almost one-third of the price in Gangnam.  

![testplot](https://user-images.githubusercontent.com/62204139/79156383-a295ba80-7d87-11ea-8336-f818b9397e67.png)

Current Korean government, especially the president Moon, Jaein, considers this is a seious problem. The government argues that the high price differences across the Gus are mainly due to speculation and to fight speculation it the Korean government adopt a number of measures such as 2017 Housing Market Stability Measures (so-called 8.2 measures). The measures starts from Auguest 2nd, and it includes policy that restrict the Loan-to-Value (LTV limits) when households purchases apartment housing in 11 Gus (Gangnam-Gu, Seocho-Gu, Songpa-Gu, and 8 more).

**2-2. Apartment prices in Seoul: restricted and unrestricted areas**


Among 25 Gus, 11 Gus are under the LTV limits. So, simplyfy the above graph by dividing Gus into Restricted areas and unrestricted areas. In graphs, 0 is for unit price in unrestricted areas (14 Gus) and 1 is for unit price in restricted areas (11 Gus). One thing that catch my eyes is that the price increases in restricted areas seems to be greater than the changes in unrestricted areas after the policy in 2017 August.

![targetplot](https://user-images.githubusercontent.com/62204139/79161081-b9d8a600-7d8f-11ea-8529-d5b0a781cf25.png)

According to the Economics Theory, the price of commodity increases when the demand of good increases while the supply of good is fixed, or when the demand is fixed and supply is reduced (of course, price can also increase when both changes, too). Unfortunately, there is not much empty spaces in Seoul for Apartment housing consturction. Also, Korean government strictly restric the new construction of apartment (demolish old short apartment building and construct new tall apartment building). So I can consider the supply of apartment is fixed. 

The government may be right: the price increases in certain areas is because of specualtion. And the government restrict the LTV for potential buyers of apartment in target areas. This may can decrease the demand by potential buyers. However, this policy can also be considered a signal that the apartment in these restricted areas will still be valuable in th future. If the share of potential buyers who can affort the apartment with cash is greater than the potential buyers who want to buy using LTV, then this policy still attract the buyers. At least, the graph shows the policy might ineffective sicne the price increases in the target areas is greater than the other.



**To be continued**


