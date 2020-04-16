
# Feature Scaling and Normalization - Lab

## Introduction
In this lab, you'll practice your feature scaling and normalization skills!

## Objectives
You will be able to:
* Identify if it is necessary to perform log transformations on a set of features
* Perform log transformations on different features of a dataset
* Determine if it is necessary to perform normalization/standardization for a specific model or set of data
* Compare the different standardization and normalization techniques
* Use standardization/normalization on features of a dataset

## Back to the Ames Housing data

Let's import our Ames Housing data.


```python
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
plt.style.use('seaborn')

ames = pd.read_csv('ames.csv')
```


```python
# __SOLUTION__ 
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
plt.style.use('seaborn')

ames = pd.read_csv('ames.csv')
```

## Look at the histograms for the continuous variables

Since there are so many features it is helpful to filter the columns by datatype and number of unique values. A heuristic you might use to select continous variables might be a combination of features that are not object datatypes and have at least a certain amount of unique values.


```python
# Your code here
```


```python
# __SOLUTION__ 
cat_data = ames.loc[:, ((ames.dtypes != 'object') & (ames.nunique() > 20))]

fig, axes = plt.subplots(nrows=(cat_data.shape[1] // 3), ncols=3, figsize=(16,40))

categoricals = [column for column in cat_data.columns if column != 'Id']

for col, ax in zip(categoricals, axes.flatten()):
    ax.hist(ames[col].dropna(), bins='auto')
    ax.set_title(col)
    
fig.tight_layout()
```


![png](index_files/index_9_0.png)


We can see from our histogram of the contiuous features that there are many examples where there are a ton of zeros. For example, WoodDeckSF (square footage of a wood deck) gives us a positive number indicating the size of the deck and zero if no deck exists. It might have made sense to categorize this variable to "deck exists or not (binary variable 1/0). Now you have a zero-inflated variable which is cumbersome to work with.

Lets drop these zero-inflated variables for now and select the features which don't have this characteristic.


```python
# Select non zero-inflated continuous features as ames_cont
ames_cont = None
```


```python
# __SOLUTION__ 
continuous = ['LotFrontage', 'LotArea', 'YearBuilt', '1stFlrSF', 'GrLivArea', 'SalePrice']
ames_cont = ames[continuous]
```


```python
# __SOLUTION__ 
ames_cont.hist(figsize  = [8, 8], bins='auto');
```


![png](index_files/index_13_0.png)


## Perform log transformations for the variables where it makes sense


```python
# Your code here
```


```python
# __SOLUTION__ 
import numpy as np

log_names = [f'{column}_log' for column in ames_cont.columns]

ames_log = np.log(ames_cont)
ames_log.columns = log_names
ames_log.hist(figsize=(10, 10), bins='auto')
fig.tight_layout();
```


![png](index_files/index_16_0.png)


## Standardize the continuous variables

Store your final features in a DataFrame `features_final`: 


```python
# Your code here
```


```python
# __SOLUTION__ 
# Commentary:

# We decided not to include the zero inflated features anymore
# We decided to perform log transformations on the data, 
# except for "YearBuilt" where the logtransforms did not improve the skewness.
```


```python
# __SOLUTION__ 

def normalize(feature):
    return (feature - feature.mean()) / feature.std()

features_final = ames_log.apply(normalize)

features_final.hist(figsize  = [8, 8]);
```


![png](index_files/index_21_0.png)


## Summary
Great! You've now got some hands-on practice transforming data using log transforms, feature scaling, and normalization!
