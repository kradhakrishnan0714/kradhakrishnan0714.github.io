---
layout: single
title: "Exploratory data analysis on 1994 census database"
classes: wide
date: 2025-03-08
---

**Description**: Exploratory data analysis is done on the 1994 census database. This dataset contains 14 features such as Age, workclass, education, Martial status etc. These features are a mix of continuous & discrete variables. Response variable is individual's income. This is provided as binary to denote whether income is greater than $50,000 or not.  

This dataset will be used to predict whether a person makes over 50K a year using classification algorithms. We expect to use the different metrics such as sensitivity, Specificity, Positive Predictive Value, Negative Predictive Value, AUC-ROC to measure the effectiveness of prediction algorithm.  

**Tech Stack**: Python

## Source Code & Insights from Analysis
[View on GitHub](https://github.com/kradhakrishnan0714/Machine-Learning-I/blob/main/Census%20dataset-EDA.ipynb)

## Important Visualizations

<img 
  src="/assets/visuals/census_age_distribution_by_income.png" 
  alt="Age distribution by income" 
  style="max-width: 100%; height: auto;" />

<img 
  src="/assets/visuals/census_data_corrrelation_matrix.png" 
  alt="census_data_corrrelation_matrix" 
  style="max-width: 100%; height: auto;" />

<img 
  src="/assets/visuals/census_data_distributions.png" 
  alt="census data distributions" 
  style="max-width: 100%; height: auto;" />

<img 
  src="/assets/visuals/census_data_feature_importance.png" 
  alt="Feature Importance" 
  style="max-width: 100%; height: auto;" />