---
layout: single
title: "[Decision Tree] Experimenting and visualizing classification and regression trees with different depths"
description: "[Decision Tree] Experimenting and visualizing classification and regression trees with different depths"
headline: "[Decision Tree] Experimenting and visualizing classification and regression trees with different depths"
typora-copy-images-to: ../images/2021-05-19
---



# 1. Decision Classification Tree

### (1) Importing modules and data(iris data)


```python
from sklearn.datasets import load_iris
from sklearn import tree
from os import system
```


```python
system("pip install graphviz")
```




    0




```python
import graphviz 
```


```python
iris=load_iris()
```


```python
iris.data
```




    array([[5.1, 3.5, 1.4, 0.2],
           [4.9, 3. , 1.4, 0.2],
           [4.7, 3.2, 1.3, 0.2],
           [4.6, 3.1, 1.5, 0.2],
           [5. , 3.6, 1.4, 0.2],
           [5.4, 3.9, 1.7, 0.4],
           [4.6, 3.4, 1.4, 0.3],
           [5. , 3.4, 1.5, 0.2],
           [4.4, 2.9, 1.4, 0.2],
           [4.9, 3.1, 1.5, 0.1],
           [5.4, 3.7, 1.5, 0.2],
           [4.8, 3.4, 1.6, 0.2],
           [4.8, 3. , 1.4, 0.1],
           [4.3, 3. , 1.1, 0.1],
           [5.8, 4. , 1.2, 0.2],
           [5.7, 4.4, 1.5, 0.4],
           [5.4, 3.9, 1.3, 0.4],
           [5.1, 3.5, 1.4, 0.3],
           [5.7, 3.8, 1.7, 0.3],
           [5.1, 3.8, 1.5, 0.3],
           [5.4, 3.4, 1.7, 0.2],
           [5.1, 3.7, 1.5, 0.4],
           [4.6, 3.6, 1. , 0.2],
           [5.1, 3.3, 1.7, 0.5],
           [4.8, 3.4, 1.9, 0.2],
           [5. , 3. , 1.6, 0.2],
           [5. , 3.4, 1.6, 0.4],
           [5.2, 3.5, 1.5, 0.2],
           [5.2, 3.4, 1.4, 0.2],
           [4.7, 3.2, 1.6, 0.2],
           [4.8, 3.1, 1.6, 0.2],
           [5.4, 3.4, 1.5, 0.4],
           [5.2, 4.1, 1.5, 0.1],
           [5.5, 4.2, 1.4, 0.2],
           [4.9, 3.1, 1.5, 0.2],
           [5. , 3.2, 1.2, 0.2],
           [5.5, 3.5, 1.3, 0.2],
           [4.9, 3.6, 1.4, 0.1],
           [4.4, 3. , 1.3, 0.2],
           [5.1, 3.4, 1.5, 0.2],
           [5. , 3.5, 1.3, 0.3],
           [4.5, 2.3, 1.3, 0.3],
           [4.4, 3.2, 1.3, 0.2],
           [5. , 3.5, 1.6, 0.6],
           [5.1, 3.8, 1.9, 0.4],
           [4.8, 3. , 1.4, 0.3],
           [5.1, 3.8, 1.6, 0.2],
           [4.6, 3.2, 1.4, 0.2],
           [5.3, 3.7, 1.5, 0.2],
           [5. , 3.3, 1.4, 0.2],
           [7. , 3.2, 4.7, 1.4],
           [6.4, 3.2, 4.5, 1.5],
           [6.9, 3.1, 4.9, 1.5],
           [5.5, 2.3, 4. , 1.3],
           [6.5, 2.8, 4.6, 1.5],
           [5.7, 2.8, 4.5, 1.3],
           [6.3, 3.3, 4.7, 1.6],
           [4.9, 2.4, 3.3, 1. ],
           [6.6, 2.9, 4.6, 1.3],
           [5.2, 2.7, 3.9, 1.4],
           [5. , 2. , 3.5, 1. ],
           [5.9, 3. , 4.2, 1.5],
           [6. , 2.2, 4. , 1. ],
           [6.1, 2.9, 4.7, 1.4],
           [5.6, 2.9, 3.6, 1.3],
           [6.7, 3.1, 4.4, 1.4],
           [5.6, 3. , 4.5, 1.5],
           [5.8, 2.7, 4.1, 1. ],
           [6.2, 2.2, 4.5, 1.5],
           [5.6, 2.5, 3.9, 1.1],
           [5.9, 3.2, 4.8, 1.8],
           [6.1, 2.8, 4. , 1.3],
           [6.3, 2.5, 4.9, 1.5],
           [6.1, 2.8, 4.7, 1.2],
           [6.4, 2.9, 4.3, 1.3],
           [6.6, 3. , 4.4, 1.4],
           [6.8, 2.8, 4.8, 1.4],
           [6.7, 3. , 5. , 1.7],
           [6. , 2.9, 4.5, 1.5],
           [5.7, 2.6, 3.5, 1. ],
           [5.5, 2.4, 3.8, 1.1],
           [5.5, 2.4, 3.7, 1. ],
           [5.8, 2.7, 3.9, 1.2],
           [6. , 2.7, 5.1, 1.6],
           [5.4, 3. , 4.5, 1.5],
           [6. , 3.4, 4.5, 1.6],
           [6.7, 3.1, 4.7, 1.5],
           [6.3, 2.3, 4.4, 1.3],
           [5.6, 3. , 4.1, 1.3],
           [5.5, 2.5, 4. , 1.3],
           [5.5, 2.6, 4.4, 1.2],
           [6.1, 3. , 4.6, 1.4],
           [5.8, 2.6, 4. , 1.2],
           [5. , 2.3, 3.3, 1. ],
           [5.6, 2.7, 4.2, 1.3],
           [5.7, 3. , 4.2, 1.2],
           [5.7, 2.9, 4.2, 1.3],
           [6.2, 2.9, 4.3, 1.3],
           [5.1, 2.5, 3. , 1.1],
           [5.7, 2.8, 4.1, 1.3],
           [6.3, 3.3, 6. , 2.5],
           [5.8, 2.7, 5.1, 1.9],
           [7.1, 3. , 5.9, 2.1],
           [6.3, 2.9, 5.6, 1.8],
           [6.5, 3. , 5.8, 2.2],
           [7.6, 3. , 6.6, 2.1],
           [4.9, 2.5, 4.5, 1.7],
           [7.3, 2.9, 6.3, 1.8],
           [6.7, 2.5, 5.8, 1.8],
           [7.2, 3.6, 6.1, 2.5],
           [6.5, 3.2, 5.1, 2. ],
           [6.4, 2.7, 5.3, 1.9],
           [6.8, 3. , 5.5, 2.1],
           [5.7, 2.5, 5. , 2. ],
           [5.8, 2.8, 5.1, 2.4],
           [6.4, 3.2, 5.3, 2.3],
           [6.5, 3. , 5.5, 1.8],
           [7.7, 3.8, 6.7, 2.2],
           [7.7, 2.6, 6.9, 2.3],
           [6. , 2.2, 5. , 1.5],
           [6.9, 3.2, 5.7, 2.3],
           [5.6, 2.8, 4.9, 2. ],
           [7.7, 2.8, 6.7, 2. ],
           [6.3, 2.7, 4.9, 1.8],
           [6.7, 3.3, 5.7, 2.1],
           [7.2, 3.2, 6. , 1.8],
           [6.2, 2.8, 4.8, 1.8],
           [6.1, 3. , 4.9, 1.8],
           [6.4, 2.8, 5.6, 2.1],
           [7.2, 3. , 5.8, 1.6],
           [7.4, 2.8, 6.1, 1.9],
           [7.9, 3.8, 6.4, 2. ],
           [6.4, 2.8, 5.6, 2.2],
           [6.3, 2.8, 5.1, 1.5],
           [6.1, 2.6, 5.6, 1.4],
           [7.7, 3. , 6.1, 2.3],
           [6.3, 3.4, 5.6, 2.4],
           [6.4, 3.1, 5.5, 1.8],
           [6. , 3. , 4.8, 1.8],
           [6.9, 3.1, 5.4, 2.1],
           [6.7, 3.1, 5.6, 2.4],
           [6.9, 3.1, 5.1, 2.3],
           [5.8, 2.7, 5.1, 1.9],
           [6.8, 3.2, 5.9, 2.3],
           [6.7, 3.3, 5.7, 2.5],
           [6.7, 3. , 5.2, 2.3],
           [6.3, 2.5, 5. , 1.9],
           [6.5, 3. , 5.2, 2. ],
           [6.2, 3.4, 5.4, 2.3],
           [5.9, 3. , 5.1, 1.8]])




```python
iris.feature_names
```




    ['sepal length (cm)',
     'sepal width (cm)',
     'petal length (cm)',
     'petal width (cm)']




```python
iris.target
```




    array([0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
           0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
           0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
           1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1,
           1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
           2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2,
           2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2])




```python
iris.target_names
```




    array(['setosa', 'versicolor', 'virginica'], dtype='<U10')



### (2) Splitting train dataset and test dataset


```python
from sklearn.model_selection import train_test_split
```


```python
X_train, X_test, y_train, y_test = train_test_split(iris.data,iris.target, stratify=iris.target,random_state=123)
```


```python
## 3. Making classificaion decision tree
```


```python
clf=tree.DecisionTreeClassifier(criterion='gini', max_depth=3, random_state=1)
clf=clf.fit(X_train, y_train)
```


```python
y_train_pred = clf.predict(X_train)
```


```python
y_test_pred = clf.predict(X_test)
```


```python
from sklearn.metrics  import accuracy_score
print(accuracy_score(y_train, y_train_pred))
print(accuracy_score(y_test, y_test_pred))
```

    0.9642857142857143
    0.9210526315789473



### (3) Comparing accuracy scores with different depths


```python
from sklearn.metrics import confusion_matrix
```


```python
def compare_depth(max_depth):
    clf=tree.DecisionTreeClassifier(criterion='gini', max_depth=max_depth, random_state=1)
    clf=clf.fit(X_train, y_train)
    
    y_train_pred = clf.predict(X_train)
    y_test_pred = clf.predict(X_test)
    
    print(accuracy_score(y_train, y_train_pred))
    print(confusion_matrix(y_train, y_train_pred))
    print(accuracy_score(y_test, y_test_pred))
    print(confusion_matrix(y_test, y_test_pred))
```


```python
compare_depth(3)
```

    0.9642857142857143
    [[38  0  0]
     [ 0 36  1]
     [ 0  3 34]]
    0.9210526315789473
    [[12  0  0]
     [ 0 12  1]
     [ 0  2 11]]



```python
compare_depth(5)
```

    0.9910714285714286
    [[38  0  0]
     [ 0 37  0]
     [ 0  1 36]]
    0.9736842105263158
    [[12  0  0]
     [ 0 12  1]
     [ 0  0 13]]



```python
compare_depth(7)
```

    1.0
    [[38  0  0]
     [ 0 37  0]
     [ 0  0 37]]
    0.9736842105263158
    [[12  0  0]
     [ 0 12  1]
     [ 0  0 13]]

As it is seen, I made a function for comparing accuracy scores with a different scale of depth. The more depth, the better accuracy yields for train set. This means that the model can explain train data better as it learns data more in details. Yet, when I compared the model with its depth 5 and 7, the accuracy of test dataset was not improved while that of trainset increased up to 100%. It implies that overfitting has occurred between depth = 5 to 7.



### (4) Visualizing classification trees

In the beginning, each class has the same or similar number of sample : setosa(38), versicolor(37), virginica(37). Value implies that how many samples belong to each class.


```python
decisiontree_comparision(3)
```

<center><img src ='/images/2021-05-19/output_28_0.svg'></center>    






```python
decisiontree_comparision(5)
```

<center><img src ='/images/2021-05-19/output_29_0.svg'></center>


​    




```python
decisiontree_comparision(7)
```

<center><img src ='/images/2021-05-19/output_30_0.svg'></center>


​       

Among above three, to alleviate the overfitting problem, I would choose the second classification decision tree where the tree depth is 5.

<br>



# 2. Decision regression tree

### (1) Importing modules and making random data


```python
import numpy as np
from sklearn.tree import DecisionTreeRegressor
import matplotlib.pyplot as plt

rng = np.random.RandomState(1)
X = np.sort(5 * rng.rand(80, 1), axis=0)
y = np.sin(X).ravel()
y[::5] += 3 * (0.5 - rng.rand(16))
```



### (2) Making regression trees with different depths


```python
regr1=tree.DecisionTreeRegressor(max_depth=2)
regr2=tree.DecisionTreeRegressor(max_depth=5)
```


```python
regr1.fit(X,y)
regr2.fit(X,y)
```




    DecisionTreeRegressor(max_depth=5)




```python
y_1=regr1.predict(X)
y_2=regr2.predict(X)
```


```python
from sklearn.metrics import mean_squared_error
```


```python
print(mean_squared_error(y, y_1))
```

    0.12967126328231798



```python
print(mean_squared_error(y, y_2))
```

    0.025236948989861896

The MSE is smaller when the depth=5 than the depth = 2.



### (3) Visualizing how correct regression trees can predict the data with different settings of the depth


```python
X_test=np.arange(0.0,5.0,0.01)[:,np.newaxis]

y_pred_1=regr1.predict(X_test)
y_pred_2=regr2.predict(X_test)
```





The figure below shows when the tree depth is 2 while the next one shows when the depth is 5


```python
plt.figure()
plt.scatter(X, y, s=20, edgecolor="black", c="darkorange", label="data")
plt.plot(X_test, y_pred_1, color="cornflowerblue", label="max_depth=2", linewidth=2)

plt.xlabel("data")
plt.ylabel("target")
plt.title("Decision Tree Regression")
plt.legend()
plt.show()
```

  <center><img src ='/images/2021-05-19/output_46_0.png'></center>



```python
plt.figure()
plt.scatter(X, y, s=20, edgecolor="black", c="darkorange", label="data")
plt.plot(X_test, y_pred_2, color="yellowgreen", label="max_depth=5", linewidth=2)

plt.xlabel("data")
plt.ylabel("target")
plt.title("Decision Tree Regression")
plt.legend()
plt.show()
```

 <center><img src ='/images/2021-05-19/output_47_0.png'></center> 



```python
### Plottingn to see the difference when the tree depth is 2 and 5

plt.figure()
plt.scatter(X, y, s=20, edgecolor="black",
            c="darkorange", label="data")
plt.plot(X_test, y_pred_1, color="cornflowerblue",
         label="max_depth=2", linewidth=2)
plt.plot(X_test, y_pred_2, color="yellowgreen", label="max_depth=5", linewidth=2)
plt.xlabel("data")
plt.ylabel("target")
plt.title("Decision Tree Regression")
plt.legend()
plt.show()
```

  

<center><img src ='/images/2021-05-19/output_48_0.png'></center>

   

The regression model with depth=2 could find a general trend, while the one with depth=5 could predict more in details. However, the latter might react too sensitive to outliers.



### (4) Visualizing regression trees


```python
reg_tree1 = tree.export_graphviz(regr1, out_file=None, 
                                filled=True, rounded=True,  
                                special_characters=True)
```


```python
g_reg_tree1 = graphviz.Source(reg_tree1)
g_reg_tree1
```

<center><img src ='/images/2021-05-19/output_52_0.svg'></center>


  


```python
reg_tree2 = tree.export_graphviz(regr2, out_file=None, 
                                filled=True, rounded=True,  
                                special_characters=True)
```


```python
g_reg_tree2 = graphviz.Source(reg_tree2)
g_reg_tree2
```

<center><img src ='/images/2021-05-19/output_54_0.svg'></center>


​      

Each graph shows regression trees when its depth is 2 and 5. In the regression trees, value refers to a representative value of samples(for instance, the mean of samples).

<br>





**Reference**

- <https://scikit-learn.org/stable/auto_examples/tree/plot_tree_regression.html>
- 세종대*최유경 교수 9주차 의사결정나무 Part4(유튜브 강의): <https://www.youtube.com/watch?v=z683uUHlDeM>
- 패스트 캠퍼스 머신러닝과 데이터분석 A-Z 올인원 패키지 Online 강의

