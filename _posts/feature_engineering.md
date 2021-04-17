---
title: "Feature Engineering with Pandas"
tags: [Feature Engineering, Pandas, Python, scikit-learn Machine Learning]
style: border
color: primary
description: Feature engineering is an important part of building machine learning models, this article introduces how we can use Pandas and Scikit-learn to manipulate data and extract better feature before we run any machine learning models.  
---

# Feature Engineering 

## Categorical variables 

One-Hot-Encoding (Dummy Variables) 

```Python 
# pandas to display unique values in a columns. 
data.gender.value_counts()

# transfer all categorical variables into dummy varaibles
data_dummies = pd.get_dummies(data) 

# transfer selected categorical variables into dummy variables
data_dummies = pd.get_dummies(data, columns=['gender', 'marriage_status'])
# scikit-learn has a similar function
from sklearn.preprocessing import OneHotEncoder 
encoder = OneHotEncoder(sparse=False)
encoder.fit(data)
data_dummies = encoder.transform(data)
```

## Numerical variables 

Binning, Discretization

```Python
bins = np.linspace(-3, 3, 11)
which_bin = np.digitize(data.colunm1, bins=bins) 
from sklearn.preprocessing import OneHotEncoder 
encoder = OneHotEncoder(sparse=False)
encoder.fit(which_bin)
data_binned = encoder.transform(which_bin)
data_combined = np.hstack([data, data_binned]) # data here is only one column, need to remove the categorical variable if more than one. 
```

Interactions and Polynomials 

```Python 
# interaction
data_product = np.hstack([data_combined, data_binned*data]) 
# polynomialfeatures 
from sklearn import PolynomialFeatures 
poly = PolynomialFeatures(degree=100, include_bias=False)
poly.fit(x)
X_poly = poly.transform(x)
# another way is to use svr, no need to transform data first. 
```

Univariate Nonlinear Transformations

Log transformation 

```Python
X_train_log = np.log(X_train + 1)
X_test_log = np.log(X_test + 1) 
```

## Automatic Feature Selection 

Univariates Statistics 

Model-based Feature Selection 

Iterative Feature Selection 


# AutoML 

auto-sklearn 

# Model Evaluation and Improvement 

### 1 Cross Validation 

Cross validation is a statistical method of evaluating generalization performance that is more stable and thorough than using a split into a training and a test set. 

**k-fold CV** 

```Python
from sklearn.model_selection import cross_val_score 
model = LogisticRegression()
scores = cross_val_score(model, x, y, cv=5)
# this code does not return any specific model, but a way to evaluate a model. 
```

k-fold cv advantages: 1. more robust way to evaluate model. 2. more data to build the model than simple split train and test. 

k-fold cv disadvantages: more computational cost. 

**stratified k-fold cross-validation** 

This method makes sure the proportions between between classes are the same in each fold as they are in the whole dataset.  

```Python
from sklearn.model_selection import KFold  
kfold = KFold(n_splits=5) 
scores = croos_val_score(model, x, y, cv=kfold)
```

**leave one out cv** 

```Python
from sklearn.model_selection import LeaveOneOut
loo = LeaveOneOut()
scores = cross_val_score(model, x, y, cv=loo)
```

**Shuffle split cv**

**Cross validation with groups (GroupCV)**


# Evaluation Metrics and Scoring 

**Binary Classification**

Confusion matrix 

```Python
from sklearn.metrics import confusion_matrix 
confusion_matrix(y_test, pred)
# report
from sklearn.pygame.font.metrics import classification_report
classification_report(y_test, pred, target_names=['class_0, class_1'])
```

accuracy: overall performance.  

precision: measures how many of the samples predicted as positive are actually positive. 

recall: measure how many of the positive examples are correctly predicted. 

F-score: The F1 score is the harmonic mean of the precision and recall. The highest possible value of an F-score is 1, indicating perfect precision and recall, and the lowest possible value is 0. (Wiki) 

Receiver operating characteristics (ROC) and AUC

The ROC curve consider all possible thresholds for a given classifier. It reports the true positive rate (recall) and false positive rate. The goal is to have high recall while keeping fpr low. For imbalanced dataset, AUC is much better than accuracy. 

```Python 
from sklearn.metrics import roc_curve 
fpr, tpr, threshold = roc_curve(y_test, pred) 
plt.plot(fpr, tpr, label='roc curve')
plt.xlabel('FPR')
plt.ylabel('TPR')
```

**Multiclass Classification** 

For multiclass classification task, all the metrics used on binary classification can be used. However, the most common used metric is F-score. 








