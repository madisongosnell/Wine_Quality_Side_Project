# Performing EDA, MLR, 
Exploring wine quality from two datasets containing physiochemical properties of red and white wine samples from the north of Portugal.
# Overview
Although white and red wine may be an easy choice for most simply based on personal preference, both of these wines have different physiochemical properties that make them unique. Using a dataset from UCI Machine Learning Repository containing physiochemical information and an overall recorded quality of the wine, I set out to explore some questions. Can we estimate the overall quality of the wine based on the physiochemical properties? Do white and red wine have statistically different physiochemical properties? These questions, among others, formed the basis of my analysis as I investigated the varieties of wine found in these datasets. 
# Data Collection and Preparation
The data for white and red wine came in different csv files with the same variable names. To create a full datset containing both wine types for comparison, the data was joined by concatenaning along a new variable "Wine Type" that was created in both datasets. The data was then split into a training (80%), validation (10%), and test (10%) datasets that would be exported to csv files to maintain precision amongst platforms. 
### Initial EDA
Intial exploratory data analysis was used to explore: 
*Summary statistics for a variety of variables
*Plotting white vs. red wine in regard to various physiochemical properties
*Correlation matrices between all variables and identifying the types of relationships present
*Plotting grouped box plots to visualize the summary statistics and potential outliers 
### Prior to Modeling
Before any modeling occurred, GVIF values were measured in R to determine if multicollinearity existed. The Density variable tended to have strong multicollinearity issues and is removed from the models. If doing so didn't fix the issues, then they were otherwise resolved in a different manor prior to modeling occurring. 
