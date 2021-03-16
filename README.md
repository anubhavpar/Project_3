# Metis Project 3 
Project 3 in the Metis Data Science Bootcamp

Problem statement: 

Based on given set of information, predict if the individual will get vaccinated for H1N1 and Seasonal Flu. 

### Data Definition

* Source - National 2009 H1N1 Flu Survey.
* Total Records - 26K and Features - 36.

The features analyzed are broadly classified into three categories:

* Background 
* Opinion 
* Behavioural 

### Data Analysis - Key Observations

I analyzed the data with the below questions:

**Question - How many people were inoculated with H1N1 vaccine ?**

![](https://github.com/anubhavpar/Project_3/blob/main/Images/H1n1pie.png) 

Less than 25% people were inoculated and imbalance in the adoption of the H1N1 vaccine.

Correlation between People inoculated with H1N1 and Flu Vaccine : 0.337 . Which indicates that **a person getting one vaccine will in theory be inclined to get the other vaccine too and vice-versa**

* 18% of population was inoculated with both vaccines.
* 50% of population got neither of the two vaccines.
* 29% of population got H1N1 but not Seasonal Flu
* 4% of population got seasonal Flu but not H1N1 vaccines.

**Question - Are people with children more likely to get the vaccines ?**

For both H1N1 and Flu vaccines, I observed the below trends:
* I was surprised to find that 50% of the inoculuated populated had 0 children.

**Question - How do people across age groups vary with respect to the vaccination trends ?**

* Younger generation (< 55 years) are less inclined to get vaccinated 
* Older generation(55+ years) are more likely to get generated 


![](https://github.com/anubhavpar/Project_3/blob/main/Images/agetrend.png)

### Modeling

Key Features from Data Analysis:
* Vaccine Concern
* Vaccine Knowledge
* Doctor's Recommendation
* Underlying Medical Condition
* Income Distribution
* Opinion

Baseline Likelihood for H1N1 Vaccine - 10%
Baseline Likelihood for Flu Vaccine  - 22%

I applied various classification algorithms, and I found that tree-based models like XGBoost as well as Logistic Regression performed the best.

![](https://github.com/anubhavpar/Project_3/blob/main/Images/model_comp_roc.svg)

Due to its interpretability and ability to quantitatively translate the inputs to the output, I deployed the Logistic Regression model. My model was biased towards a higher recall as I wanted to capture as many positive scenarios and a loss of precision was not a huge concern for me.

![](https://github.com/anubhavpar/Project_3/blob/main/Images/H1N1_LO_UPD.png)

Files
* SQL_Questions- Folder with the File SQL_new.md which has the solution for the SQL challenges.

* project3_final.ipynb - File which has the code for Data Cleaning, EDA and Model

* Presentation.pdf - Presentation File 
