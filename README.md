﻿# Chronic-Disease-Risk-Calculator

----

Background:

The CDC has been collecting behavioral, demographic, and disease info on millions of Americans that can be used in a large number of applications. During this project I used this info to explore some of the linkages to chronic disease, though many other outcomes may be predicted for any interested in tweaking the existing code. My goal is to make well-informed models for predicting **diabetes**, **stroke**, and **depression** using an intersection of health behavior theory and feature selection to calculate risk. Anyone willing to fill out 29 question survey can use this tool to estimate their risk for all 3 conditions. 

----

Project Benefit:

The risk calculator tool will give risk of the 3 chronic conditions mentioned above and will be more comprehensive than other population specific calculators found on the web. Due to its increased number of inputs it is likely much more accurate than 5 - 10 question tools found elsewhere. 

----

Methods:

1.	Feature selection: Theory based filtering was used which filtered down hundreds of variables to just 45 questions. To make things easier for the user, random forest variable importance was used to select top 20 to 22 questions (depending on model) of the remaining 45.
2.	Feature interpretability – As mentioned, RF variable importance was used to show top influencers for each model.
3.	Random Forest, GLM Net, and Naive Bayes Ensemble – these are a powerful algorithms with the ability to crunch hundreds of variables and millions of rows in a reasonable amount of time. The ensemble of the 3 did not boost accuracy too much due to the high correlation of the outputs. For this reason I stuck with random forest models for simplicity. This decision is supported by the ROC analysis.
4.	Computational Boosts – R’s `doParallel` and `parallel` packages were helpful to speed up computation time. Accessing the computer's GPU through `gputools` and `keras` will speed up the process even further if SVM or deep learning is used in the future.

These data sets are in csv format on Kaggle and span from 2001 to 2015. Only years 2011, 2013, and 2015 were used however, due to differences in survey structure during other years. Data can be found here: https://www.kaggle.com/cdc/behavioral-risk-factor-surveillance-system/data

Prevalence statistics across time are already available on the CDC's website (https://www.cdc.gov/brfss/brfssprevalence/index.html) and will not be pursued here. 

----

Obstacles Overcome:

* Since this data is collected from phone surveys there was plenty of missing data. Also, this is a social science / health study which meant there is high variability, making models hard to optimize for accuracy. In this case however, accuracy is not the aim, but rather a precise estimate of probability. 
* Survey data was stored in integer values only (though the integers represented categories most of the time) so dealing with that and conversion to missing data (7, 77, 777, 9, 99 and 999 = missing, 97 and 98 were sometimes missing, 8, 88, and 888 were 'None'/0, etc).
* Computing time was an issue, so installing Linux and using the command line to communicate with my university's computer network helped gain computational resources. Downsampling was also used on each respective test and training set of the outcomes to speed things up. Using packages such as `ranger` (`ranger` algorithm) and `klaR` (`NaiveBayes` algorithm) outside of `caret`'s `train()` also greatly sped up the process.

----

Performance:

All performance and diagnostic plots can be seen in the `figures` folder.

* The variable importance plots give an idea of top predictors for each outcome. Example using stroke as an outcome seen below:
![Stroke Variable Importance](https://github.com/tmbluth/Chronic-Disease-Risk-Calculator/blob/master/figures/strk_var_imp.png)

* Models' outputs with high correlation (above 0.9) were not combined for ensemble output since they would not inform each other much. For example when depression was an outcome, prediction probabilities from the GLM and Random Forest models were not combined and averaged, since they gave similar results.
![Depression Models Correlation](https://github.com/tmbluth/Chronic-Disease-Risk-Calculator/blob/master/figures/dep_cor.png)

* ROC analysis produced models with AUCs from 0.76 to 0.84. Only top models based off this metric were selected for each outcome. Below, you can see the Random Forest model barely outperformed other models, so it was used when predicting risk of having diabetes. This of course all depends on the desired cutoff the user wants to specify to determine what risk score is high enough to see a doctor.
![Diabetes ROC](https://github.com/tmbluth/Chronic-Disease-Risk-Calculator/blob/master/figures/db_rocs.png)

----

App:

The user interface is still under construction, but the code is set up to do all the cleaning, modeling, and prediction to get your personal scores. Just download code from here and data from: https://www.kaggle.com/cdc/behavioral-risk-factor-surveillance-system/data
