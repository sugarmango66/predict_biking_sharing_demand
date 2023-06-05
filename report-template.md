# Report: Predict Bike Sharing Demand with AutoGluon Solution
#### Shan Song

## Initial Training
### What did you realize when you tried to submit your predictions? What changes were needed to the output of the predictor to submit your results?
The training just use raw data and the predictions may not perform very well in kaggle score. But it is still valuable as a baseline.

We need to check if there is negative numbers in predictions and if there is, turn them to 0.

### What was the top ranked model that performed?
In all 3 type train, best model is WeightedEnsemble_L3.

## Exploratory data analysis and feature creation
### What did the exploratory analysis find and how did you add additional features?
Through exploratory analysis we find some features basically conforms to normal distribution like temp, atemp, humidity. Some features show domain factor like holiday, workingday, weather. while datetime and season have relatively even distribution.

We add additional features by spliting datetime to separate field which is month, day, hour. This way will help the training identify which feature matters more and make reasonable fits.

### How much better did your model preform after adding additional features and why do you think that is?
The model performance improves a lot with eval metric from -53.056462 to -30.321846.

I think this happens because the additional features month, day and hour extracted from datetime just make more sense to the model which part of datetime contribute how much.

## Hyper parameter tuning
### How much better did your model preform after trying different hyper parameters?
First I tried hyper parameters as String "light", the model performance improves 0.12 with kaggle score compared to model after adding features. Then I tried custom hyper parameter dictionary, the model performance improves 0.02 compared to "light".

### If you were given more time with this dataset, where do you think you would spend more time?
I will adjust parameter in current dictionary one by one to see the influence direction they cause. Then try the combination in which individual parameter bring positive impact.

### Create a table with the models you ran, the hyperparameters modified, and the kaggle score.
| model        | GBM                                                  | XGB                                                               | CAT                                             | score   |
|--------------|------------------------------------------------------|-------------------------------------------------------------------|-------------------------------------------------|---------|
| initial      | Default                                              | Default                                                           | Default                                         | 1.79417 |
| add_features | Default                                              | Default                                                           | Default                                         | 0.65929 |
| hpo          | objective=regression_l1, num_boost_round=100, num_leaves=50 | objective=reg:pseudohubererror, eval_metric=rmse | depth=8, l2_leaf_reg=10 | 0.51598 |

### Create a line plot showing the top model score for the three (or more) training runs during the project.

![model_train_score](https://cdn.jsdelivr.net/gh/sugarmango66/imgbed1@main/img/model_train_score.png)

### Create a line plot showing the top kaggle score for the three (or more) prediction submissions during the project.

![model_test_score](https://cdn.jsdelivr.net/gh/sugarmango66/imgbed1@main/img/model_test_score.png)

## Summary
AutoGluon can help try many models automaticly and save us a lot effort. After we have some initial trained model as baseline, we can improve the model performance mainly by two ways.

- EDA.From the data insight, we can deal with data like changing type and adding features(note datetime).

- Hyperparameter tune. Ovserve the fit summary, use top performance models and adjust their hyperparameters with patience.

