# Performing EDA, Statistical Tests, and Multiple Linear Regression modeling on Wine Type Quality datsets
Exploring wine quality from two datasets containing physiochemical properties of red and white wine samples from Northern Portugal.
# Overview
Although white and red wine may just be a drink choice for most, the two types differ in their physiochemical properties making them each quite unique. Using datasets from UCI Machine Learning Repository, which contain physiochemical information and overall recorded quality of the wine samples, several key questions were identified: 
* Can the overall quality of the wine be estimated based on its physiochemical properties?
* Do white and red wine have statistically different physiochemical properties?

These questions, among others, formed the foundation of this analysis as the wine varieties began to be investigated.
# Data Collection and Preparation
The data for white and red wine came in different csv files each containing identical variable names and no missing values. To allow for direct comparison, a new categorical variable, "Wine Type", was created. This variable was used to concatenate the two datasets and form a new "all_wine" csv file for analysis. 

The data in this "all_wine" csv file was then split into a training (80%), validation (10%), and test (10%) datasets that would be exported to csv files to maintain precision amongst programming platforms. 
### Initial EDA
Initial exploratory data analysis was conducted to examine: 
* Summary statistics for a variety of variables
* Plots comparing white and red wine based on various physiochemical properties
* Correlation matrices between all variables to identify potential relationships
* Grouped box plots to visualize summary statistics and potential outliers 
### Prior to Modeling
Before modeling, Generalized Variance Inflation Factor (GVIF) values were calculated in R to determine if multicollinearity existed. The Density variable tended to have strong multicollinearity issues and is removed from the models. If doing so didn't fix the issues, then they were otherwise resolved in a different manner prior to modeling occurring. 
# Analysis
Analysis includes: 
* Initial investigation for identifying statistical differences between wine types with physiochemical properties of interest
* Testing of assumptions for statistical tests, multiple linear regression models, multicollinearity, and more
* Developing regularized regression models (Ridge and LASSO) for pH as well as for quality
* Developing a Multiple Linear Regression Model (MLR) to explore the relationship between the pH of a wine and a variety of significant physiochemical properties
* Developing a Multiple Linear Regression Model to explore the relationship between the quality of a wine and a variety of significant physiochemical properties
### Identifying Statistical Differences
To determine whether statistically significant differences existed between red and white wines in terms of physiochemical properties, the following steps were performed for multiple variables:
1. Visualized data with grouped box plots
2. Assessed Normality using the Shapiro-Wilk Test and QQplots for visualization
3. Checked for heteroscedasticity using a Levene Test
4. Plotted overlaid histograms to observe symmetry and the distribution's shape
5. Conducted a Kruskal-Wallis Test when the normality assumptions were violated
6. Drew conclusions based on normality, homoscedasticity, and the overall data distribution
### Regularized Regression Models
By testing GVIF values in R, multicollinearity in the dataset is identifiable. Regularized regression models, such as Ridge and LASSO, may be used to continue with modeling without having to remove any variables from the dataset. To use such models, dummy coding of categorical variables must occur first. Followed by specification of the x and y variables (identifying that the response, y, should either be pH or quality in this circumstance). Then, the ridge or lasso model should be fit to the data and plotted to identify what penalty must be used. After the regularized regression models have been applied to the training, validation, and test datasets, the Mean Absolute Error (MAE) may be calculated for both models to use for comparison between each other as well as with the Multiple Linear Regression model MAE. 
### Multiple Linear Regression Models
As with the regularized regression models, MLR will be done to explore the relationships for both the pH and quality of the wine based on a variety of other physiochemical properties. pH and quality were chosen for interest simply because they seemed likely to be most impacted by changes in the different estimates and variables. The steps for creating and executing a MLR model are as followed:
* Perform GVIF test in R to determine if multicollinearity exists. Remove necessary variables to do away with any multicollinearity
* Perform stepwise selection methods such as forward and stepwise to form a beginning model based on AIC (may also use BIC or p-value)
* Remove insignificant variables from the model one-by-one based on the p-value for each variable from the model summary
* Test Normality of the model residuals via a QQplot and Shapiro-Wilk Test
* Account for Non-Normality by applying a transformation (typically log for interpretability)
* Test for equality of variances by using a scatterplot of the residuals and performing the Spearman's Rank Correlation
* Account for heteroscedasticity with a transformation (as above) and/or applying robust standard errors

These steps will be applied to both the training and validation datasets, but only the test dataset will simply run the final model and give valuable statistics about model performance including adjusted R^2 and MAE. 
# Conclusion and Future Opportunities
From the intial exploratory data analysis, the grouped box plots displayed that there appeared to be some potential outliers for variables such as quality, residual sugar, citric acid, and more. In addition, the correlation matrix across all variables in the dataset displayed there was a relatively strong postive correlation between free sulfur dioxide and total sulfur dioxide. There was also a relatively strong negative correlation between density and alcohol. In the heatmap below we are able to visualize those values as anything close to 1 or -1 will be a darker shade of red (positive relationship) or blue(negative relationship). The 1's down the center represent a variable correlated with itself, which of course will be 1 as they are identical.

<img width="643" height="550" alt="image" src="https://github.com/user-attachments/assets/01e1fd3e-fc86-41a4-bf63-10128303ed2f" />

Figure 1: Heatmap of Variable Correlation Matrix


As seen in the MAE model comparison, for both the pH and quality models, the ridge regression model had the smallest mean absolute error. The ridge regression models' aligned more with the actual dataset than the others presented. The reasoning behind such result could likely be due to the multicollinearity seen in the data. Although, the multiple linear regression models that removed conflicting variables causing multicollinearity concerns were a close second behind the ridge regression models in MAE performance.

Table 1: pH MAE Comparisons
| Model | MAE |
|---|---|
| pH Multiple Linear Regression Model | 0.097 |
| pH Ridge Regression Model | 0.075 |
| pH Lasso Regression Model | 0.128 |

Table 2: Quality MAE Comparisons 
| Model | MAE |
|---|---|
| Quality Multiple Linear Regression Model | 0.561 |
| Quality Ridge Regression Model | 0.560 |
| Quality Lasso Regression Model | 0.671 |

Such models created can likely provide a relatively good estimation of pH or quality based on the variables included in the final model, but more data and variables could result in a higher performing model. Below are the final two models for pH and quality (would be exponentiated to get the interpretable value): 

* log(pH) = 1.3547 - 0.0578(Wine Type) + 0.0009(Alcohol) - 0.0208(Fixed Acidity) + 0.0409(Sulphates) - 0.0013(Residual Sugar) - 0.0239(Citric Acid) + 2.196e-05(Total Sulfur Dioxide) - 0.2259(Chlorides)

* log(Quality)=1.2455−0.0388(Wine Type)+0.0548(Alcohol−0.2985(Volatile Acidity)+0.095(Sulphates)+0.004(Residual Sugar)+0.001(Free Sulfur Dioxide)−0.0003(Total Sulfur Dioxide)−0.2145(Chlorides)

The tables below show the model performance results on the test datasets. The pH model had a better adjusted R^2 value than the quality model, which means it was able to explain the data slightly better. 

Table 3: pH Regression Model Summary
| Regression Statistics |       |
|-----------------------|-------|
| R-Squared             | 0.373 |
| Adj R-Squared         | 0.369 |
| F-Statistic           | 106.2 |
| AIC                   | -4703 |
| Observations          | 1300  |

Table 4: Quality Regression Model Summary
| Regression Statistics |       |
|-----------------------|-------|
| R-Squared             | 0.263 |
| Adj R-Squared         | 0.258 |
| F-Statistic           | 57.52 |
| AIC                   | -1627 |
| Observations          | 1300  |

For the quality MLR model I created a simple Tableau dashboard that dynamically updates based on inputs for the significant physiochemical properties from the model. I've attached an image of such dashboard below along with the link to access it for yourself. 
<img width="912" height="559" alt="Screenshot 2025-09-24 at 6 25 55 PM" src="https://github.com/user-attachments/assets/a42f71bd-3606-411d-a921-1de27b9e5b78" />

Link: https://public.tableau.com/shared/RRF2QQGJG?:display_count=n&:origin=viz_share_link

### Next Steps
The most logical next steps I see for improving the estimation of these variables and others include: 
* Collecting more data to have a greater dispersion of data points that will improve model performability and reduce variance of the model
* Introduce additional physiochemical or external factors that may correlate with the response variables and produce greater predictability
* Performing hyperparameter tuning on the regularized regression and multiple linear regresion models to optimize model performance

These steps could all be beneficial to improving and building upon the models created and formed during this project. They will likely all contribute to improving model performance and reduce variance and multicollinearity within the data. 

Link to original datasets: https://archive.ics.uci.edu/dataset/186/wine+quality 
