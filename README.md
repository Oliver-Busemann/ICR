# ICR - Competition
https://www.kaggle.com/competitions/icr-identify-age-related-conditions

## Goal: Identify age related conditions (binary classification)<br>
## Challenge: <br>
1) Small Dataset (617)<br>
2) Hard points to predict<br>
3) Metric: balanced-log-loss (punishes confident wrong predictions rly hard)<br>

## Notebooks: (Insights & Ideas)<br>
####1. EDA:<br>
   -> Imbalanced Dataset<br>
   -> Low correlation of most features and with target (max 0.3)<br>
   -> Positive Class consists of 3 subclasses (alpha)<br>
   -> Additional Data not available for test: High correlation of 1 feature with target (gamma)<br>
   -> Plot distribution of data reduced to 2 features <br>
####2. - 5. CV_Setup, Modeling, Hyperparameter Opt.:<br>
   -> Setup the CV-Pipe: Use a Multilabel-Stratified-Split with target (alpha) and important feature (gamma)<br>
   -> Train differnet models: XGB, SVM, Extra Tree, Random Forest<br>
   -> Optimize Hyperparameters based on CV-Score<br>
   -> XGB worked best<br>
####6. Try ensemble by averaging the predictions:<br>
   -> Did not improve CV-Score<br>
####7. Try Stacking:<br>
   -> Build a nested CV-Pipe to train & evaluate the meta-model (Logistic Regression) without leakage<br>
   -> Add all models, also the ones that did not perform well since Log. Reg. can figure out the impotant features itself (i.e. base model pred.)<br>
####8. Train a model that predicts gamma:<br>
   -> Train an XGBClassifier that predicts 3 values of gamma (the 2 important ones & other)<br>
   -> This is a multiclass classification<br>
   -> Hyperparameter Optimization, including weights<br>
####9. Add the Gamma Predictor:<br>
   -> Add the Gamma-Predictor as another base-model and use its predictions as features for the meta-model<br>
####10. Try custom novelty detection:<br>
   -> Idea: Try to cap predictions where the data can be classified as an anomaly based on a distance metric<br>
   -> Compare each point from the testdata with the training data of the predicted class<br>
   -> Use the k-nearest-neightbors for that<br>
   -> Optimize the distance on when to classify an anomaly, the number of neighbors and the maximum probability to cut of each anomaly<br>
   -> Do it for both classes individually<br>
   -> This gave a huge increase in CV-Score

### Recap:<br>
   -> The distribution of hidden-testset was completly different from the training & public-testdata<br>
   -> Was still fun and a good learning experience<br>
