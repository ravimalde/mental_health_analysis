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
  * [ Preprocessing ](#preprocessing)
  * [ Modelling (Part 1/2) ](#modelling_1)
  * [ Evaluation (Part1/2) ](#evaluation_1)   
  * [ Modelling (Part 2/2) ](#modelling_2)
  * [ Evaluation (Part 2/2) ](#evaluation_2)
  * [ Recommendations ](#recommendations)
  
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

<a name="modelling_1"></a>
### Modelling (Part 1/2)

The data was then split into a training set (2095 instances) and a test set (1000 instances). The data was then transformed using StandardScaler. Linear regression, polynomial regression, lasso regression, ridge regression and ElasticNet regression models were then made. KFold cross validation was used to validate each model's performance. The optimum polynomial degree was found to be 3, therefore this was used for the regularised regression models as well.

<h5 align="center">Model Performances</h5>
<p align="center">
  <img src="https://github.com/ravimalde/mental_health_analysis/blob/master/images/model_performances.png" width=750>
</p>

All of the models performed similarly; the ridge regression was the best performer with an r^2 of 0.872 however it had a 55 features! For this reason the much simpler linear regression model with an r^2 of 0.854 was chosen as it only had 5 features. Model simplicity was regarded as highly impartant as the results must be interpretable so that findings from this analysis can drive policy change. **The linear regression model was then tested on the test dataset and achieved an r^2 of 0.858**.

<a name="evaluation_1"></a>
### Evaluation (Part 1/2)

The model was then evaluated against the assumptions of linear regression. These are as follows:

1. Linear relationships between features and the target variable.
2. Residuals must be normally distributed.
3. There must be no multicolinearity in the data.

The first two assumptions were satisfied. However a variance inflation factor (VIF) analsyis was conducted to determine how much multicolinearity was present in the dataset. Unfortunately, there was a lot. The normal acceptable rate of multicolinearity is approximately 5, however all of the features had VIF factors between the range of 35 and 113. This doesn't affect the accuracy of the model, however it does mean that the coefficients of the features are inflated/deflated and therefore not realistic. Because the purpose of this analysis is to drive policy change, the coefficients of the features are important; this meant that the modelling needed to be re-run with features that had less colinearity.

<a name="modelling_2"></a>
### Modelling (Part 2/2)

New features were selected by conducting a VIF analysis on the top 20 most correlated features with poor mental health. The 5 features with the lowest VIF factor were selected for modelling. These were as follows:

- Unemployment
- Teen births
- Children in single-parent households
- Low birthweight
- Food insecurity

The dataset was then split and transformed in the same way as previously. This time, the polynomial degree chosen was 2, as despite a polynomial degree of 3 performing marginally better, it was decided that the simplicity of a degree of 2 is of more importance (for the sake of interpretability). The model performances on the validation dataset are given below:

<h5 align="center">Model Performances</h5>
<p align="center">
  <img src="https://github.com/ravimalde/mental_health_analysis/blob/master/images/model_performances2.png" width=750>
</p>

This time, lasso regression was the best performer, and it had only 9 features; therefore it was selected as the final model. **The lasso model was then tested on the test dataset, achieving an r^2 of 0.51**. Although this is a significant drop in performance, due to the reduction in colinearity it means that the feature coefficients are more realistic. 

**The top 5 most important features were identified as**:

- **Number of teen births**
- **Percentage of people living with food insecurity**
- **Percentage of children living in single-parent households**
- **Percentage born at a low birthweight**
- **Percentage of unemployment**

<a name="evaluation_2"></a>
### Evaluation (Part 2/2)
 
As the chosen model was now Lasso regression with a polynomial degree of 2, we no longer needed to check that the relationships between the features and poor mental health days were linear, and the multicolinearity had already been dealt with through the VIF Analysis. Lastly we had to check that the distribution of residuals was normal; this was done using the yellowbrick library. The plot presented below shows that the residuals from both the training and test datasets follow an approximately normal distribution:

<h5 align="center">Model Performances</h5>
<p align="center">
  <img src="https://github.com/ravimalde/mental_health_analysis/blob/master/images/residuals_distribution.png" width=750>
</p>

<a name="recommendations"></a>
### Recommendations

In this analysis no policies were put forward to reduce the number of poor mental health days, we'll leave that to the experts. However, this analysis gives the policy makers a place to start. They should investigate whether or not these features simply correlate with poor mental health or if the relationship is causal. 


