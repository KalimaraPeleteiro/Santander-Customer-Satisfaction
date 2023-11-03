<h1 align = "center">Santander Customer Satisfaction (Kaggle Competition)</h1> 
<br>
In order to reach a good score and solve a complex machine learning problem, multiple angles and attempts are often necessary. In here, I hope to document my trials on my
journey to find an optimal solution for the Santander Satisfaction Competition, from Kaggle. Although it's a bygone tournament from 7 years ago, I believe there's much I can
learn trying to crack it.
<br>
<br>

> __Important info__: In Kaggle Competitions, there are two type of scores: the *Public Score*, used to rank the public leaderboard, and the *Private Score*, used to
> order the final positions of the competition. Since the other's private scores aren't available for comparison, I'll focus on the public score instead.
<br>

<h3 align = "center">Results so Far</h3> 
<h5 align = "center">Best Public Score: 0.82907</h5> 


<div align = "center">

| Attempt  | Model | Public Score | Private Score | 
| :---: | :---: |  :---: |  :---: |
| 1st | ``DecisionTreeClassifier`` | 0.65662 | 0.6621 |
| 2nd (Oversampling)  | ``RandomForestClassifier`` | 0.69402 | 0.6884 |
| 2nd (Undersampling)  | ``DecisionTreeClassifier`` | 0.68533 | 0.68632 |
| 3rd (Oversampling)  | ``VotingClassifier`` | 0.71566 | 0.7067 |
| 3rd (Undersampling)  | ``VotingClassifier`` | 0.74544 | 0.73255 |
| 4th | ``VotingClassifier`` | 0.74268 | 0.72855 |
| 5th | ``Keras.Sequential()`` | 0.73499 | 0.72331 |
| 6th (Oversampling) | ``Keras.Sequential()`` | 0.71362 | 0.68998 |
| 6th (Undersampling) | ``Keras.Sequential()`` | 0.71995 | 0.71407 |
| 7h (Stacking) | ``StackingClassifier()`` | 0.74151 | 0.73156 |
| __Best Public Score__   <br> 7h (Voting)| ``VotingClassifier()`` | 0.74636 | 0.73214 |
| __Best Private Score__ <br> 7h (Voting)| ``VotingClassifier()`` | 0.74544 | 0.7351 |






</div>

<br>
<br>

[First Attempt](#first-attempt)

[Second Attempt](#second-attempt)

[Third Attempt](#third-attempt)

[Fourth Attempt](#fourth-attempt)

[Fifth Attempt](#fifth-attempt)

[Sixth Attempt](#sixth-attempt)

[Seventh Attempt](#seventh-attempt)





  
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

<h3 id = "fourth-attempt">Fourth Attempt</h3>

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
<br>
<br>

<h3 id = "fifth-attempt">Fifth Attempt</h3>

This time, I wanted to test the performance of a specialized NeuralNetwork in the data. So, I created a simple ``Keras`` classification network, and optimized its parameters later with the ``KerasClassifier`` wrapper for scikit-learn from the Tensorflow library.

After only ten iterations (and 16 minutes of runtime), the result was the following network.

<div align = "center">

  ![image](https://github.com/KalimaraPeleteiro/Santander-Satisfaction-Kaggle/assets/94702837/b4d2f97a-4358-4365-b742-33c23b7eb198)
</div>

I used the network to predict the results, ending with a __private score of 0.72331__, showing that neural networks exhibit great potential for future attempts.

<br>
<br>

<h3 id = "sixth-attempt">Sixth Attempt</h3>

In the sixth attempt, I tried to tune more with neural networks, exploring its parameters and results. Plotting the performance of the earlier model (5th Attempt) coupled with the results of earlier models in the notebook, it was clear that the neural network was finding major difficulties to generalize.

<div align = "center">

  __Neural Network in Oversampling Dataset__

  ![__results___43_0](https://github.com/KalimaraPeleteiro/Santander-Satisfaction-Kaggle/assets/94702837/50467a11-4469-4c9a-ba47-c27249c35b83)
</div>

<br>

<div align = "center">

  __5th Attempt Model (In Undersampling Dataset)__
  
  ![__results___50_0](https://github.com/KalimaraPeleteiro/Santander-Satisfaction-Kaggle/assets/94702837/4d71f2e8-8783-42d3-bfa7-4a1059332c53)
</div>

<br>
<br>

Due to such results, multiple approaches were tried. Be it adjusting the learning rate or batch size, using ``BatchNormalization`` and ``Dropout``, and trying Oversampling and Undersampling datasets, all of these attempts yielded poor results.

<br>

In the end, using the ``keras_tuner`` library, the best parameters found were the 0.001 learning rate for 512 neurons. This result improved performance in the validation set, and using the ``selu`` activation function, later on, the results were, once gain, optimized.

<br>

Even so, the results were poor. The neural network, trained in a Oversampling Dataset, resulted in a __private score of 0.68998__, slightly better than the ``RandomForestClassifier`` of the second attempt. The same network, trained in a Undersampling Dataset, resulted in a __private score of 0.71407__

<br>

This conclusion likely shows that an ensemble approach with multiple classifiers, like the ``VotingClassifier`` is likely to be the best apporach.

<br>
<br>

<h3 id = "seventh-attempt">Seventh Attempt</h3>

Since the ensemble learners had the best performance so far, and the brief dive in Neural Networks didn't yield the same results (at the very least for now), I went back to the ``Scikit-Learn``library, this time to test the ``VotingClassifier`` with as much different estimators as possible, as well as trying the ``StackingClassifier``.
<br>

In the end, I created and submitted 9 ``VotingClassifier`` and 4 ``StackingClassifier``, to acess their performance. A few conclusions can be drawn.

<br>

1. The ``VotingClassifier`` showcased better performance.
2. Using estimators with optimized hyperparameters, the best ``VotingClassifier`` was the one with 7 classifiers.
3. The ensemble learners with the estimators NOT optimized stil yielded the best results.

<br>

In the end, I reached a new record for public and private scores, with the following models.

<br>

__New Public Record__
```
classificador = VotingClassifier(estimators = [
                                 ('Xgboost', xgb.XGBClassifier(random_state = SEED)),
                                 ('RandomForest', RandomForestClassifier(random_state = SEED)),
                                 ('Rede Neural', MLPClassifier(random_state = SEED, max_iter = 1000)),
                                 ('KNN', KNeighborsClassifier()),
                                 ('Regressão Logística', LogisticRegression(random_state = SEED, max_iter = 1000)),
                                 ('Máquina de Vetor', SVC(random_state = SEED, probability = True))
                               ], voting = "soft")
```

<br>

__New Private Record__
```
classificador = VotingClassifier(estimators=[
                                  ('Xgboost', xgb.XGBClassifier(random_state = SEED)),
                                  ('RandomForest', RandomForestClassifier(random_state = SEED)),
                                  ('Rede Neural', MLPClassifier(random_state = SEED, max_iter = 1000)),
                                  ('KNN', KNeighborsClassifier()),
                                  ('Regressão Logística', LogisticRegression(random_state = SEED, max_iter = 1000)),
                                ], voting='soft')
