# Linear Transformations - Lab

## Introduction

In this lab, you'll practice your linear transformation skills!

## Objectives

You will be able to:

* Determine if a linear transformation would be useful for a specific model or set of data
* Identify an appropriate linear transformation technique for a specific model or set of data
* Apply linear transformations to independent and dependent variables in linear regression
* Interpret the coefficients of variables that have been transformed using a linear transformation

## Ames Housing Data

Let's look at the Ames Housing data, where each record represents a home sale:


```python
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
plt.style.use('seaborn')

ames = pd.read_csv('ames.csv', index_col=0)
ames
```


```python
# __SOLUTION__ 
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
plt.style.use('seaborn')

ames = pd.read_csv('ames.csv', index_col=0)
ames
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>MSSubClass</th>
      <th>MSZoning</th>
      <th>LotFrontage</th>
      <th>LotArea</th>
      <th>Street</th>
      <th>Alley</th>
      <th>LotShape</th>
      <th>LandContour</th>
      <th>Utilities</th>
      <th>LotConfig</th>
      <th>...</th>
      <th>PoolArea</th>
      <th>PoolQC</th>
      <th>Fence</th>
      <th>MiscFeature</th>
      <th>MiscVal</th>
      <th>MoSold</th>
      <th>YrSold</th>
      <th>SaleType</th>
      <th>SaleCondition</th>
      <th>SalePrice</th>
    </tr>
    <tr>
      <th>Id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>60</td>
      <td>RL</td>
      <td>65.0</td>
      <td>8450</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>Inside</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>2</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
      <td>208500</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20</td>
      <td>RL</td>
      <td>80.0</td>
      <td>9600</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>FR2</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>5</td>
      <td>2007</td>
      <td>WD</td>
      <td>Normal</td>
      <td>181500</td>
    </tr>
    <tr>
      <th>3</th>
      <td>60</td>
      <td>RL</td>
      <td>68.0</td>
      <td>11250</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>Inside</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>9</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
      <td>223500</td>
    </tr>
    <tr>
      <th>4</th>
      <td>70</td>
      <td>RL</td>
      <td>60.0</td>
      <td>9550</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>Corner</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>2</td>
      <td>2006</td>
      <td>WD</td>
      <td>Abnorml</td>
      <td>140000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>60</td>
      <td>RL</td>
      <td>84.0</td>
      <td>14260</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>IR1</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>FR2</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>12</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
      <td>250000</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1456</th>
      <td>60</td>
      <td>RL</td>
      <td>62.0</td>
      <td>7917</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>Inside</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>8</td>
      <td>2007</td>
      <td>WD</td>
      <td>Normal</td>
      <td>175000</td>
    </tr>
    <tr>
      <th>1457</th>
      <td>20</td>
      <td>RL</td>
      <td>85.0</td>
      <td>13175</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>Inside</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>MnPrv</td>
      <td>NaN</td>
      <td>0</td>
      <td>2</td>
      <td>2010</td>
      <td>WD</td>
      <td>Normal</td>
      <td>210000</td>
    </tr>
    <tr>
      <th>1458</th>
      <td>70</td>
      <td>RL</td>
      <td>66.0</td>
      <td>9042</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>Inside</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>GdPrv</td>
      <td>Shed</td>
      <td>2500</td>
      <td>5</td>
      <td>2010</td>
      <td>WD</td>
      <td>Normal</td>
      <td>266500</td>
    </tr>
    <tr>
      <th>1459</th>
      <td>20</td>
      <td>RL</td>
      <td>68.0</td>
      <td>9717</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>Inside</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>4</td>
      <td>2010</td>
      <td>WD</td>
      <td>Normal</td>
      <td>142125</td>
    </tr>
    <tr>
      <th>1460</th>
      <td>20</td>
      <td>RL</td>
      <td>75.0</td>
      <td>9937</td>
      <td>Pave</td>
      <td>NaN</td>
      <td>Reg</td>
      <td>Lvl</td>
      <td>AllPub</td>
      <td>Inside</td>
      <td>...</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0</td>
      <td>6</td>
      <td>2008</td>
      <td>WD</td>
      <td>Normal</td>
      <td>147500</td>
    </tr>
  </tbody>
</table>
<p>1460 rows × 80 columns</p>
</div>



We'll use this subset of features. These are specifically the _continuous numeric_ variables, which means that we'll hopefully have meaningful mean values.

From the data dictionary (`data_description.txt`):

```
LotArea: Lot size in square feet

MasVnrArea: Masonry veneer area in square feet

TotalBsmtSF: Total square feet of basement area

GrLivArea: Above grade (ground) living area square feet

GarageArea: Size of garage in square feet
```


```python
ames = ames[[
    "LotArea",
    "MasVnrArea",
    "TotalBsmtSF",
    "GrLivArea",
    "GarageArea",
    "SalePrice"
]].copy()
ames
```


```python
# __SOLUTION__
ames = ames[[
    "LotArea",
    "MasVnrArea",
    "TotalBsmtSF",
    "GrLivArea",
    "GarageArea",
    "SalePrice"
]].copy()
ames
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LotArea</th>
      <th>MasVnrArea</th>
      <th>TotalBsmtSF</th>
      <th>GrLivArea</th>
      <th>GarageArea</th>
      <th>SalePrice</th>
    </tr>
    <tr>
      <th>Id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>8450</td>
      <td>196.0</td>
      <td>856</td>
      <td>1710</td>
      <td>548</td>
      <td>208500</td>
    </tr>
    <tr>
      <th>2</th>
      <td>9600</td>
      <td>0.0</td>
      <td>1262</td>
      <td>1262</td>
      <td>460</td>
      <td>181500</td>
    </tr>
    <tr>
      <th>3</th>
      <td>11250</td>
      <td>162.0</td>
      <td>920</td>
      <td>1786</td>
      <td>608</td>
      <td>223500</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9550</td>
      <td>0.0</td>
      <td>756</td>
      <td>1717</td>
      <td>642</td>
      <td>140000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>14260</td>
      <td>350.0</td>
      <td>1145</td>
      <td>2198</td>
      <td>836</td>
      <td>250000</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1456</th>
      <td>7917</td>
      <td>0.0</td>
      <td>953</td>
      <td>1647</td>
      <td>460</td>
      <td>175000</td>
    </tr>
    <tr>
      <th>1457</th>
      <td>13175</td>
      <td>119.0</td>
      <td>1542</td>
      <td>2073</td>
      <td>500</td>
      <td>210000</td>
    </tr>
    <tr>
      <th>1458</th>
      <td>9042</td>
      <td>0.0</td>
      <td>1152</td>
      <td>2340</td>
      <td>252</td>
      <td>266500</td>
    </tr>
    <tr>
      <th>1459</th>
      <td>9717</td>
      <td>0.0</td>
      <td>1078</td>
      <td>1078</td>
      <td>240</td>
      <td>142125</td>
    </tr>
    <tr>
      <th>1460</th>
      <td>9937</td>
      <td>0.0</td>
      <td>1256</td>
      <td>1256</td>
      <td>276</td>
      <td>147500</td>
    </tr>
  </tbody>
</table>
<p>1460 rows × 6 columns</p>
</div>



We'll also drop any records with missing values for any of these features:


```python
ames.dropna(inplace=True)
ames
```


```python
# __SOLUTION__
ames.dropna(inplace=True)
ames
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LotArea</th>
      <th>MasVnrArea</th>
      <th>TotalBsmtSF</th>
      <th>GrLivArea</th>
      <th>GarageArea</th>
      <th>SalePrice</th>
    </tr>
    <tr>
      <th>Id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>8450</td>
      <td>196.0</td>
      <td>856</td>
      <td>1710</td>
      <td>548</td>
      <td>208500</td>
    </tr>
    <tr>
      <th>2</th>
      <td>9600</td>
      <td>0.0</td>
      <td>1262</td>
      <td>1262</td>
      <td>460</td>
      <td>181500</td>
    </tr>
    <tr>
      <th>3</th>
      <td>11250</td>
      <td>162.0</td>
      <td>920</td>
      <td>1786</td>
      <td>608</td>
      <td>223500</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9550</td>
      <td>0.0</td>
      <td>756</td>
      <td>1717</td>
      <td>642</td>
      <td>140000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>14260</td>
      <td>350.0</td>
      <td>1145</td>
      <td>2198</td>
      <td>836</td>
      <td>250000</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1456</th>
      <td>7917</td>
      <td>0.0</td>
      <td>953</td>
      <td>1647</td>
      <td>460</td>
      <td>175000</td>
    </tr>
    <tr>
      <th>1457</th>
      <td>13175</td>
      <td>119.0</td>
      <td>1542</td>
      <td>2073</td>
      <td>500</td>
      <td>210000</td>
    </tr>
    <tr>
      <th>1458</th>
      <td>9042</td>
      <td>0.0</td>
      <td>1152</td>
      <td>2340</td>
      <td>252</td>
      <td>266500</td>
    </tr>
    <tr>
      <th>1459</th>
      <td>9717</td>
      <td>0.0</td>
      <td>1078</td>
      <td>1078</td>
      <td>240</td>
      <td>142125</td>
    </tr>
    <tr>
      <th>1460</th>
      <td>9937</td>
      <td>0.0</td>
      <td>1256</td>
      <td>1256</td>
      <td>276</td>
      <td>147500</td>
    </tr>
  </tbody>
</table>
<p>1452 rows × 6 columns</p>
</div>



And plot the distributions of the un-transformed variables:


```python
ames.hist(figsize=(15,10), bins="auto");
```


```python
# __SOLUTION__
ames.hist(figsize=(15,10), bins="auto");
```


    
![png](index_files/index_14_0.png)
    


## Step 1: Build an Initial Linear Regression Model

`SalePrice` should be the target, and all other columns in `ames` should be predictors.


```python
# Your code here - build a linear regression model with un-transformed features
```


```python
# __SOLUTION__
y = ames["SalePrice"]
X = ames.drop("SalePrice", axis=1)
X
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LotArea</th>
      <th>MasVnrArea</th>
      <th>TotalBsmtSF</th>
      <th>GrLivArea</th>
      <th>GarageArea</th>
    </tr>
    <tr>
      <th>Id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>8450</td>
      <td>196.0</td>
      <td>856</td>
      <td>1710</td>
      <td>548</td>
    </tr>
    <tr>
      <th>2</th>
      <td>9600</td>
      <td>0.0</td>
      <td>1262</td>
      <td>1262</td>
      <td>460</td>
    </tr>
    <tr>
      <th>3</th>
      <td>11250</td>
      <td>162.0</td>
      <td>920</td>
      <td>1786</td>
      <td>608</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9550</td>
      <td>0.0</td>
      <td>756</td>
      <td>1717</td>
      <td>642</td>
    </tr>
    <tr>
      <th>5</th>
      <td>14260</td>
      <td>350.0</td>
      <td>1145</td>
      <td>2198</td>
      <td>836</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1456</th>
      <td>7917</td>
      <td>0.0</td>
      <td>953</td>
      <td>1647</td>
      <td>460</td>
    </tr>
    <tr>
      <th>1457</th>
      <td>13175</td>
      <td>119.0</td>
      <td>1542</td>
      <td>2073</td>
      <td>500</td>
    </tr>
    <tr>
      <th>1458</th>
      <td>9042</td>
      <td>0.0</td>
      <td>1152</td>
      <td>2340</td>
      <td>252</td>
    </tr>
    <tr>
      <th>1459</th>
      <td>9717</td>
      <td>0.0</td>
      <td>1078</td>
      <td>1078</td>
      <td>240</td>
    </tr>
    <tr>
      <th>1460</th>
      <td>9937</td>
      <td>0.0</td>
      <td>1256</td>
      <td>1256</td>
      <td>276</td>
    </tr>
  </tbody>
</table>
<p>1452 rows × 5 columns</p>
</div>




```python
# __SOLUTION__
import statsmodels.api as sm

initial_model = sm.OLS(y, sm.add_constant(X))
initial_results = initial_model.fit()

print(initial_results.summary())
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:              SalePrice   R-squared:                       0.676
    Model:                            OLS   Adj. R-squared:                  0.675
    Method:                 Least Squares   F-statistic:                     603.0
    Date:                Wed, 18 May 2022   Prob (F-statistic):               0.00
    Time:                        19:14:48   Log-Likelihood:                -17622.
    No. Observations:                1452   AIC:                         3.526e+04
    Df Residuals:                    1446   BIC:                         3.529e+04
    Df Model:                           5                                         
    Covariance Type:            nonrobust                                         
    ===============================================================================
                      coef    std err          t      P>|t|      [0.025      0.975]
    -------------------------------------------------------------------------------
    const       -1.525e+04   4145.934     -3.677      0.000   -2.34e+04   -7113.396
    LotArea         0.2568      0.125      2.056      0.040       0.012       0.502
    MasVnrArea     55.0481      7.427      7.412      0.000      40.480      69.616
    TotalBsmtSF    44.1640      3.324     13.286      0.000      37.643      50.685
    GrLivArea      63.8443      2.772     23.030      0.000      58.406      69.282
    GarageArea     93.4629      6.795     13.755      0.000      80.134     106.792
    ==============================================================================
    Omnibus:                      817.744   Durbin-Watson:                   1.991
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):            77147.499
    Skew:                          -1.709   Prob(JB):                         0.00
    Kurtosis:                      38.546   Cond. No.                     5.09e+04
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    [2] The condition number is large, 5.09e+04. This might indicate that there are
    strong multicollinearity or other numerical problems.


## Step 2: Evaluate Initial Model and Interpret Coefficients

Describe the model performance overall and interpret the meaning of each predictor coefficient. Make sure to refer to the explanations of what each feature means from the data dictionary!


```python
# Your written answer here
```

<details>
    <summary style="cursor: pointer"><b>Answer (click to reveal)</b></summary>

The model overall is statistically significant and explains about 68% of the variance in sale price.

The coefficients are all statistically significant.

* `LotArea`: for each additional square foot of lot area, the price increases by about \\$0.26
* `MasVnrArea`: for each additional square foot of masonry veneer, the price increases by about \\$55
* `TotalBsmtSF`: for each additional square foot of basement area, the price increases by about \\$44
* `GrLivArea`: for each additional square foot of above-grade living area, the price increases by about \\$64
* `GarageArea`: for each additional square foot of garage area, the price increases by about \\$93

</details>

## Step 3: Express Model Coefficients in Metric Units

Your stakeholder gets back to you and says this is great, but they are interested in metric units.

Specifically they would like to measure area in square meters rather than square feet.

Report the same coefficients, except using square meters. You can do this by building a new model, or by transforming just the coefficients.

The conversion you can use is **1 square foot = 0.092903 square meters**.


```python
# Your code here - building a new model or transforming coefficients
# from initial model so that they are in square meters

```


```python
# __SOLUTION__
# New model approach
X_metric = X.copy()

# All of the features are measured in square feet, so apply the same transformation
for col in X_metric.columns:
    X_metric[col] = X_metric[col] * 0.092903

# One of the features has "SF" in it which is no longer accurate
X_metric.rename(columns={"TotalBsmtSF": "TotalBsmtArea"}, inplace=True)

X_metric
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LotArea</th>
      <th>MasVnrArea</th>
      <th>TotalBsmtArea</th>
      <th>GrLivArea</th>
      <th>GarageArea</th>
    </tr>
    <tr>
      <th>Id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>785.030350</td>
      <td>18.208988</td>
      <td>79.524968</td>
      <td>158.864130</td>
      <td>50.910844</td>
    </tr>
    <tr>
      <th>2</th>
      <td>891.868800</td>
      <td>0.000000</td>
      <td>117.243586</td>
      <td>117.243586</td>
      <td>42.735380</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1045.158750</td>
      <td>15.050286</td>
      <td>85.470760</td>
      <td>165.924758</td>
      <td>56.485024</td>
    </tr>
    <tr>
      <th>4</th>
      <td>887.223650</td>
      <td>0.000000</td>
      <td>70.234668</td>
      <td>159.514451</td>
      <td>59.643726</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1324.796780</td>
      <td>32.516050</td>
      <td>106.373935</td>
      <td>204.200794</td>
      <td>77.666908</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1456</th>
      <td>735.513051</td>
      <td>0.000000</td>
      <td>88.536559</td>
      <td>153.011241</td>
      <td>42.735380</td>
    </tr>
    <tr>
      <th>1457</th>
      <td>1223.997025</td>
      <td>11.055457</td>
      <td>143.256426</td>
      <td>192.587919</td>
      <td>46.451500</td>
    </tr>
    <tr>
      <th>1458</th>
      <td>840.028926</td>
      <td>0.000000</td>
      <td>107.024256</td>
      <td>217.393020</td>
      <td>23.411556</td>
    </tr>
    <tr>
      <th>1459</th>
      <td>902.738451</td>
      <td>0.000000</td>
      <td>100.149434</td>
      <td>100.149434</td>
      <td>22.296720</td>
    </tr>
    <tr>
      <th>1460</th>
      <td>923.177111</td>
      <td>0.000000</td>
      <td>116.686168</td>
      <td>116.686168</td>
      <td>25.641228</td>
    </tr>
  </tbody>
</table>
<p>1452 rows × 5 columns</p>
</div>




```python
# __SOLUTION__
fig, axes = plt.subplots(nrows=5, figsize=(15, 25))

for index, col in enumerate(X):
    ax = axes[index]
    ax.hist(X[col], bins="auto", label="Imperial")
    if col == "TotalBsmtSF":
        col = "TotalBsmtArea"
    ax.hist(X_metric[col], bins="auto", label="Metric", color="orange")
    ax.set_xlabel(col)
    ax.legend()
```


    
![png](index_files/index_25_0.png)
    



```python
# __SOLUTION__
metric_model = sm.OLS(y, sm.add_constant(X_metric))
metric_results = metric_model.fit()

metric_results.params
```




    const           -15246.083611
    LotArea              2.763844
    MasVnrArea         592.532631
    TotalBsmtArea      475.377849
    GrLivArea          687.214850
    GarageArea        1006.027003
    dtype: float64




```python
# __SOLUTION__
# Transforming initial coefficients approach
# (using [1:] to skip over the intercept)
initial_results.params[1:] / 0.092903
```




    LotArea           2.763844
    MasVnrArea      592.532631
    TotalBsmtSF     475.377849
    GrLivArea       687.214850
    GarageArea     1006.027003
    dtype: float64




```python
# Your written answer here

```

<details>
    <summary style="cursor: pointer"><b>Answer (click to reveal)</b></summary>

* `LotArea`: for each additional square meter of lot area, the price increases by about \\$2.76
* `MasVnrArea`: for each additional square meter of masonry veneer, the price increases by about \\$593
* `TotalBsmtArea`: for each additional square meter of basement area, the price increases by about \\$475
* `GrLivArea`: for each additional square meter of above-grade living area, the price increases by about \\$687
* `GarageArea`: for each additional square meter of garage area, the price increases by about \\$1,006

</details>

## Step 4: Center Data to Provide an Interpretable Intercept

Your stakeholder is happy with the metric results, but now they want to know what's happening with the intercept value. Negative \\$17k for a home with zeros across the board...what does that mean?

Center the data so that the mean is 0, fit a new model, and report on the new intercept.

(It doesn't matter whether you use data that was scaled to metric units or not. The intercept should be the same either way.)


```python
# Your code here - center data

```


```python
# __SOLUTION__
X_centered = X_metric.copy()

for col in X_centered.columns:
    X_centered[col] = X_centered[col] - X_centered[col].mean()

X_centered
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LotArea</th>
      <th>MasVnrArea</th>
      <th>TotalBsmtArea</th>
      <th>GrLivArea</th>
      <th>GarageArea</th>
    </tr>
    <tr>
      <th>Id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>-191.127128</td>
      <td>8.576316</td>
      <td>-18.566396</td>
      <td>18.200478</td>
      <td>7.016480</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-84.288678</td>
      <td>-9.632672</td>
      <td>19.152222</td>
      <td>-23.420066</td>
      <td>-1.158984</td>
    </tr>
    <tr>
      <th>3</th>
      <td>69.001272</td>
      <td>5.417614</td>
      <td>-12.620604</td>
      <td>25.261106</td>
      <td>12.590660</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-88.933828</td>
      <td>-9.632672</td>
      <td>-27.856696</td>
      <td>18.850799</td>
      <td>15.749362</td>
    </tr>
    <tr>
      <th>5</th>
      <td>348.639302</td>
      <td>22.883378</td>
      <td>8.282571</td>
      <td>63.537142</td>
      <td>33.772544</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1456</th>
      <td>-240.644427</td>
      <td>-9.632672</td>
      <td>-9.554805</td>
      <td>12.347589</td>
      <td>-1.158984</td>
    </tr>
    <tr>
      <th>1457</th>
      <td>247.839547</td>
      <td>1.422785</td>
      <td>45.165062</td>
      <td>51.924267</td>
      <td>2.557136</td>
    </tr>
    <tr>
      <th>1458</th>
      <td>-136.128552</td>
      <td>-9.632672</td>
      <td>8.932892</td>
      <td>76.729368</td>
      <td>-20.482808</td>
    </tr>
    <tr>
      <th>1459</th>
      <td>-73.419027</td>
      <td>-9.632672</td>
      <td>2.058070</td>
      <td>-40.514218</td>
      <td>-21.597644</td>
    </tr>
    <tr>
      <th>1460</th>
      <td>-52.980367</td>
      <td>-9.632672</td>
      <td>18.594804</td>
      <td>-23.977484</td>
      <td>-18.253136</td>
    </tr>
  </tbody>
</table>
<p>1452 rows × 5 columns</p>
</div>




```python
# __SOLUTION__
fig, axes = plt.subplots(nrows=5, figsize=(15, 25))

for index, col in enumerate(X_metric):
    ax = axes[index]
    ax.hist(X_metric[col], bins="auto", label="Un-Centered")
    ax.hist(X_centered[col], bins="auto", label="Centered", color="orange")
    ax.set_xlabel(col)
    ax.legend()
```


    
![png](index_files/index_33_0.png)
    



```python
# Your code here - build a new model

```


```python
# __SOLUTION__
centered_model = sm.OLS(y, sm.add_constant(X_centered))
centered_results = centered_model.fit()

centered_results.params["const"]
```




    180615.06336088135




```python
# Your written answer here - interpret the new intercept

```

<details>
    <summary style="cursor: pointer"><b>Answer (click to reveal)</b></summary>

The new intercept is about \\$181k. This means that a home with average lot area, average masonry veneer area, average total basement area, average above-grade living area, and average garage area would sell for about \\$181k.

</details>

## Step 5: Identify the "Most Important" Feature

Finally, either build a new model with transformed coefficients or transform the coefficients from the Step 4 model so that the most important feature can be identified.

Even though all of the features are measured in area, they are different kinds of area (e.g. lot area vs. masonry veneer area) that are not directly comparable as-is. So apply **standardization** (dividing predictors by their standard deviations) and identify the feature with the highest standardized coefficient as the "most important".


```python
# Your code here - building a new model or transforming coefficients
# from centered model so that they are in standard deviations

```


```python
# __SOLUTION__
# New model approach
X_standardized = X_centered.copy()

# We have already subtracted the mean, just need to divide by std
for col in X_standardized.columns:
    X_standardized[col] = X_standardized[col] / X_standardized[col].std()

X_standardized
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>LotArea</th>
      <th>MasVnrArea</th>
      <th>TotalBsmtArea</th>
      <th>GrLivArea</th>
      <th>GarageArea</th>
    </tr>
    <tr>
      <th>Id</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>-0.205943</td>
      <td>0.509840</td>
      <td>-0.456148</td>
      <td>0.372713</td>
      <td>0.352744</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-0.090822</td>
      <td>-0.572637</td>
      <td>0.470541</td>
      <td>-0.479601</td>
      <td>-0.058266</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.074350</td>
      <td>0.322063</td>
      <td>-0.310069</td>
      <td>0.517302</td>
      <td>0.632979</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-0.095828</td>
      <td>-0.572637</td>
      <td>-0.684396</td>
      <td>0.386031</td>
      <td>0.791778</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.375664</td>
      <td>1.360357</td>
      <td>0.203490</td>
      <td>1.301127</td>
      <td>1.697870</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1456</th>
      <td>-0.259298</td>
      <td>-0.572637</td>
      <td>-0.234747</td>
      <td>0.252857</td>
      <td>-0.058266</td>
    </tr>
    <tr>
      <th>1457</th>
      <td>0.267051</td>
      <td>0.084581</td>
      <td>1.109636</td>
      <td>1.063316</td>
      <td>0.128557</td>
    </tr>
    <tr>
      <th>1458</th>
      <td>-0.146681</td>
      <td>-0.572637</td>
      <td>0.219467</td>
      <td>1.571280</td>
      <td>-1.029746</td>
    </tr>
    <tr>
      <th>1459</th>
      <td>-0.079110</td>
      <td>-0.572637</td>
      <td>0.050564</td>
      <td>-0.829659</td>
      <td>-1.085793</td>
    </tr>
    <tr>
      <th>1460</th>
      <td>-0.057087</td>
      <td>-0.572637</td>
      <td>0.456846</td>
      <td>-0.491016</td>
      <td>-0.917652</td>
    </tr>
  </tbody>
</table>
<p>1452 rows × 5 columns</p>
</div>




```python
# __SOLUTION__
fig, axes = plt.subplots(nrows=5, figsize=(15, 25))

for index, col in enumerate(X_standardized):
    ax = axes[index]
    ax.hist(X_centered[col], bins="auto", label="Centered")
    ax.hist(X_standardized[col], bins="auto", label="Standardized", color="orange")
    ax.set_xlabel(col)
    ax.legend()
    
# Manually adjust LotArea axis because otherwise standardized data is invisible
# (LotArea has a large standard deviation)
axes[0].set_xlim(-500, 500);
```


    
![png](index_files/index_41_0.png)
    



```python
# __SOLUTION__
standardized_model = sm.OLS(y, sm.add_constant(X_standardized))
standardized_results = standardized_model.fit()

standardized_results.params
```




    const            180615.063361
    LotArea            2565.014370
    MasVnrArea         9967.343227
    TotalBsmtArea     19349.103860
    GrLivArea         33558.347891
    GarageArea        20011.010509
    dtype: float64




```python
# __SOLUTION__
# Transforming initial coefficients approach
# (using [1:] to skip over the intercept)
for param in centered_results.params.index[1:]:
    transformed_val = centered_results.params[param] * X_centered[param].std()
    print(f"{param:18}{round(transformed_val, 5):>12}")
```

    LotArea             2565.01437
    MasVnrArea          9967.34323
    TotalBsmtArea      19349.10386
    GrLivArea          33558.34789
    GarageArea         20011.01051



```python
# Your written answer here - identify the "most important" feature

```

<details>
    <summary style="cursor: pointer"><b>Answer (click to reveal)</b></summary>

The feature with the highest standardized coefficient is `GrLivArea`. This means that above-grade living area is most important.

</details>

## Summary
Great! You've now got some hands-on practice transforming data and interpreting the results!
