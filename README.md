<h1 align = "center">Santander-Satisfaction-Kaggle</h1> 
<br>
In order to reach a good score and solve a complex machine learning problem, multiple angles and attempts are often necessary. In here, I hope to document my trials on my
journey to find an optimal solution for the Santander Satisfaction Competition, from Kaggle. Although it's a bygone tournament from 7 years ago, I believe there's much I can
learn trying to crack it.
<br>
<br>

[First Attempt](#first-attempt)

[Second Attempt](#second-attempt)

[Third Attempt](#third-attempt)

[Fourth Attempt](#fourth-attempt)


  
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

The best result found was a ``DecisionTreeClassifier`` that was then submitted to a public score of __a private score of 0.65662.__

<br>
<br>
<h3 id = "second-attempt">Second Attempt</h3>
In my second attempt, the target was to study the effects of Undersampling and Oversampling into the result. After cleaning the dataser from the already known unchageable columns, I Used the ``SelectKBest`` algorithm to generate 5 different set of columns. Since in the first attempt, larger sets showed greater performance, this time, I created sets of 25, 50, 75, 100 and 150 columns, and separated them into 10 datasets, with one with oversampling and another with undersampling for each set.
<br>
<br>
From training, some conclusions could be apparently drawn.

- Tree algorithms (RandomForest, ExtraTrees, DecisionTree...) showed great promise in both attempts so far.
- Larger sets of columns impacted positively in some cases, albeit slightly.
- Undersampled datasets showed significant worse performance in all cases.

At the end, the best results were the RandomForest algorithm with 100 set of columns for oversampling and 150 sets for undersampling.

The answers resulted were then compared and submitted.

<div align = "center">
  The First Attempt

  ![image](https://github.com/KalimaraPeleteiro/Santander-Satisfaction-Kaggle/assets/94702837/61b35568-5bb6-4f20-9869-6a7f3f2c5bd3)
</div>

<div align = "center">
  The Undersampling Answer

  ![image](https://github.com/KalimaraPeleteiro/Santander-Satisfaction-Kaggle/assets/94702837/dd90ead5-a1fa-4e81-9695-9b374398e982)
</div>

<div align = "center">
  The Oversampling Answer

  ![image](https://github.com/KalimaraPeleteiro/Santander-Satisfaction-Kaggle/assets/94702837/e5e8a99a-5fef-4fbc-ba3a-62bae248f400)
</div>

<br>

The two answers were submitted. The Undersampling Answer resulted in a **private score of 0.68632**. The Oversampling Answer resulted in a **private score of 0.6884**.

<br>
<br>

<h3 id = "third-attempt">Third Attempt</h3>

The third attempt aimed at discovering the impact of feature selection, Undersampling and Oversampling in the dataset. This time, the only columns removed from the dataset were the ones who were constant (whose values didn't change). Otherwise, all columns were used. Two datasets were made from this with different approaches: one with Oversampling made with ``SMOTE``, and another with Undersampling made with ``RandomUnderSampler``.
<br>
<br>

The best results were made with the ``VotingClassifier``, searching to complement each Classifier mistakes with one of another kind.
<br>

The results were great. The Oversampling method resulted in a **private score of 0.7067**, while the Undersampling method resulted in a **private score of 0.73255**.

These results show that the Undersampling is similarly the correct approach, that Feature Selection isn't needed for performance, and that the VotingClassifier is promising.
<br>
<br>

<h3 id = "fourht-attempt">Fourth Attempt</h3>

Searching to confirm these findings, the Fourth Attempt consisted at a tentative to create an "Ultimate ``VotingClassifier``", in a undersampled dataset with all non-constant columns. In theory, this approach, making use of classifiers optimized with ``RandomizedSearchCV`` should yield better results.
<br>
<br>

The chosen classifiers were as follows:
<br>

1. A KNN Classifier
2. A LogisticRegression Classifier
3. A SVC with kernel RBF Classifier
4. A Bagging RandomForestClassifier
5. A Boosting GradientBoostingClassifier
6. A MLPClassifier
<br>

All of them were optimized and joined in a single classifier. The end result, however, wasn't the expected, with a __private score of 0.72855__, a downgrade from the previous result, hinting at some level of overfitting, maybe.
