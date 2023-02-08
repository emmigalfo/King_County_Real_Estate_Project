# King County Real Estate 
Project for phase 2 of Flatiron's data science program


![seattle sky line](./Photos/Seattle-Skyline2.jpeg)

**Author:** [Emmi Galfo](mailto:emmi.galfo@gmail.com)

## Overview 

This project looks at factors that influence price in the King County real estate market. Through linear regression modeling, predictors such as square footage, number of bedrooms, number of bathrooms, condition, grade, and views are factored in to determine their effects on housing price. The project ultimately finds that more bedrooms are associated with lower prices, and conversley, more bathrooms and square footage are each assiciated with higher home prices all else constant.  The better views are linked with higher home prices as well as better grade and home conditions all else constant. 

## Business Problem

A real estate agent is helping a couple find a house in King County. In order to stay within a set budget, the couple is interested in seeing how different factors affect overall price. This project looks at the following variables and their affect on the price of buying a house in King County, Washington:
*  Square Footage
*  Number of Bedrooms
*  Number of Bathrooms
*  House Condition 
*  Quality of the View

## Data Understanding

For this project, data was gathered and used from the King County assessor's website. Below are descriptions taken from the database describing each column used in this project.  

### Column Names and Descriptions for King County Data Set

* `id` - Unique identifier for a house
* `price` - Sale price (prediction target)
* `bedrooms` - Number of bedrooms
* `bathrooms` - Number of bathrooms
* `sqft_living` - Square footage of living space in the home
* `view` - Quality of view from house
  * Includes views of Mt. Rainier, Olympics, Cascades, Territorial, Seattle Skyline, Puget Sound, Lake Washington, Lake Sammamish, small lake / river / creek, and other
* `condition` - How good the overall condition of the house is. Related to maintenance of house.
  * See the [King County Assessor Website](https://info.kingcounty.gov/assessor/esales/Glossary.aspx?type=r) for further explanation of each condition code
* `grade` - Overall grade of the house. Related to the construction and design of the house.
  * See the [King County Assessor Website](https://info.kingcounty.gov/assessor/esales/Glossary.aspx?type=r) for further explanation of each building grade code

Most fields were pulled from the [King County Assessor Data Download](https://info.kingcounty.gov/assessor/DataDownload/default.aspx).

## Data Preparation

### Target: 
The target variable for this project is __price__. The clients want to know how much certain factors will contribute to the overall price of a house. 
Let's look at the correlation each factor has with price. 

![Heatmap of correlations](./Photos/heatmap.png)

> __Correlations with Price from Highest to Lowest__ \
price-            1.000000 \
sqft_living-      0.608521 \
sqft_above-       0.538651 \
bathrooms-        0.480401 \
sqft_patio-       0.313409 \
bedrooms-         0.289204 \
sqft_garage-      0.264169 \
sqft_basement-    0.245058 \
floors-           0.180576 \
yr_built-         0.096013 \
sqft_lot-         0.085730 \
yr_renovated-     0.084786 \
lat-              0.063632 \
long-            -0.022509 

Looking at the correlations above I see two things:
* The heatmap shows that square footage of living space and above grade square footage are strongly correlated with each other. This makes sense; it follows that the more above grade square footage, the more square footage of living space. In order to avoid multicollinearity, only one of these factors should be used. Square footage of living space has a higher correlation than above grade square footage. So when looking at square footage, I will use this one as one of the predictors.  
* Of the factors that the clients are intersted in looking at, square footage of living space has the highest correlation with price. This is a good place to start for the baseline model. 

### Predictors: 
The predictors that the clients are most intersted in are __square footage of living area, number of bathrooms, number of bedrooms, condition of the home, grade of the home, and quality of views__. 

## Baseline Model: Square Footage

I will use square footage of living space as my factor for my baseline model because it is relevent to the business problem and has the highest correlation with the target variable, price. 

![baseline model 1](./Photos/Model_1_results.png)
##### Interpretation
The model is significant overall. The model explains 37% of the variance in price. 
The constant and coefficient are both significant. 

The constant is a bit non-sensical at this point stating that a home with no square footage would cost -74k USD. 

The model shows that for every square foot in living area, the price goes up by about 560 USD. 
This may seem expensive but it is consistent with a mean square footage of 2,112 and a mean price of 1.1 million USD.

Overall, I don't think this is a very good model because it only explains 37% of the variance.

## Second Model: Square Footage, Bedrooms, Bathrooms

### Adding additional numerical factors: 
Three of the six factors we are considering are numeric: square footage, bedrooms, and bathrooms. 

![scatterplots of numerical factors with price](./Photos/Numerical_factors_scatter.png)

Just by looking at the graphs, it seems that square footage of living space, bedrooms, and bathrooms have positive correlations. Though bedrooms looks a little weaker. This is consistent with what the heatmap showed previously. Let's look at the linear regression model. 

![Linear Regression model 2](./Photos/Model_2_results.png)

##### Interpretation
The model is significant overall. The model explains ~39% of the variance in price. The constant and coefficients are all significant. 

The constant shows a starting value of 201k USD. For King County, this seems reasonable. 

The model shows that for every additional sqare foot in living area, the price goes up by about 616 USD all else constant. 

Adding bedrooms brings down the price of the house by about -162K per bedroom, all else constant. 

For each bathroom the house price goes up by about 68k per bathroom, all else constant. 

Overall, the model seems weak with only explaining 39% of variance in price.

## Third Model: Square Footage, Bedrooms, Bathrooms, Grade, View, Condition
The categorical data we will be looking at includes grade, condition, and views. 
Let's look at each variable and the descriptions from the data base. 

### Grade
__Description:__ \
Represents the construction quality of improvements. Grades run from grade 1 to 13. Generally defined as:

1-3 Falls short of minimum building standards. Normally cabin or inferior structure.

4 Generally older, low quality construction. Does not meet code.

5 Low construction costs and workmanship. Small, simple design.

6 Lowest grade currently meeting building code. Low quality materials and simple designs.

7 Average grade of construction and design. Commonly seen in plats and older sub-divisions.

8 Just above average in construction and design. Usually better materials in both the exterior and interior finish work.

9 Better architectural design with extra interior and exterior design and quality.

10 Homes of this quality generally have high quality features. Finish work is better and more design quality is seen in the floor plans. Generally have a larger square footage.

11 Custom design and higher quality finish work with added amenities of solid woods, bathroom fixtures and more luxurious options.

12 Custom design and excellent builders. All materials are of the highest quality and all conveniences are present.

13 Generally custom designed and built. Mansion level. Large amount of highest quality cabinet work, wood trim, marble, entry ways etc.


>__Grade Mean Prices from Highest to Lowest:__ \
 13 Mansion-       7.399048e+06 \
 12 Luxury-        5.088029e+06 \
 11 Excellent-     3.542308e+06 \
 10 Very Good-     2.341987e+06 \
 9 Better-         1.586307e+06 \
 1 Cabin-          1.352500e+06 \
 8 Good-           1.083520e+06 \
 7 Average-        8.201276e+05 \
 6 Low Average-    6.539746e+05 \
 4 Low-            6.388057e+05 \
 5 Fair-           6.186138e+05 \
 3 Poor-           4.644615e+05 \
 2 Substandard-    3.025000e+05 

*A note for modeling: one column needs to be dropped to avoid the dummy trap. I will drop cabin as it's one of the most inferior grades and is most likely not what the clients would be looking for. When it's dropped, cabin will become the reference column when looking at grade.*

### Condition
__Description:__ \
Relative to age and grade. Coded 1-5.

1 = Poor- Worn out. Repair and overhaul needed on painted surfaces, roofing, plumbing, heating and numerous functional inadequacies. Excessive deferred maintenance and abuse, limited value-in-use, approaching abandonment or major reconstruction; reuse or change in occupancy is imminent. Effective age is near the end of the scale regardless of the actual chronological age.

2 = Fair- Badly worn. Much repair needed. Many items need refinishing or overhauling, deferred maintenance obvious, inadequate building utility and systems all shortening the life expectancy and increasing the effective age.

3 = Average- Some evidence of deferred maintenance and normal obsolescence with age in that a few minor repairs are needed, along with some refinishing. All major components still functional and contributing toward an extended life expectancy. Effective age and utility is standard for like properties of its class and usage.

4 = Good- No obvious maintenance required but neither is everything new. Appearance and utility are above the standard and the overall effective age will be lower than the typical property.

5= Very Good- All items well maintained, many having been overhauled and repaired as they have shown signs of wear, increasing the life expectancy and lowering the effective age with little deterioration or obsolescence evident with a high degree of utility.

> __Condition Mean Prices from Highest to Lowest:__ \
Average-      1.134336e+06 \
Very Good-    1.130726e+06 \
Good-         1.053242e+06 \
Fair-         7.799337e+05 \
Poor-         6.482829e+05 

*Again, a column will need to be dropped in order to avoid multicollinearity. The column that makes the most sense to be dropped is the poor condition because our clients will not be interested in these homes. In the model, poor condition will be the reference column.* 

### View
__Description:__ \
Quality of view from house
Includes views of Mt. Rainier, Olympics, Cascades, Territorial, Seattle Skyline, Puget Sound, Lake Washington, Lake Sammamish, small lake / river / creek, and other

> __View Mean Prices from Highest to Lowest:__ \
Excellent-    2.994147e+06 \
Fair-         1.742069e+06 \
Good-         1.736416e+06 \
Average-      1.451993e+06 \
None-         1.018607e+06 

*The view column, none, will be dropped to avoid the dummy trap. The column, none, will become the reference for view.*

![Linear Regression model 3](./Photos/Model_3_results.png)

##### Interpretation
The model is significant overall. The model explains about 50% of the variance in price. 

The constant and most of the predictors are significant at an alpha level of 0.05. 

## Fourth Model: Remove Outliers in Price

*Adding the categorical data increased the R-squared value from ~39% to ~50%! 
This is definitely a step in the right direction.  
Outliers can throw off our data. We should look to see if there are any outliers, and then remove them to help improve the model.* 
