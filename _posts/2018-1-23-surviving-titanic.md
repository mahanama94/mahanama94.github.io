---
layout: post
title: Surviving Titanic
tags: [Data Science, Machine Learning, Kaggle]
---

Titanic is one of the most remarkable shipwreck in the history as ship was on it's maiden voyage.
The ship sank after it collided with an ice berg and around 1500 people were killed as a result of the tragedy.
Here we will have a look at predicting survivors of the tragedy with the help of some basic machine learning.

![Spoiler Alert]({{ site.baseurl }}/images/surviving-titanic/spoiler-alert.jpeg "Spoiler Alert")

## Getting Ready

We will be using `python` with `scikit` and `pandas` for the predicting the survivors of Titanic.
You can easily install the prerequisites easily using following commands on a debian based OS. (Im using Ubuntu ;) )
Installation methods might vary with the operating system, for others it is safe to head to the official docs and
follow the instructions for setting the environment.

``` bash
# Install pip
$ apt install python3-pip   # Python 3
$ apt install python-pip    # Python 2

# Install scikit
$ pip install scikit-learn

# Install pandas
$ pip install pandas
```
We also will be using the dataset provided which you can obtain once you have created a kaggle account (Highly recommended)
Download the dataset in to your local file system. The downloads(data tab) section has 3 main files as
`gender_submissions.csv`, `test.csv`, `train.csv`.
* Gender Submissions    - Sample Submission assuming only females survived. (You submission file should look like this)
* Test                  - Dataset used to test your model. (You have to predict each person in here will survive or not)
* Train                 - Known information you have to use for training your model. (Toys ;) )

[Kaggle Titanic Competition](https://www.kaggle.com/c/titanic)

## Getting Hands Dirty

For the purpose of the blog, we will be focusing mainly on getting on working machine learning code. So, straight to coding.

### Reading and pre-processing data
`pandas` is a python library that provides easy data manipulation and analysis. Here we will be using `pandas` to
* Read data             - Read data from csv file
* Transforming data     - Transform non-numeric fields to numeric
* Fix missing data      - Add values to missing fields
* Extract required data - Extract features that we will be using the model

Your code will look like the following code.

``` python

import pandas as pd

df = pd.read_csv("train.csv")

## We have to provide numerical values for the Sex and Port of Embark
df = df.replace({"Sex": {"male": 1, "female": 0}})
df = df.replace({"Embarked": {"C": 1, "Q": 2, "S": 3}})
## Fill the missing data
df = df.fillna(-99999)

## Extract data for training the  model
X = df[["Age", "SibSp", "Parch", "Sex", "Embarked", "Cabin"]]
y = df["Survived"]

````

In here we have used simple transformations for data relating to Sex and Port of Embark. You can always be creative
and come up with new data transformations to add more features for your model. We will also use only 6 features related with
each passenger.

### Getting the model

As the model, we will be using K Nearest algorithm to predict the survival of a person. The K Nearest Classifier, classifies the
input depending on the closest K number of neighbors to the point. For instance if 5 neighbors of the data point of
the person **X** has 3 survivors and 2 deaths, then **X** will be classified as a survivor. Before training the model,
we will have to  split the data in to training and testing from the Training set provided from the kaggle. (For final submission you can use the entire dataset for training)

``` python

from sklearn import cross_validation

X_train, X_test, y_train, y_test = cross_validation.train_test_split(X, y, test_size=0.2)

```

Having different sets for training and testing are important as if tested with the same data set, the predictions would be
similar to you referring to the memory. So, it is always important to have different sets for training and testing.

Then we will be training the K Neighbors classifier with training set and calculating the accuracy for predictions
from the model with the help of the testing data set.

``` python

from sklearn import neighbors

clf = neighbors.KNeighborsClassifier()
clf.fit(X_train, y_train)
accuracy = clf.score(X_test, y_test)

```

The convenience of `sklearn` library is that you can use the same code for another machine learning model. For instance we can use
another classifier with a simple change in the code as below.

``` python
from sklearn.neural_network import MLPClassifier
from sklearn import svm

clf = MLPClassifier()
# OR
clf = svm.SVC()

```

### Predicting a survivor

The trained model can be used to predict survival of a person with a given set of parameters as follows,

``` python

clf.predict([[person_information]])

```
With some tweaks to the code segments, you can easily build a simple model that predicts the survival of a
person on Titanic and of course, you can make a submission on [Kaggle](https://www.kaggle.com) too.

## Closing thoughts

Here we have used a simple set of data with no or less transformation and as per tests, the model has an
accuracy of around 70%. We can improve the accuracy by considering other data that we have ignored during the process.
For instance the cabin number (or the letter) can provide a hint on the survival. (Think like people in the cabins of front part
    of the ship might have a higher change of survival compared to others)
On the other-hand, the predictions might lose accuracy as a result of those data. Plus you also can use the name of the person
for your predictions, cause we know Jack died and Rose survived. ;)

### Hire me

I currently work as a freelancer for projects in mobile applications, web applications, desktop applications, data mining
and machine learning. I provide my services mainly using Ionic framework, PHP, NodeJs, Java, electronJs, Erlang and Python.
You can directly contact me for your projects or can visit my Fiverr profile. Please contact me before placing an order.

[mahanama94 Fiverr](https://www.fiverr.com/mahanama94/)

Cheers !!!

**mahanama94**
