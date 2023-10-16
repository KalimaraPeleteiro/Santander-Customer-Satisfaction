<h1 align = "center">Santander-Satisfaction-Kaggle</h1> 
<br>
In order to reach a good score and solve a complex machine learning problem, multiple angles and attempts are often necessary. In here, I hope to document my trials on my
journey to find an optimal solution for the Santander Satisfaction Competition, from Kaggle. Although it's a bygone tournament from 7 years ago, I believe there's much I can
learn trying to crack it.
<br>
<br>

[First Attempt](#first-attempt)
  
<br>
<h3 id = "first-attempt">First Attempt</h3>
Overall, there are some key elements to the dataset that need to be adressed at every new try. These factors are its high dimentionality (371 columns) and the major
unbalance of the Target variable.
<br>
<br>

<div align = "center">
  
  ![image](https://github.com/KalimaraPeleteiro/Santander-Satisfaction-Kaggle/assets/94702837/5c38fef8-1d3e-4445-9e74-f33b881b49db)
</div>

<br>
<br>
To adress the second issue, I used the oversampling technique - SMOTE. As for the first issue, I decided to try a combination of feature selection techniques and
algorithms to choose the best set of columns.

Four approaches were used:
1. I calculated the correlation between features and Target, and made 5 lists, with the respective, 10, 20, 30, 40 and 50 most correlated columns.
2. After applyng the ``MinMaxScaler()`` to the dataset, I used the ``VarianceTreshold`` algorithm to make two lists, one with features with more than 5% of variance, and another with features above 1%.
3. I used the ``SelectKBest``algorithm to select the 10, 20, 30, 40, 50 best features with the chi2 method.
4. I used the ``SelectFromModel`` class to return the list of features gathered by LogisticRegression and DecisionTree algorithms.

As a result, I was left with 14 sets of features, to which I submitted to a function that returned the ones that appeared most frequently, creating, at the end,
five sets of features, with columns that appeared in 3, 5, 7, 9, and 11 methods, respectively.

These sets were then submmited to various *scikit-learn* algorithms, using the ``cross_validate`` function.
<br>
<br>

<div align = "center">
  
  ![image](https://github.com/KalimaraPeleteiro/Santander-Satisfaction-Kaggle/assets/94702837/71331869-c315-4f31-85ef-de7175edb2a3)
</div>

<br>
<br>

The best result found was a ``DecisionTreeClassifier`` that was then submitted to a public score of __a private score of 0.6621, and the position 4447 of 5155 teams.__
