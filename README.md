# United States Mantal Health Analysis

The aim of this project was to get a better understanding of what the drivers of poor mental health are in the US. The dataset used was from a [2019 US County Health Ranking census](https://www.countyhealthrankings.org/explore-health-rankings/rankings-data-documentation). An array of regression techniques were used to explore how mental health related to the other features in the dataset. The final model highlighted that teen births, food insecurity and single parent households as the the best predictors of poor mental health.

- Email: ravidmalde@gmail.com
- LinkedIn: www.linkedin.com/in/ravi-malde
- Medium: www.medium.com/@ravimalde

## Table of Contents

1. [ File Descriptions ](#file_description)
2. [ Methods Used ](#methods_used)
3. [ Technologies Used ](#technologies_used)
4. [ Executive Summary ](#executive_summary)

<a name="file_description"></a>
## File Descriptions

- index.ipynb: notebook containing all of the preprocessing and modelling.
- analytic_data2019.csv: [2019 US County Health Rankings](https://www.countyhealthrankings.org/explore-health-rankings/rankings-data-documentation) data.

<a name="methods_used"></a>
## Methods Used

- Data exploration
- Data visualisation
- Data cleaning
- Data transformation
- Regression (linear, polynomial, lasso, ridge, ElasticNet)
- Variance inflation factor analysis

<a name="technologies_used"></a>
## Technologies

- Python
- Pandas
- Numpy
- Matplotlib
- Seaborn
- Scikit-learn
- Yellowbrick
- statsmodels

<a name="executive_summary"></a>
## Executive Summary

The dataset contained physical and mental health information on the constituents of 3195 counties across the US. In these counties, the mean number of poor mental health days per month is 3.94; sadly, that's quite a substantial number! The aim of this analysis was to get to the bottom of why people experience poor mental health. This was done using various regression models that establish how certain features in the dataset correlate with poor mental health. The final model found that there were three features that had the most predictive power; these were teen births, food insecurity and single parent households. From this analysis we cannot assume any causation, however it may be indicative that more research should be done in these areas if we are trying to combat poor mental health.

<a name="preprocessing"></a>
### Preprocessing

Many of the columns in the dataset had almost zero entries and therefore have no value in the analysis. One of the first stages in the cleaning process was to remove these, therefore any columns with more than 10% of the data missing were omitted. The remaining null values were then filled in with the median of the state in which the county is situated. This reduced the number of nulls significantly, however there were still a few remaining in cases where there was no information on that feature in the whole state. Columns containing these nulls were then removed from the analysis.

A correlation matrix was then made, and the top 5 features correlating with poor mental health days were selected as the features to continue the analysis with. These were as follows:

- Poor physical health days
- Adult smoking
- Insufficient sleep
- Diabetes prevelance
- Premature age-adjusted mortality

The top 3 features and there relationship to the number of poor physical health days are given in the plot below (diabetes prevalence and premature age-adjusted mortality are not shown as they could not be represented on the same axes):

<h5 align="center">Poor Mental Health Days vs Top 3 Most Correlated Features</h5>
<p align="center">
  <img src="https://github.com/ravimalde/mental_health_analysis/blob/master/images/feature_plots.png" width=750>
</p>

The data was then split into a training set (2095 instances) and a test set (1000 instances). The data was then transformed using StandardScaler. Linear regression, polynomial regression, lasso regression, ridge regression and ElasticNet regression models were then made. KFold cross validation was used to validate each model's performance. The optimum polynomial degree was found to be 3, therefore this was used for the regularised regression models as well.

<h5 align="center">Model Performances</h5>
<p align="center">
  <img src="https://github.com/ravimalde/mental_health_analysis/blob/master/images/model_performances.png" width=750>
</p>
