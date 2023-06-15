# Prediction-of-Power-aoutages-causes
This project using the dataset, at https://www.sciencedirect.com/science/article/pii/S2352340918307182, and giving us diverse informations about power outages, is used to help us understand and predict the causes og major outages.This is a project made at UCSD.
hohoie


## Framing the Problem
Using the dataset informations about major power outages we want to asses the following problem : what are the causes of major outages ?
To answer this question, as the causes are severe weather, intentional attack, system operability disruption, equipment failure, public appeal, fuel supply emergency, and islanding we will answer this question using a multicalss classification model.
I chose this subject because being able to predict the cause of major outages can be very powerful. By powerful, I mean being able to guess the cause instead of a long investigation can be a gain of a huge amount of time and can also help fiw the dammages it caused.

We will focus on the precision result as we want to avoid false positive case, in other words as we want to avoid to predict wrongly the cause of the major outages.

Description of the used features :
The informations we dispose at the time of the prediction are the anomaly level  (ANOMALY.LEVEL (numeric)), the details of the cause (CAUSE.CATEGORY.DETAIL), the type of climate (CLIMATE.CATEGORY), the change of real estate price during the year (PC.REALGSP.CHANGE), population description like urban and rural density (POPDEN_URBAN (persons per square mile), POPDEN_UC (persons per square mile) ,POPDEN_RURAL (persons per square mile) ), the climate in the region (CLIMATE.REGION), the month of the event (MONTH), the price of electricity per month in average (TOTAL.PRICE (cents / kilowatt-hour)), the total electricity consumption in the U.S state (TOTAL.SALES (Megawatt-hour)), the percentage of the total population of the U.S. state represented by the population of the urban clusters (POPPCT_UC), the duration of the outage in minutes (OUTAGE.DURATION (mins)), the number of customers affected by the outage (CUSTOMERS.AFFECTED), the percentage of water area in the U.S. state as compared to the overall water area in the continental (U.S.PCT_WATER_TOT), the percentage of inland water area in the U.S. state as compared to the overall inland water area in the continental U.S. (PCT_WATER_INLAND), 
the amount of peak demand lost during an outage event (in Megawatt) (DEMAND.LOSS.MW (Megawatt)), an the Annual number of total customers served in the U.S. state (TOTAL.CUSTOMERS).


                  where the most importants are anomaly.level which gives us the anomaly level of el nino, the cause categroy detail which tells us what happenend, the climate category and the duration of the outage.

## Baseline Model
The baseline model is a Random Forst classifier which has been trained on the folllowing features : CLIMATE.REGION, MONTH, TOTAL.PRICE (cents / kilowatt-hour), TOTAL.SALES (Megawatt-hour), POPPCT_UC. 
The model uses it's default parameters and will be tested using a cross validation.
About the features, CLIMATE.REGION is one hot encoded as the only categorical (nominal) feature. The others will be left as is.
This gives us an of 44%, which is quite low and can definitely can be improved as we have more datas and can tune our model and try other ones. The precision score is 

## Final Model
We want to predict severe weather, intentional attack, system operability disruption, equipment failure, public appeal, fuel supply emergency, and islanding 
About the features, CLIMATE.REGION and 'CAUSE.CATEGORY.DETAIL' are one hot encoded as the only categorical (nominal) feature.
The features that were added are CAUSE.CATEGORY.DETAIL', 'CLIMATE.CATEGORY','ANOMALY.LEVEL (numeric)','PC.REALGSP.CHANGE', 
                  'POPDEN_URBAN (persons per square mile)','POPDEN_UC (persons per square mile)',
                  'POPDEN_RURAL (persons per square mile)', 'POPPCT_UC', 'OUTAGE.DURATION (mins)', 'CUSTOMERS.AFFECTED', 'PCT_WATER_TOT',
                    'PCT_WATER_INLAND', 'U.S._STATE', 'DEMAND.LOSS.MW (Megawatt)','TOTAL.CUSTOMERS'
                 
In here, Cause.Category.DETAIL gives us the details of the power outage, which is really helpful identifying intentional atacks for example. In our case, intentional attacks is the second most recurrent power outage in our dataset.
ANOMALY.LEVEL gives us the anomaly level of el nino which is an indice of anomaly ususally used to characterised nature event. Regarind this, it definitely helps our dataset identifying power outage caused by severe weather, which is the most recurrent power outage cause of our dataset.
'PC.REALGSP.CHANGE', 'POPDEN_URBAN (persons per square mile)','POPDEN_UC (persons per square mile)', 'POPDEN_RURAL (persons per square mile)', 'POPPCT_UC', describing distribution of population across different areas of the U.S will help us odentify power outages cause by human as public appeal, fuel supply emergency, or intentional attacks. This variable are not correlate athough they are complementary. Knowing this is uselful for example for fuel supply emergency, it will unlikely occur in low density zones compared to high density of population where the deman for fuel is higher.
'PC.REALGSP.CHANGE' describes the Per capita real gross state product in the U.S. state ( in 2009 ), this can be a ENELEVER !!
'CUSTOMERS.AFFECTED' and 'DEMAND.LOSS.MW (Megawatt)' gives us how many customers were affected and the amount of peak demand lost (in Megawatt) which is an indcator of the importance (size, destruction etc... ) of the cause of the major outage, which can help us classify them.
'PCT_WATER_TOT', and 'PCT_WATER_INLAND' describe Percentage of water area in the U.S. state as compared to the overall water area in the continental U.S and Percentage of inland water area in the U.S. state as compared to the overall inland water area in the continental U.S. will help us identify islanding.



The chosen models to perform this are Random forest, Decision Tree,  Gradient boosting, Ada boost and the method GridSearchCV is used to find the best hyperparameters. Let's take a look at each of them.

### Random Forest
The random forest model has a precision of 0.5 which is not very high and we could expect better result with all the new features and the hyperparameters used. In this case the best hyperparameters are the criterion gini, the max depth is 10, and the minimum samples split is 5. Therefore the overall result with the accurachy is 0.71 which is not a low result, especially compared to our baseline model where the accuracy was 0.45 .
The recall score is also quite low with a result of 0.45 .

Here is the confusion matrix : 
<iframe src="confusion_matrix_RF.png" width=800 height=600 frameBorder=0></iframe>

According to this, result look quite good as it seems to be few mistakes.



### Decision Tree
The decisison tree model has a precision of 0.6 with the hyperparameters 'criterion': 'gini', 'max_depth': None, 'min_samples_split': 10 that were chosen by the method GridSearchCV. Here the precision is higher than the previous model with a value of 0.6 against 0.5 for the previous model which is encouraging. According to this, we are getting less false positives, which is what we are looking for. The accuracy score is also higher with 0.76 against 0.71 and the recall value is 0.6 which is also better than the previous model that obtained 0.45 .

Here is the confusion matrix : 
<iframe src="confusion_matrix_DT.png" width=800 height=600 frameBorder=0></iframe>

Here, results look better than the previous model, which is coherent with our results.

### Gradient Boosting 
The gradient boost model provide s a precision of 0.69, an accuracy of 0.81 and a recall value of 0.61 using a learning rate of 0.1, a maximum depth of 3 and the n_estimators is set at 50 (chosen to be the best by GridSearchCV). This model is the best one compared to the previous ones as it obtained the best metrics each time.

Here is the confusion matrix : 
<iframe src="confusion_matrix_GB.png" width=800 height=600 frameBorder=0></iframe>

As our results state, the confusion matrix here is the best we have compared to the previous ones.

### Ada Boost
Finally, Ada boost with a learning rate of 0.1 and the n_estimators set at 200 by GridSearchCV, obtained a surprisingly low precision score of 0.35, and the same goes for the accuracy whiwh is evaluated at 0.68 and the recall score at 0.33. As we are looking for the precision primarly, this is the worst model. And overall the results are not the best ones too, therefore we will not use this model.

Here is the confusion matrix : 
<iframe src="confusion_matrix_AB.png" width=800 height=600 frameBorder=0></iframe>

Again, our results are coherent with the confusion matrix as this one is the worst one.

### Conclusion about final model
The final model chosen is, according to this analyse, the Gradient boost model which provided the best resultsconsidering precision, recall and overall the accuracy. It's F1-score is evaluated at 0.65.
This model is better than our baseline model for the same reason. This can be also explained, by the different feature provided, and not only due to the tunning and the choice of the model as our previous analysis was. But results show that this model is better, making the gradient boost model our final choice model.


## Fairness Analysis

Let's adress the fairness question. Here we would like to know if one of our causes is more likely to be chosen to any given major outage. We will look at the cause severe weather using the anomaly level.
Null hypothesis : The classifier's accuracy is the same for both extreme levels and centered levels of El Nino (El Nino is strong over 1.5 or under -1.5 and moderate or low between these two values), and any differences are due to chance.
Alternative Hypothesis: The classifier's accuracy is higher for extreme anomaly levels.
Test statistic: Difference in accuracy (extreme minus centered).
Significance level: 0.01.

Here is the plot of our result : 

As we can see, it seems that 