---
layout: single
title:  "Can we prevent heart failure by data analysis?"
---

# [ML] Classification with cilinical data 
----------

## About data:
- This analysis includes heart failure clinical records dataset from Kaggle(https://www.kaggle.com/andrewmvd/heart-failure-clinical-data).
- The given CSV file contains such columns below.
    1. age: age of patients
    2. anaemia: decrease of red blood cells or hemoglobin (boolean: 0-normal, 1-anaemia)
    3. creatinine_phosphokinase: level of the CPK enzyme in the blood (mcg/L)
    4. diabetes: if the patient has diabetes (boolean: 0-normal, 1-diabetes)
    5. ejection_fraction: percentage of blood leaving the heart at each contraction (percentage: %)
    6. high_blood_pressure: If the patient has hypertension (boolean: 0-normal, 1-high blood pressure)
    7. platelets: platelets in the blood (kiloplatelets/mL)
    8. serum_creatinine: level of serum creatinine in the blood (mg/dL)
    9. serum_sodium: level of serum sodium in the blood (mEq/L)
    10. sex: sex of the patient (binary: 0-woman, 1-man)
    11. smoking: if the patient smokes or not (boolean: 0-no, 1-yes)
    12. time: follow-up period (days)
    13. DEATH_EVENT: If the patient deceased during the follow-up period (boolean: 0-alive, 1-dead)    

## Goals of the analysis
- Understanding the form of clinical data
- Applying pandas libary
- Acquring insights through data visualization
- Training models based on Scikit-learn
- Applying classification models and evaluating their performance

----------

## Step 1. Preparing dataset


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

### 1-1. Kaggle API setting


```python
import os
```


```python
os.environ['KAGGLE_USERNAME']="jisuleeoslo"
os.environ['KAGGLE_KEy']=""
```

### 1-2. Downloading data and unzipping data


```python
!kaggle datasets download -d andrewmvd/heart-failure-clinical-data
```

    heart-failure-clinical-data.zip: Skipping, found more recently modified local copy (use --force to force download)



```python
import zipfile
with zipfile.ZipFile("heart-failure-clinical-data.zip","r") as zip_ref:
    zip_ref.extractall()
```

### 1-3. Opening the csv file with Pandas library


```python
df = pd.read_csv('heart_failure_clinical_records_dataset.csv')
```


```python
ls
```



## Step 2. Exploratory Data Analysis using descriptive statistics


### 2-1. Analyzing columns



```python
df.head(5)
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
      <th>age</th>
      <th>anaemia</th>
      <th>creatinine_phosphokinase</th>
      <th>diabetes</th>
      <th>ejection_fraction</th>
      <th>high_blood_pressure</th>
      <th>platelets</th>
      <th>serum_creatinine</th>
      <th>serum_sodium</th>
      <th>sex</th>
      <th>smoking</th>
      <th>time</th>
      <th>DEATH_EVENT</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>75.0</td>
      <td>0</td>
      <td>582</td>
      <td>0</td>
      <td>20</td>
      <td>1</td>
      <td>265000.00</td>
      <td>1.9</td>
      <td>130</td>
      <td>1</td>
      <td>0</td>
      <td>4</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>55.0</td>
      <td>0</td>
      <td>7861</td>
      <td>0</td>
      <td>38</td>
      <td>0</td>
      <td>263358.03</td>
      <td>1.1</td>
      <td>136</td>
      <td>1</td>
      <td>0</td>
      <td>6</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>65.0</td>
      <td>0</td>
      <td>146</td>
      <td>0</td>
      <td>20</td>
      <td>0</td>
      <td>162000.00</td>
      <td>1.3</td>
      <td>129</td>
      <td>1</td>
      <td>1</td>
      <td>7</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>50.0</td>
      <td>1</td>
      <td>111</td>
      <td>0</td>
      <td>20</td>
      <td>0</td>
      <td>210000.00</td>
      <td>1.9</td>
      <td>137</td>
      <td>1</td>
      <td>0</td>
      <td>7</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>65.0</td>
      <td>1</td>
      <td>160</td>
      <td>1</td>
      <td>20</td>
      <td>0</td>
      <td>327000.00</td>
      <td>2.7</td>
      <td>116</td>
      <td>0</td>
      <td>0</td>
      <td>8</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 299 entries, 0 to 298
    Data columns (total 13 columns):
     #   Column                    Non-Null Count  Dtype  
    ---  ------                    --------------  -----  
     0   age                       299 non-null    float64
     1   anaemia                   299 non-null    int64  
     2   creatinine_phosphokinase  299 non-null    int64  
     3   diabetes                  299 non-null    int64  
     4   ejection_fraction         299 non-null    int64  
     5   high_blood_pressure       299 non-null    int64  
     6   platelets                 299 non-null    float64
     7   serum_creatinine          299 non-null    float64
     8   serum_sodium              299 non-null    int64  
     9   sex                       299 non-null    int64  
     10  smoking                   299 non-null    int64  
     11  time                      299 non-null    int64  
     12  DEATH_EVENT               299 non-null    int64  
    dtypes: float64(3), int64(10)
    memory usage: 30.5 KB



```python
df.describe()
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
      <th>age</th>
      <th>anaemia</th>
      <th>creatinine_phosphokinase</th>
      <th>diabetes</th>
      <th>ejection_fraction</th>
      <th>high_blood_pressure</th>
      <th>platelets</th>
      <th>serum_creatinine</th>
      <th>serum_sodium</th>
      <th>sex</th>
      <th>smoking</th>
      <th>time</th>
      <th>DEATH_EVENT</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>299.000000</td>
      <td>299.000000</td>
      <td>299.000000</td>
      <td>299.000000</td>
      <td>299.000000</td>
      <td>299.000000</td>
      <td>299.000000</td>
      <td>299.00000</td>
      <td>299.000000</td>
      <td>299.000000</td>
      <td>299.00000</td>
      <td>299.000000</td>
      <td>299.00000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>60.833893</td>
      <td>0.431438</td>
      <td>581.839465</td>
      <td>0.418060</td>
      <td>38.083612</td>
      <td>0.351171</td>
      <td>263358.029264</td>
      <td>1.39388</td>
      <td>136.625418</td>
      <td>0.648829</td>
      <td>0.32107</td>
      <td>130.260870</td>
      <td>0.32107</td>
    </tr>
    <tr>
      <th>std</th>
      <td>11.894809</td>
      <td>0.496107</td>
      <td>970.287881</td>
      <td>0.494067</td>
      <td>11.834841</td>
      <td>0.478136</td>
      <td>97804.236869</td>
      <td>1.03451</td>
      <td>4.412477</td>
      <td>0.478136</td>
      <td>0.46767</td>
      <td>77.614208</td>
      <td>0.46767</td>
    </tr>
    <tr>
      <th>min</th>
      <td>40.000000</td>
      <td>0.000000</td>
      <td>23.000000</td>
      <td>0.000000</td>
      <td>14.000000</td>
      <td>0.000000</td>
      <td>25100.000000</td>
      <td>0.50000</td>
      <td>113.000000</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>4.000000</td>
      <td>0.00000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>51.000000</td>
      <td>0.000000</td>
      <td>116.500000</td>
      <td>0.000000</td>
      <td>30.000000</td>
      <td>0.000000</td>
      <td>212500.000000</td>
      <td>0.90000</td>
      <td>134.000000</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>73.000000</td>
      <td>0.00000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>60.000000</td>
      <td>0.000000</td>
      <td>250.000000</td>
      <td>0.000000</td>
      <td>38.000000</td>
      <td>0.000000</td>
      <td>262000.000000</td>
      <td>1.10000</td>
      <td>137.000000</td>
      <td>1.000000</td>
      <td>0.00000</td>
      <td>115.000000</td>
      <td>0.00000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>70.000000</td>
      <td>1.000000</td>
      <td>582.000000</td>
      <td>1.000000</td>
      <td>45.000000</td>
      <td>1.000000</td>
      <td>303500.000000</td>
      <td>1.40000</td>
      <td>140.000000</td>
      <td>1.000000</td>
      <td>1.00000</td>
      <td>203.000000</td>
      <td>1.00000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>95.000000</td>
      <td>1.000000</td>
      <td>7861.000000</td>
      <td>1.000000</td>
      <td>80.000000</td>
      <td>1.000000</td>
      <td>850000.000000</td>
      <td>9.40000</td>
      <td>148.000000</td>
      <td>1.000000</td>
      <td>1.00000</td>
      <td>285.000000</td>
      <td>1.00000</td>
    </tr>
  </tbody>
</table>
</div>



#### Applying df.describe(), one can check:

    - If binary data are balanced or imblanced
    (for example, the mean of sex is 0.64 and that of smoking is 0.32.
    This implies there are more data of patients who are men than women.
    The data include more of non-smokers than smokers.)
    - If ther are extreme outliers
    - Increase rate of each percentile
    - If some variables are correlated

### 2-2. Drawing histogram on numerical data


```python
import seaborn as sns
sns.histplot(data=df, x='age')
```



![output_19_1](/images/2021-03-02/output_19_1.png)
    



```python
sns.histplot(data=df, x='age', hue='DEATH_EVENT', kde=True)
```




​    
![output_20_1](/images/2021-03-02/output_20_1.png)
​    



```python
sns.histplot(data=df.loc[df['creatinine_phosphokinase'] < 3000, 'creatinine_phosphokinase'])
```




​    
![output_21_1](/images/2021-03-02/output_21_1.png)
​    



```python
sns.histplot(data=df, x='ejection_fraction', bins=13, hue='DEATH_EVENT', kde=True)
```





​    
![output_22_1](/images/2021-03-02/output_22_1.png)
​    



```python
sns.histplot(data=df, x='platelets', hue='DEATH_EVENT', kde=True)
```








​    
![output_23_1](/images/2021-03-02/output_23_1.png)
​    



```python
sns.jointplot(x='platelets', y='creatinine_phosphokinase', hue='DEATH_EVENT', data=df, alpha=0.3)
```





​    
![output_24_1](/images/2021-03-02/output_24_1.png)
​    


### 2-3. Drawing boxplot on categorical data


```python
sns.boxplot(x='DEATH_EVENT', y='ejection_fraction', data=df)
```





​    
![output_26_1](/images/2021-03-02/output_26_1.png)
​    



```python
sns.boxplot(x='smoking', y='ejection_fraction', data=df)
```





​    
![output_27_1](/images/2021-03-02/output_27_1.png)
​    



```python
sns.violinplot(x='DEATH_EVENT', y='ejection_fraction', hue='smoking', data=df)
```





​    
![output_28_1](/images/2021-03-02/output_28_1.png)
​    



```python
sns.swarmplot(x='DEATH_EVENT', y='platelets', hue='smoking', data=df)
```



​    
![output_29_2](/images/2021-03-02/output_29_2.png)
​    


## Step 3. Data pre-processing for modeling


### 3-1. Feature scaling continous data with StandardScaler


```python
from sklearn.preprocessing import StandardScaler
```


```python
df.head(5)
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
      <th>age</th>
      <th>anaemia</th>
      <th>creatinine_phosphokinase</th>
      <th>diabetes</th>
      <th>ejection_fraction</th>
      <th>high_blood_pressure</th>
      <th>platelets</th>
      <th>serum_creatinine</th>
      <th>serum_sodium</th>
      <th>sex</th>
      <th>smoking</th>
      <th>time</th>
      <th>DEATH_EVENT</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>75.0</td>
      <td>0</td>
      <td>582</td>
      <td>0</td>
      <td>20</td>
      <td>1</td>
      <td>265000.00</td>
      <td>1.9</td>
      <td>130</td>
      <td>1</td>
      <td>0</td>
      <td>4</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>55.0</td>
      <td>0</td>
      <td>7861</td>
      <td>0</td>
      <td>38</td>
      <td>0</td>
      <td>263358.03</td>
      <td>1.1</td>
      <td>136</td>
      <td>1</td>
      <td>0</td>
      <td>6</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>65.0</td>
      <td>0</td>
      <td>146</td>
      <td>0</td>
      <td>20</td>
      <td>0</td>
      <td>162000.00</td>
      <td>1.3</td>
      <td>129</td>
      <td>1</td>
      <td>1</td>
      <td>7</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>50.0</td>
      <td>1</td>
      <td>111</td>
      <td>0</td>
      <td>20</td>
      <td>0</td>
      <td>210000.00</td>
      <td>1.9</td>
      <td>137</td>
      <td>1</td>
      <td>0</td>
      <td>7</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>65.0</td>
      <td>1</td>
      <td>160</td>
      <td>1</td>
      <td>20</td>
      <td>0</td>
      <td>327000.00</td>
      <td>2.7</td>
      <td>116</td>
      <td>0</td>
      <td>0</td>
      <td>8</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
# dividing data into numerical, categorical and output
X_num= df[['age', 'creatinine_phosphokinase', 'ejection_fraction', 'platelets',
       'serum_creatinine', 'serum_sodium', 'time']]
X_cat = df[['anaemia', 'diabetes', 'high_blood_pressure', 'sex', 'smoking']]
y = df['DEATH_EVENT'] 
```


```python
# Feature scaling continous data and integrating them into numpy array
scaler = StandardScaler()
scaler.fit(X_num)
X_scaled = scaler.transform(X_num)
X_scaled
```




    array([[ 1.19294523e+00,  1.65728387e-04, -1.53055953e+00, ...,
             4.90056987e-01, -1.50403612e+00, -1.62950241e+00],
           [-4.91279276e-01,  7.51463953e+00, -7.07675018e-03, ...,
            -2.84552352e-01, -1.41976151e-01, -1.60369074e+00],
           [ 3.50832977e-01, -4.49938761e-01, -1.53055953e+00, ...,
            -9.09000174e-02, -1.73104612e+00, -1.59078490e+00],
           ...,
           [-1.33339153e+00,  1.52597865e+00,  1.85495776e+00, ...,
            -5.75030855e-01,  3.12043840e-01,  1.90669738e+00],
           [-1.33339153e+00,  1.89039811e+00, -7.07675018e-03, ...,
             5.92615005e-03,  7.66063830e-01,  1.93250906e+00],
           [-9.12335403e-01, -3.98321274e-01,  5.85388775e-01, ...,
             1.99578485e-01, -1.41976151e-01,  1.99703825e+00]])




```python
X_scaled = pd.DataFrame(data=X_scaled, index=X_num.index, columns=X_num.columns)
```


```python
X = pd.concat([X_scaled, X_cat], axis=1)
```


```python
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
      <th>age</th>
      <th>creatinine_phosphokinase</th>
      <th>ejection_fraction</th>
      <th>platelets</th>
      <th>serum_creatinine</th>
      <th>serum_sodium</th>
      <th>time</th>
      <th>anaemia</th>
      <th>diabetes</th>
      <th>high_blood_pressure</th>
      <th>sex</th>
      <th>smoking</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.192945</td>
      <td>0.000166</td>
      <td>-1.530560</td>
      <td>1.681648e-02</td>
      <td>0.490057</td>
      <td>-1.504036</td>
      <td>-1.629502</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-0.491279</td>
      <td>7.514640</td>
      <td>-0.007077</td>
      <td>7.535660e-09</td>
      <td>-0.284552</td>
      <td>-0.141976</td>
      <td>-1.603691</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.350833</td>
      <td>-0.449939</td>
      <td>-1.530560</td>
      <td>-1.038073e+00</td>
      <td>-0.090900</td>
      <td>-1.731046</td>
      <td>-1.590785</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.912335</td>
      <td>-0.486071</td>
      <td>-1.530560</td>
      <td>-5.464741e-01</td>
      <td>0.490057</td>
      <td>0.085034</td>
      <td>-1.590785</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.350833</td>
      <td>-0.435486</td>
      <td>-1.530560</td>
      <td>6.517986e-01</td>
      <td>1.264666</td>
      <td>-4.682176</td>
      <td>-1.577879</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
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
    </tr>
    <tr>
      <th>294</th>
      <td>0.098199</td>
      <td>-0.537688</td>
      <td>-0.007077</td>
      <td>-1.109765e+00</td>
      <td>-0.284552</td>
      <td>1.447094</td>
      <td>1.803451</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>295</th>
      <td>-0.491279</td>
      <td>1.278215</td>
      <td>-0.007077</td>
      <td>6.802472e-02</td>
      <td>-0.187726</td>
      <td>0.539054</td>
      <td>1.816357</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>296</th>
      <td>-1.333392</td>
      <td>1.525979</td>
      <td>1.854958</td>
      <td>4.902082e+00</td>
      <td>-0.575031</td>
      <td>0.312044</td>
      <td>1.906697</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>297</th>
      <td>-1.333392</td>
      <td>1.890398</td>
      <td>-0.007077</td>
      <td>-1.263389e+00</td>
      <td>0.005926</td>
      <td>0.766064</td>
      <td>1.932509</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>298</th>
      <td>-0.912335</td>
      <td>-0.398321</td>
      <td>0.585389</td>
      <td>1.348231e+00</td>
      <td>0.199578</td>
      <td>-0.141976</td>
      <td>1.997038</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>299 rows × 12 columns</p>
</div>




```python
X.head()
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
      <th>age</th>
      <th>creatinine_phosphokinase</th>
      <th>ejection_fraction</th>
      <th>platelets</th>
      <th>serum_creatinine</th>
      <th>serum_sodium</th>
      <th>time</th>
      <th>anaemia</th>
      <th>diabetes</th>
      <th>high_blood_pressure</th>
      <th>sex</th>
      <th>smoking</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.192945</td>
      <td>0.000166</td>
      <td>-1.530560</td>
      <td>1.681648e-02</td>
      <td>0.490057</td>
      <td>-1.504036</td>
      <td>-1.629502</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-0.491279</td>
      <td>7.514640</td>
      <td>-0.007077</td>
      <td>7.535660e-09</td>
      <td>-0.284552</td>
      <td>-0.141976</td>
      <td>-1.603691</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.350833</td>
      <td>-0.449939</td>
      <td>-1.530560</td>
      <td>-1.038073e+00</td>
      <td>-0.090900</td>
      <td>-1.731046</td>
      <td>-1.590785</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.912335</td>
      <td>-0.486071</td>
      <td>-1.530560</td>
      <td>-5.464741e-01</td>
      <td>0.490057</td>
      <td>0.085034</td>
      <td>-1.590785</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.350833</td>
      <td>-0.435486</td>
      <td>-1.530560</td>
      <td>6.517986e-01</td>
      <td>1.264666</td>
      <td>-4.682176</td>
      <td>-1.577879</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



### 3-2. Dividing the data into train and test dataset



```python
from sklearn.model_selection import train_test_split
```


```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=1)
```


```python
X_train
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
      <th>age</th>
      <th>creatinine_phosphokinase</th>
      <th>ejection_fraction</th>
      <th>platelets</th>
      <th>serum_creatinine</th>
      <th>serum_sodium</th>
      <th>time</th>
      <th>anaemia</th>
      <th>diabetes</th>
      <th>high_blood_pressure</th>
      <th>sex</th>
      <th>smoking</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>79</th>
      <td>-0.491279</td>
      <td>-0.253792</td>
      <td>0.585389</td>
      <td>0.621074</td>
      <td>-0.478205</td>
      <td>0.766064</td>
      <td>-0.726094</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>282</th>
      <td>-1.586025</td>
      <td>-0.534591</td>
      <td>-0.684180</td>
      <td>-0.495266</td>
      <td>2.329754</td>
      <td>-1.958056</td>
      <td>1.545334</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>297</th>
      <td>-1.333392</td>
      <td>1.890398</td>
      <td>-0.007077</td>
      <td>-1.263389</td>
      <td>0.005926</td>
      <td>0.766064</td>
      <td>1.932509</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>233</th>
      <td>-0.659702</td>
      <td>0.129209</td>
      <td>-0.007077</td>
      <td>0.682524</td>
      <td>0.005926</td>
      <td>0.085034</td>
      <td>1.016195</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>106</th>
      <td>-0.491279</td>
      <td>0.171536</td>
      <td>0.585389</td>
      <td>-0.003667</td>
      <td>-0.090900</td>
      <td>0.085034</td>
      <td>-0.545412</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
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
    </tr>
    <tr>
      <th>203</th>
      <td>-0.070223</td>
      <td>-0.539753</td>
      <td>-1.107370</td>
      <td>-0.525991</td>
      <td>2.039276</td>
      <td>-0.141976</td>
      <td>0.732266</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>255</th>
      <td>-0.743913</td>
      <td>-0.403483</td>
      <td>-0.684180</td>
      <td>0.723490</td>
      <td>-0.381379</td>
      <td>1.220084</td>
      <td>1.106535</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>72</th>
      <td>2.035057</td>
      <td>5.471619</td>
      <td>-0.260991</td>
      <td>-0.208500</td>
      <td>-0.381379</td>
      <td>-1.050016</td>
      <td>-0.751905</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>235</th>
      <td>1.361368</td>
      <td>-0.488136</td>
      <td>1.008578</td>
      <td>1.460889</td>
      <td>-0.284552</td>
      <td>0.085034</td>
      <td>1.016195</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>37</th>
      <td>1.782424</td>
      <td>0.281997</td>
      <td>1.008578</td>
      <td>0.590349</td>
      <td>-0.381379</td>
      <td>1.901114</td>
      <td>-1.293951</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>200 rows × 12 columns</p>
</div>



## Step 4. Applying classification models

### 4-1. Applying Logistic Regression model


```python
from sklearn.linear_model import LogisticRegression
```


```python
model_lr = LogisticRegression()
model_lr.fit(X_train, y_train)
```




    LogisticRegression()



### 4-2. Evaluating classification performance of Logistic Regression model


```python
from sklearn.metrics import classification_report
```


```python
pred = model_lr.predict(X_test)
print(classification_report(y_test, pred))


```

                  precision    recall  f1-score   support
    
               0       0.86      0.90      0.88        70
               1       0.73      0.66      0.69        29
    
        accuracy                           0.83        99
       macro avg       0.80      0.78      0.79        99
    weighted avg       0.82      0.83      0.83        99


​    

### 4-3. Applying XGBoost model


```python
from xgboost import XGBClassifier
```


```python
model_xgb = XGBClassifier()
model_xgb.fit(X_train, y_train)
```

   

    XGBClassifier(base_score=0.5, booster='gbtree', colsample_bylevel=1,
                  colsample_bynode=1, colsample_bytree=1, gamma=0, gpu_id=-1,
                  importance_type='gain', interaction_constraints='',
                  learning_rate=0.300000012, max_delta_step=0, max_depth=6,
                  min_child_weight=1, missing=nan, monotone_constraints='()',
                  n_estimators=100, n_jobs=8, num_parallel_tree=1, random_state=0,
                  reg_alpha=0, reg_lambda=1, scale_pos_weight=1, subsample=1,
                  tree_method='exact', validate_parameters=1, verbosity=None)



### 4-4. Evaluating classification performance of XGBoost model



```python
pred = model_xgb.predict(X_test)
print(classification_report(y_test, pred))
```

                  precision    recall  f1-score   support
    
               0       0.90      0.91      0.91        70
               1       0.79      0.76      0.77        29
    
        accuracy                           0.87        99
       macro avg       0.84      0.84      0.84        99
    weighted avg       0.87      0.87      0.87        99


​    

## Too good to be true!


```python
# The accuracy is almost 90%. Maybe there is something missing!
```



### 4-5. Finding important features


```python
# What is the important features of XGBClassifier model?
plt.plot(model_xgb.feature_importances_)
```





![output_58_1](/images/2021-03-02/output_58_1.png)
    



```python
plt.bar(X.columns, model_xgb.feature_importances_)
plt.xticks(rotation=90)
plt.show()
```


​    
![output_59_0](/images/2021-03-02/output_59_0.png)
​    



```python
sns.histplot(x='time', data=df, hue='DEATH_EVENT', kde=True)
```



​    
![output_61_1](/images/2021-03-02/output_61_1.png)
​    


### Time as the most important feature?
    - Time refers to follow-up period.
    - Time strongly depends on the death event.
    - Therefore, It could have been better to get rid of 'time'data.

### 4-6. Training again without time data


```python
X_num= df[['age', 'creatinine_phosphokinase', 'ejection_fraction', 'platelets',
       'serum_creatinine', 'serum_sodium']]
X_cat = df[['anaemia', 'diabetes', 'high_blood_pressure', 'sex', 'smoking']]
y = df['DEATH_EVENT'] 
```


```python
scaler = StandardScaler()
scaler.fit(X_num)
X_scaled = scaler.transform(X_num)
X_scaled = pd.DataFrame(data=X_scaled, index=X_num.index, columns=X_num.columns)
X = pd.concat([X_scaled, X_cat], axis=1)
```


```python
X.head()
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
      <th>age</th>
      <th>creatinine_phosphokinase</th>
      <th>ejection_fraction</th>
      <th>platelets</th>
      <th>serum_creatinine</th>
      <th>serum_sodium</th>
      <th>anaemia</th>
      <th>diabetes</th>
      <th>high_blood_pressure</th>
      <th>sex</th>
      <th>smoking</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.192945</td>
      <td>0.000166</td>
      <td>-1.530560</td>
      <td>1.681648e-02</td>
      <td>0.490057</td>
      <td>-1.504036</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-0.491279</td>
      <td>7.514640</td>
      <td>-0.007077</td>
      <td>7.535660e-09</td>
      <td>-0.284552</td>
      <td>-0.141976</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.350833</td>
      <td>-0.449939</td>
      <td>-1.530560</td>
      <td>-1.038073e+00</td>
      <td>-0.090900</td>
      <td>-1.731046</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.912335</td>
      <td>-0.486071</td>
      <td>-1.530560</td>
      <td>-5.464741e-01</td>
      <td>0.490057</td>
      <td>0.085034</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.350833</td>
      <td>-0.435486</td>
      <td>-1.530560</td>
      <td>6.517986e-01</td>
      <td>1.264666</td>
      <td>-4.682176</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=1)
```


```python
model_lr = LogisticRegression(max_iter=1000)
model_lr.fit(X_train, y_train)
```




    LogisticRegression(max_iter=1000)




```python
pred = model_lr.predict(X_test)
print(classification_report(y_test, pred))
```

                  precision    recall  f1-score   support
    
               0       0.78      0.92      0.84        64
               1       0.64      0.35      0.45        26
    
        accuracy                           0.76        90
       macro avg       0.71      0.63      0.65        90
    weighted avg       0.74      0.76      0.73        90


​    


```python
model_xgb = XGBClassifier()
model_xgb.fit(X_train, y_train)
```

    

    XGBClassifier(base_score=0.5, booster='gbtree', colsample_bylevel=1,
                  colsample_bynode=1, colsample_bytree=1, gamma=0, gpu_id=-1,
                  importance_type='gain', interaction_constraints='',
                  learning_rate=0.300000012, max_delta_step=0, max_depth=6,
                  min_child_weight=1, missing=nan, monotone_constraints='()',
                  n_estimators=100, n_jobs=8, num_parallel_tree=1, random_state=0,
                  reg_alpha=0, reg_lambda=1, scale_pos_weight=1, subsample=1,
                  tree_method='exact', validate_parameters=1, verbosity=None)




```python
pred = model_xgb.predict(X_test)
print(classification_report(y_test, pred))
```

                  precision    recall  f1-score   support
    
               0       0.81      0.88      0.84        64
               1       0.62      0.50      0.55        26
    
        accuracy                           0.77        90
       macro avg       0.72      0.69      0.70        90
    weighted avg       0.76      0.77      0.76        90


​    


```python
plt.bar(X.columns, model_xgb.feature_importances_)
plt.xticks(rotation=90)
plt.show()
```


​    
![output_72_0](/images/2021-03-02/output_72_0.png)
​    


### 4-7. What is the relationship between the two most important features?
    - serum_creatinine as the most important feature.
    - ejection_fraction as the second important feature.


```python
sns.jointplot(x='ejection_fraction', y='serum_creatinine',hue='DEATH_EVENT',data=df)
```




​    
![output_74_1](/images/2021-03-02/output_74_1.png)
​    


## Step 5. In-depth analysis on the learning results

### 5-1. Idetifying Precision-Recall curve


```python
from sklearn.metrics import plot_precision_recall_curve
```


```python
plot_precision_recall_curve(model_lr, X_test, y_test)
```





​    
![output_78_1](/images/2021-03-02/output_78_1.png)
​    



```python
plot_precision_recall_curve(model_xgb, X_test, y_test)
```




![output_79_1](/images/2021-03-02/output_79_1.png)
    



```python
 fig = plt.figure()
 ax = fig.gca()
 plot_precision_recall_curve(model_lr, X_test, y_test,ax=ax)
 plot_precision_recall_curve(model_xgb, X_test, y_test,ax=ax)
```





![output_80_1](/images/2021-03-02/output_80_1.png)
    


### P*R curve implies XGBClassifier yielded better performance on the data
    - The closer to 1, the better performance

### 5-2. Identifying ROC curve


```python
from sklearn.metrics import plot_roc_curve
```


```python
fig = plt.figure()
ax = fig.gca()
plot_roc_curve(model_lr, X_test, y_test,ax=ax)
plot_roc_curve(model_xgb, X_test, y_test,ax=ax)
plt.show()
```


​    
![output_84_0](/images/2021-03-02/output_84_0.png)
​    


### ROC curve implies LogisticRegression yielded better performance on the data
    - The faster reaching 1 while keeping low FPS, the better performance.
    - The higher Area Under Cover(AUC), the better performance.
    - In result, it is difficult to say which model is better than the other.
