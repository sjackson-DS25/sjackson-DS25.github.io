---
title: "Seminar 2 - Coding Exercise"
layout: page
permalink: /seminar2/
---

```python
import pandas as pd
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
import scipy.stats as st
from sklearn import ensemble, tree, linear_model
import missingno as msno

```


```python
auto = pd.read_csv(r"C:\Users\alexa\Sonya\Machine learning\Unit02 auto-mpg (1).csv")

```


```python
auto.describe()
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
      <th>mpg</th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>model year</th>
      <th>origin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>398.000000</td>
      <td>398.000000</td>
      <td>398.000000</td>
      <td>398.000000</td>
      <td>398.000000</td>
      <td>398.000000</td>
      <td>398.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>23.514573</td>
      <td>5.454774</td>
      <td>193.425879</td>
      <td>2970.424623</td>
      <td>15.568090</td>
      <td>76.010050</td>
      <td>1.572864</td>
    </tr>
    <tr>
      <th>std</th>
      <td>7.815984</td>
      <td>1.701004</td>
      <td>104.269838</td>
      <td>846.841774</td>
      <td>2.757689</td>
      <td>3.697627</td>
      <td>0.802055</td>
    </tr>
    <tr>
      <th>min</th>
      <td>9.000000</td>
      <td>3.000000</td>
      <td>68.000000</td>
      <td>1613.000000</td>
      <td>8.000000</td>
      <td>70.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>17.500000</td>
      <td>4.000000</td>
      <td>104.250000</td>
      <td>2223.750000</td>
      <td>13.825000</td>
      <td>73.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>23.000000</td>
      <td>4.000000</td>
      <td>148.500000</td>
      <td>2803.500000</td>
      <td>15.500000</td>
      <td>76.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>29.000000</td>
      <td>8.000000</td>
      <td>262.000000</td>
      <td>3608.000000</td>
      <td>17.175000</td>
      <td>79.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>46.600000</td>
      <td>8.000000</td>
      <td>455.000000</td>
      <td>5140.000000</td>
      <td>24.800000</td>
      <td>82.000000</td>
      <td>3.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#identify missing values
```


```python
auto.info()
```

    <class 'pandas.DataFrame'>
    RangeIndex: 398 entries, 0 to 397
    Data columns (total 9 columns):
     #   Column        Non-Null Count  Dtype  
    ---  ------        --------------  -----  
     0   mpg           398 non-null    float64
     1   cylinders     398 non-null    int64  
     2   displacement  398 non-null    float64
     3   horsepower    398 non-null    str    
     4   weight        398 non-null    int64  
     5   acceleration  398 non-null    float64
     6   model year    398 non-null    int64  
     7   origin        398 non-null    int64  
     8   car name      398 non-null    str    
    dtypes: float64(3), int64(4), str(2)
    memory usage: 28.1 KB
    


```python
# there are no missing values; visualise with msno
msno.matrix(auto)

```




    <Axes: >




    
![png](seminar%202%20auto_files/seminar%202%20auto_5_1.png)
    



```python
msno.bar(auto, color = 'g', figsize = (10,8))
```




    <Axes: >




    
![png](seminar%202%20auto_files/seminar%202%20auto_6_1.png)
    



```python
# invesitgate skewness and kurtosis
auto.select_dtypes(include=["number"]).skew()
```




    mpg             0.457066
    cylinders       0.526922
    displacement    0.719645
    weight          0.531063
    acceleration    0.278777
    model year      0.011535
    origin          0.923776
    dtype: float64




```python
#why is horespower a string? - look at data
```


```python
auto.head()
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
      <th>mpg</th>
      <th>cylinders</th>
      <th>displacement</th>
      <th>horsepower</th>
      <th>weight</th>
      <th>acceleration</th>
      <th>model year</th>
      <th>origin</th>
      <th>car name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18.0</td>
      <td>8</td>
      <td>307.0</td>
      <td>130</td>
      <td>3504</td>
      <td>12.0</td>
      <td>70</td>
      <td>1</td>
      <td>chevrolet chevelle malibu</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15.0</td>
      <td>8</td>
      <td>350.0</td>
      <td>165</td>
      <td>3693</td>
      <td>11.5</td>
      <td>70</td>
      <td>1</td>
      <td>buick skylark 320</td>
    </tr>
    <tr>
      <th>2</th>
      <td>18.0</td>
      <td>8</td>
      <td>318.0</td>
      <td>150</td>
      <td>3436</td>
      <td>11.0</td>
      <td>70</td>
      <td>1</td>
      <td>plymouth satellite</td>
    </tr>
    <tr>
      <th>3</th>
      <td>16.0</td>
      <td>8</td>
      <td>304.0</td>
      <td>150</td>
      <td>3433</td>
      <td>12.0</td>
      <td>70</td>
      <td>1</td>
      <td>amc rebel sst</td>
    </tr>
    <tr>
      <th>4</th>
      <td>17.0</td>
      <td>8</td>
      <td>302.0</td>
      <td>140</td>
      <td>3449</td>
      <td>10.5</td>
      <td>70</td>
      <td>1</td>
      <td>ford torino</td>
    </tr>
  </tbody>
</table>
</div>




```python
#convert horsepower to numberical
auto['horsepower'] = pd.to_numeric(auto['horsepower'], errors='coerce')
```


```python
auto.select_dtypes(include=["number"]).skew()
```




    mpg             0.457066
    cylinders       0.526922
    displacement    0.719645
    horsepower      1.087326
    weight          0.531063
    acceleration    0.278777
    model year      0.011535
    origin          0.923776
    dtype: float64




```python
# model year is roughly symmetrical, others are skewed to the right, i.e. some values are larger

```


```python
#Model year is approximately symmetric. The remaining variables are positively skewed to varying degrees, meaning most observations occur at lower values while a smaller number of large values create a right tail. 
#Horsepower is the most strongly right-skewed variable.
```


```python
auto.select_dtypes(include=["number"]).kurt()
```




    mpg            -0.510781
    cylinders      -1.376662
    displacement   -0.746597
    horsepower      0.696947
    weight         -0.785529
    acceleration    0.419497
    model year     -1.181232
    origin         -0.817597
    dtype: float64




```python
#Most variables have negative kurtosis, indicating flatter distributions with lighter tails than a normal distribution.
#Horsepower and acceleration have positive kurtosis, suggesting somewhat heavier tails and a greater presence of extreme values, particularly for horsepower.
```


```python
#correlation heat map

#first select the numeric values

numeric_features = auto.select_dtypes(include=[np.number])

correlation = correlation = numeric_features.corr()

```


```python
f , ax = plt.subplots(figsize = (14,12))

plt.title('Correlation of Numeric Features',y=1,size=16)

sns.heatmap(correlation,square = True,  vmax=0.8)
```




    <Axes: title={'center': 'Correlation of Numeric Features'}>




    
![png](seminar%202%20auto_files/seminar%202%20auto_17_1.png)
    



```python
#cylinders, displacement, horsepower and weight are stringly correlated
```


```python
#catterplot for different parameters

sns.regplot(x='horsepower',y = 'mpg',data = auto,scatter= True, fit_reg=True)


```




    <Axes: xlabel='horsepower', ylabel='mpg'>




    
![png](seminar%202%20auto_files/seminar%202%20auto_19_1.png)
    



```python
# scatterplot grid

fig, axes = plt.subplots(nrows=2, ncols=2, figsize=(14,10))

sns.regplot(x='horsepower',y = 'mpg',data = auto,scatter= True, fit_reg=True, ax=axes[0,0] )
sns.regplot(x='horsepower',y = 'weight',data = auto,scatter= True, fit_reg=True, ax=axes[0,1])
sns.regplot(x='horsepower',y = 'acceleration',data = auto,scatter= True, ax=axes[1,0])
sns.regplot(x='acceleration',y = 'weight',data = auto,scatter= True, ax=axes[1,1])
```




    <Axes: xlabel='acceleration', ylabel='weight'>




    
![png](seminar%202%20auto_files/seminar%202%20auto_20_1.png)
    



```python
#replace categorical value with numberical; i.e. country names with codes
#already numerical, but example coding
```


```python
categorical_features = ['origin']
for c in categorical_features:

    auto[c] = auto[c].astype('category')

    if auto[c].isnull().any():
        auto[c] = auto[c].cat.add_categories(['MISSING'])
        auto[c] = auto[c].fillna('MISSING')

    # convert categories to numbers
    auto[c] = auto[c].cat.codes
```


```python
# or autmatically select columsn which are type = object 
```


```python
categorical_features = auto.select_dtypes(include='object').columns

for c in categorical_features:

    auto[c] = auto[c].astype('category')

    if auto[c].isnull().any():
        auto[c] = auto[c].cat.add_categories(['MISSING'])
        auto[c] = auto[c].fillna('MISSING')

    auto[c] = auto[c].cat.codes
```
