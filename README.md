# PREDICTING REPO PROGRAMMING LANGUAGE BY THE README<a name="top"></a>
![]()

by: Richard Macken & Dan Churchill

***
[[Project Description](#project_description)]
[[Project Planning](#planning)]
[[Data Dictionary](#dictionary)]
[[Data Acquire and Prep](#wrangle)]
[[Data Exploration](#explore)]
[[Modeling](#model)]
[[Conclusion](#conclusion)]
[[Steps to Reproduce](#reproduce)]
___



## <a name="project_description"></a>Project Description and Goals:
The purpose of this project is to build a model that can predict the main programming language of a repository, given the text of the README file.
    
  Goal 1: Generate a list of repositories programatically using web scraping techniques<br>
  Goal 2: Acquire and Explore the natural language data that we have acquired<br>
  Goal 3: Build a function that will take in the text of a README file, and tries to predict the programming language


[[Back to top](#top)]

***
## <a name="planning"></a>Project Planning: 


### Project Outline:
- Acquire, clean, and prepare data from the Github Website using their API and a personal access token
    - Data was aquired from the top 150 results from three independent languages (C+, Python, Java) where the readme is in english
- Split data and perform initial data exploration to determine what features will be usefull for modeling
- Train multiple classification models and evaluate on train dataset
- Choose the model with that performs the best and evaluate that single model on the test dataset
- Implement a function that accepts text from a readme file and tries to predict the programming lanquage

[[Back to top](#top)]
***

## <a name="dictionary"></a>Data Dictionary  

| Target Attribute | Definition | Data Type |
| ----- | ----- | ----- |
| language | the primary programming language of the repository | string |


---
| Feature | Definition | Data Type |
| ----- | ----- | ----- |
| repo | the user id/repo name | string |
| readme_contents | the text from the readme file of the repo | string |
| cleaned	 | the text of the readme file after intial cleaning | string |
| stemmed | the text of the readme file after it has been stemmed | string |


[[Back to top](#top)]

***

## <a name="wrangle"></a>Data Acquisition and Preparation

Data is acquired from the Github website using a personal access token to scrape their website via their API.  The modular functions within the acquire.py file obtain the userdata, clean it to remove encoded characters and save that to another column, stem the words to remove suffixes and save it to it's own column, and then creates one last column of lemmatized data.  The data is then 
into train, validate, and test dataframes in a 60% / 20% / 20% ratio.



[[Back to top](#top)]

![]()


*********************

## <a name="explore"></a>Data Exploration:

### Locate properties
The first step was to use the fips value to identify the county each property was located in.  There were three values: 6037, 6059, 6111 corresponding to Los Angeles County, Orange County, and Vetura County repsectfully, all in California.

### Exploring Tax Value
Next we look at the distribution of the target variable, Tax Value.  The values appear somewhat normal, although right-skewed.  Values appear highest in Los Angeles county, followed by Orange County and Ventura County.  Testing using variance testing allows us to reject the null hypothesis that values are the same in all three counties.

### Exploring Specific Location
Exploring the values graphically on a map show that there are clusters of higher and lower valued homes within each county.  For this reason the tract column is used in modelling.  Ideally, this would be used as a categorically, but there are too many unique values to categorically encode.

### Correlation of Numerical Features
Using a correlation matrix we see that square footage is the most correlated to tax value, followed by bedrooms and bathrooms.  However, because bedrooms and bathrooms are highly linearly correlated to square footage we do not want to use them directly in the model as a numerical value.  I researched if an adjusted square footage was feasible, but according to the National Association of Homebuilders the percentage of square footage allocated to bedrooms and bathrooms remains constant at 40% irrespective of home size [(Source)](https://bestinamericanliving.com/2016/08/where-builders-place-their-space-2/)
  </a>
</p>  For this reason I explored ways to use bedrooms and bathrooms categorically.

### Exploring Number of Bathrooms
Using a box plot we look graphically at the how the number of bathrooms affects property value.  We see that the effect appears linear up to 3.5 bathrooms as which point it jumps sharply and levels off.  We created a categorical variable to capture properties that had more than 3 bathrooms for use in modelling.  And looked at the populations of each subset graphically.  While three or less bathrooms were consistant with the mean, properties with more than three bathrooms had a significantly higher property value.  This was confirmed by a T-Test where we rejected the null hypothesis that the values were equal.  

There were similar observations in garage size and a similar category was created for garages that had three to five parking spaces.  Bedroom count showed no similar trend, so we used it as a categorical variable. 



### Takeaways from exploration:
- We've identified that location is vital to property value
- Square footage, bedrooms, and bathrooms are all key drivers of property value, but are correlated to each other and must be used differently
- High number of bathrooms is significant, as are large garages which can be used as categories

[[Back to top](#top)]

***

## <a name="model"></a>Modeling:

#### Modeling Results
| Model | RMSE on train | RMSE on validate | R2 score |
| ---- | ---- | ---- |---- |
| Baseline | $233,115.06 | N/A | N/A |
| Linear Regression (OLS) | $198,418.25 | $199,021.70 | 0.2816 |  
| LassoLars | $198,424.85 | $199,017.21 |  0.2816 |
| Tweedie Regressor | $198,944.70 | $199,738.47 | 0.2764 |

 


- The LassoLars model performed slightly better than the OLS and Tweedie Regressor models


## Testing the Model

- Tweedie Regressor model used on Test data

#### Testing Dataset

| Model | RMSE on test | R2 score |
| ---- | ---- | ---- |
| Tweedie Regressor | $198,604.47 |  0.278 |

[[Back to top](#top)]

***

## <a name="conclusion"></a>Conclusion and Next Steps:

- We created a model that was able to predict programming language of a repo with XX% accuracy

- The model performed significantly better when the dataset was restricted to a more narrow set of data

- Location was the largest driver of tax value, followed by square footage

- I attempted to create a function that created a dictionary of models trained for each of the most popular tracts
    - Consolidating results and evaluating proved too difficult to implement in the time given

[[Back to top](#top)]

*** 

## <a name="reproduce"></a>Steps to Reproduce:

In order to reproduce the project follow the steps below:

  - Download the acquire.py, prepare.py, and final_report.ipynb files
  - Generate a personal access token at https://github.com/settings/tokens 
  - Add your own env.py file to the directory containing your own 'github_token and 'github_username
  - Run the final_repot.ipynb notebook

[[Back to top](#top)]
