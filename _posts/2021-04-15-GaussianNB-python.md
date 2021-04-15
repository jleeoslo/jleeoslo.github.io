---
layout: single
title: "[Naïve Bayes] Practicing Gaussian NB(by setting up a prior probability) with Python"
description: "[Naïve Bayes] Practicing Gaussian NB(by setting up a prior probability) with Python"
headline: "[Naïve Bayes] Practicing Gaussian NB(by setting up a prior probability) with Python"
typora-copy-images-to: ../images/2021-04-15
---

## [Naïve Bayes] Practicing Gaussian NB(by setting up a prior probability) with Python

### (1) Importing modules and data


```python
from sklearn import datasets
from sklearn.naive_bayes import GaussianNB
```


```python
import pandas as pd
```


```python
iris = datasets.load_iris()
```

##### **Data information**


```python
print(iris.DESCR)
```

    .. _iris_dataset:
    
    Iris plants dataset
    --------------------
    
    **Data Set Characteristics:**
    
        :Number of Instances: 150 (50 in each of three classes)
        :Number of Attributes: 4 numeric, predictive attributes and the class
        :Attribute Information:
            - sepal length in cm
            - sepal width in cm
            - petal length in cm
            - petal width in cm
            - class:
                    - Iris-Setosa
                    - Iris-Versicolour
                    - Iris-Virginica
                    
        :Summary Statistics:
    
        ============== ==== ==== ======= ===== ====================
                        Min  Max   Mean    SD   Class Correlation
        ============== ==== ==== ======= ===== ====================
        sepal length:   4.3  7.9   5.84   0.83    0.7826
        sepal width:    2.0  4.4   3.05   0.43   -0.4194
        petal length:   1.0  6.9   3.76   1.76    0.9490  (high!)
        petal width:    0.1  2.5   1.20   0.76    0.9565  (high!)
        ============== ==== ==== ======= ===== ====================
    
        :Missing Attribute Values: None
        :Class Distribution: 33.3% for each of 3 classes.
        :Creator: R.A. Fisher
        :Donor: Michael Marshall (MARSHALL%PLU@io.arc.nasa.gov)
        :Date: July, 1988
    
    The famous Iris database, first used by Sir R.A. Fisher. The dataset is taken
    from Fisher's paper. Note that it's the same as in R, but not as in the UCI
    Machine Learning Repository, which has two wrong data points.
    
    This is perhaps the best known database to be found in the
    pattern recognition literature.  Fisher's paper is a classic in the field and
    is referenced frequently to this day.  (See Duda & Hart, for example.)  The
    data set contains 3 classes of 50 instances each, where each class refers to a
    type of iris plant.  One class is linearly separable from the other 2; the
    latter are NOT linearly separable from each other.
    
    .. topic:: References
    
       - Fisher, R.A. "The use of multiple measurements in taxonomic problems"
         Annual Eugenics, 7, Part II, 179-188 (1936); also in "Contributions to
         Mathematical Statistics" (John Wiley, NY, 1950).
       - Duda, R.O., & Hart, P.E. (1973) Pattern Classification and Scene Analysis.
         (Q327.D83) John Wiley & Sons.  ISBN 0-471-22361-1.  See page 218.
       - Dasarathy, B.V. (1980) "Nosing Around the Neighborhood: A New System
         Structure and Classification Rule for Recognition in Partially Exposed
         Environments".  IEEE Transactions on Pattern Analysis and Machine
         Intelligence, Vol. PAMI-2, No. 1, 67-71.
       - Gates, G.W. (1972) "The Reduced Nearest Neighbor Rule".  IEEE Transactions
         on Information Theory, May 1972, 431-433.
       - See also: 1988 MLC Proceedings, 54-64.  Cheeseman et al"s AUTOCLASS II
         conceptual clustering system finds 3 classes in the data.
       - Many, many more ...



```python
# Four numeric, predictive features
print(iris.data)
print(iris.feature_names)
```

    [[5.1 3.5 1.4 0.2]
     [4.9 3.  1.4 0.2]
     [4.7 3.2 1.3 0.2]
     [4.6 3.1 1.5 0.2]
            .
            .
            .
     [6.7 3.  5.2 2.3]
     [6.3 2.5 5.  1.9]
     [6.5 3.  5.2 2. ]
     [6.2 3.4 5.4 2.3]
     [5.9 3.  5.1 1.8]]
    ['sepal length (cm)', 'sepal width (cm)', 'petal length (cm)', 'petal width (cm)']



```python
# Three lables
print(iris.target)
print(iris.target_names)
```

    [0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
     0 0 0 0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
     1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 2
     2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
     2 2]
    ['setosa' 'versicolor' 'virginica']

<br>



### (2) Converting the iris dataset into a data frame form and separating the features and the labels for modeling


```python
df_X = pd.DataFrame(iris.data) # features
df_Y = pd.DataFrame(iris.target) # labes
```


```python
df_X.head()
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
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_Y.head()
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
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>

<br>



### (3) Fitting Gaussian Naïve Bayes


```python
gnb = GaussianNB()
fitted = gnb.fit(iris.data, iris.target)
y_pred = fitted.predict(iris.data)
```


```python
# To see the probabilites of assigning each label given features of instance 0, 50, 100 and 149
fitted.predict_proba(iris.data)[[0,50,100,149]]
```




    array([[1.00000000e+000, 1.35784265e-018, 7.11283512e-026],
           [3.21380935e-109, 8.04037666e-001, 1.95962334e-001],
           [3.23245181e-254, 6.35381031e-011, 1.00000000e+000],
           [3.25988243e-146, 5.60050092e-002, 9.43994991e-001]])




```python
# Each instance was assigned a label with the highest probability.
fitted.predict(iris.data)[[0,50,100,149]]
```




    array([0, 1, 2, 2])

<br>



### (4) Computing confusion matrix to evaluate the accuracy of the Gaussian Naïve Bayes classifier


```python
from sklearn.metrics import confusion_matrix
```


```python
confusion_matrix(iris.target,y_pred)
```




    array([[50,  0,  0],
           [ 0, 47,  3],
           [ 0,  3, 47]], dtype=int64)



- The Gaussian Naïve Bayes classifier is good at predicting setosa(0), yet can misclassify versicolor(1) with virginica(2) at times.

<br>



### (5) Setting up a prior probability

- A prior probability distribution, often simply called the prior, of an uncertain quantity is the probability distribution that would express one's beliefs about this quantity before an experiment is performed


```python
# For instance, let's assume that there are a lot more virginica(2) than the others.
gnb2 = GaussianNB(priors=[1/100,1/100,98/100])
fitted2 = gnb2.fit(iris.data, iris.target)
y_pred2 =  fitted2.predict(iris.data)
confusion_matrix(iris.target,y_pred2)
```




    array([[50,  0,  0],
           [ 0, 33, 17],
           [ 0,  0, 50]], dtype=int64)



- Now, the prediction for virginica(2) was much improved. However, as a trade-off, the prediction for versicolor(1) became worse.


```python
# This time let's assume that there are a lot more versicolor(1) than the others.
gnb3 = GaussianNB(priors=[1/100,98/100,1/100])
fitted3 = gnb3.fit(iris.data, iris.target)
y_pred3 =  fitted3.predict(iris.data)
confusion_matrix(iris.target,y_pred3)
```




    array([[50,  0,  0],
           [ 0, 50,  0],
           [ 0, 14, 36]], dtype=int64)



- The result is reverse from before. The prediction for versicolor(1) was much improved, yet that of versicolor(1) became worse.



**reference**

- 패스트 캠퍼스 머신러닝과 데이터분석 A-Z 올인원 패키지 Online 강의