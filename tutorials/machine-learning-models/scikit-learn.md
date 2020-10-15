---
description: >-
  Creating a simple linear regression model using scikit-learn and using dstack
  to pull and push the model.
---

# scikit-learn

We will use the **sklearn.datasets** package to use the diabetes dataset to make and deploy a **simple Linear Regression Model in just 2 minutes!**

#### 1. Importing 

We will first import _scikit-learn_, _numpy_ and _matplotlib_ for plotting and of course the `push_frame` and `pull` methods from _dstack_

```python
import matplotlib.pyplot as plt
import numpy as np
from sklearn import datasets, linear_model
from sklearn.metrics import mean_squared_error, r2_score
from sklearn.linear_model import LinearRegression
import sklearn

from dstack import push_frame, pull
```

#### 2. Loading and Splitting Dataset

Let's load our diabetes dataset now from scikit-learn, and split it.

```python
# Loading diabetes databse
diabetes_X, diabetes_y = datasets.load_diabetes(return_X_y=True)

# Use only one feature
diabetes_X = diabetes_X[:, np.newaxis, 2]

# Split the data into training/testing sets
diabetes_X_train = diabetes_X[:-20]
diabetes_X_test = diabetes_X[-20:]

# Split the targets into training/testing sets
diabetes_y_train = diabetes_y[:-20]
diabetes_y_test = diabetes_y[-20:]
```

#### 3. Fitting the Model

Finally we fit the model with the `LinearRegression()` object in scikit-learn and push the frame using dstack's `push_frame()`

```python
# Create linear regression object
regr = LinearRegression()

# Fitting the linear model
regr.fit(diabetes_X_train, diabetes_y_train)
```

#### 4. Pushing to dstack 

Now that our model is fit and ready, we push it to dstack as a stack using the `push_frame`

```python
# Push the frame
push_frame("simpleLinearReg", regr, "My first linear model")
```

