# BostonPCA Output
### Jonathan Morris

2024-09-02

## Overview
This PCA analysis was conducted using data from https://data.boston.gov on Boston housing market data from 2024. Analysis was conducted using Principle Component Analysis from the package longpca. 
Longpca is an open source package developed by Karl Rohe at the University of Wisconsin-Madison. We describe methods of analysis and then highlight variables of interest.

## Run the PCA:
formula = TOTAL_VALUE ~ (YR_BUILT & ZIP_CODE)*(LIVING_AREA & BED_RMS & LAND_SF)
im = make_interaction_model(Boston, formula)
pcs2a = pca(im, k = 6)

## Unpack Findings. First we look at how our model treated Year Built (YR_BUILT).
### Plot data against year built:
![Test6](https://github.com/user-attachments/assets/b1106911-2363-4b2e-8a01-b7db1307d0e4)
Overall we see that our loadings increase as the house was built more recently. 
However, our loadings are small suggesting that date produced doesn’t excert a ton of influence on our home prices. 
The exception to this comes in for houses built around the late 50’s - 60’s where we see a decline in housing prices. 
My hypothesis is that this housing price decrease reflects the legacy of suburban growth during the same era. 
We saw a massive influx of suburban developments during the 60s. Many of these homes were cheaply built and put up in mass developments.
Could be a possible reason we see homes built in this era with lower value. 
Essentially these plots suggest that our homes built more recently contribute more significantly to variability in home price (with the 50's and 60's as an exception).

## Let's look at our data according to zip code:
![Test7](https://github.com/user-attachments/assets/111afcf2-639b-4a93-8325-2aeeeb07cbb7)
We see an interesting trend here. It seem like these PC Rows pick up on the price of housing based on zip code. 
We see higher PC values in neighborhoods that have higher home price and also have lower numbers of occupied housing units, thus implying that the there is greater variability in pricing in these neighborhoods. 
Could also imply that the houses are bigger. Loadings are also somewhat small. 

If we look at a raw map of median home price by zip code in Boston, we see that our PCA captured similar trends. 
High variability in housing price based on zip codes where house prices are high and homes are less densely packed.
![Test8](https://github.com/user-attachments/assets/490268a9-1dfa-4343-8112-80539c6396c8)
![Test9](https://github.com/user-attachments/assets/5485f38c-4779-45ef-9831-4ba9ef4ef4f1)


## Now let's look at the PCA according to columns.
### Start with land square footage:
![Test10](https://github.com/user-attachments/assets/6115c02e-914e-4129-8f48-db83aee99c55)
It looks almost like we see an upward trend with our data. As our square footage gets bigger, do our loadings get higher? 

Let’s test with a linear model:
Call:
###### lm(formula = LAND_SF ~ loadings, data = PCS_Long)
###### Residuals:
######      Min       1Q   Median       3Q      Max 
###### -1142.63  -309.64    20.24   343.36   726.38  
###### Coefficients:
######             Estimate Std. Error t value Pr(>|t|)    
###### (Intercept) 1272.623      1.434 887.499  < 2e-16 ***
###### loadings       3.767      1.298   2.902  0.00371 ** 
###### ---
###### Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
###### Residual standard error: 426.5 on 88834 degrees of freedom
###### Multiple R-squared:  9.478e-05,  Adjusted R-squared:  8.352e-05 
###### F-statistic:  8.42 on 1 and 88834 DF,  p-value: 0.003711

We see that this assumption is likely correct. Homes with very high square footage account for a huge amount of variability in home price and exert a lot of influence on our data overall. 
This suggests that land square footage is a key variable of interest when predicting home price. This is quite obvious however.

### Now let’s look at living area square footage and number of bedrooms:
![Test11](https://github.com/user-attachments/assets/bb65c769-2a91-4939-92a0-0f18f4482eef)
![Test12](https://github.com/user-attachments/assets/db812030-f007-4d1e-acde-ce166c21923a)
We see the same trend from land square footage for living area square footage. However, we see that the houses with lower numbers of bedrooms have much higher loadings. Could be due to the fact that only a small number of houses have more than five bedrooms. Overall, this suggests that houses with 5 for less bedrooms contribute significantly to the variance seen in our data. This makes sense as Boston is an older American city, so land value is especially high. There is a lot of variance in the price of houses based on bedrooms because high priced houses may be costly for factors like location, and may often have limited space.

## Overall Interpretation:
Recall our model once more: formula = TOTAL_VALUE ~ (YR_BUILT & ZIP_CODE)*(LIVING_AREA & BED_RMS & LAND_SF).

It seems as though the year the home was built and zip code provide interesting and informative predictions toward the total value of the home. 
We see that more recent homes are often worth more. There is also an interesting period in the 50s - 60s where homes produced in that era are now worth less. 
Overall we see differences in price based on zip code. It’s clear that our model picked up on variability in home prices based on neighborhoods where homes are more valuable. 
However, since Boston is a pretty expensive city, and our PCA only looked at zip code, our model suggests that there isn’t a ton of variability in home price based on zip code. 
I am sure this would change if we looked somewhere else or used neighborhood as a term instead of zip code.

We see that our PCA suggests that overall land square footage and living area square footage account for major variability in our data. 
Homes with high square footage account especially for the variability we see in home price. We see this trend change when we look at number of bedrooms. 
Homes with an extremely high number of bedrooms don’t account for much variability while homes with 5 or less bedrooms account for a ton of variability in our data.

Overall, a successful method for uncovering which variables are most important here. 
If I were analyzing this data further I’d look into how differences in the size of a home interact with location and avg number of residents to predict home price. 
I’d also like to look at these trends over time to see how housing prices have fluctuated over the past 10 years.



