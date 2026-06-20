# Chapter 11: Machine Learning Libraries

- [Notes](#notes)
  - [Popular Machine Learning
    Libraries](#popular-machine-learning-libraries)
  - [How Machine Learning Works](#how-machine-learning-works)
    - [Transformations](#transformations)
    - [Splitting Training and Test
      Data](#splitting-training-and-test-data)
    - [Training and Testing](#training-and-testing)
  - [Learning More About
    Scikit-Learn](#learning-more-about-scikit-learn)
- [Summary](#summary)
- [Questions](#questions)

## Notes

- *Machine Learning* contrasts with traditional programming
  - Traditionally one defines the process to convert an input into a
    solution
  - Machine learning uses data to make a program “discover” how to find
    the solution
- There a number of different machine learning libraries in Python
  - Various different levels of popularity
- There a range of different machine learning models and techniques
  - Different models work well for different types of problems
  - Two common types are those that predict future values, and
    classifiers that categorise data

### Popular Machine Learning Libraries

- Four commonly used Python open-source libraries are

  1. [**Pytorch**](https://docs.pytorch.org/docs/2.12/index.html)
      - Developed by Meta based on the Torch Library
      - Probably the most widely-used library, especially in production
      - Designed to utilise GPU programming for improved performance
  2. [**Tensorflow**](https://www.tensorflow.org/guide)
      - Google’s internal-produced machine learning library
  3. **Keras**
      - Open-source library developed on top of tensorflow
      - Now included as part of Tensorflow directly
      - Aims to provide a simpler high-level API for ML beginners and
        researchers
  4. [**Scikit-Learn**](https://scikit-learn.org/stable/index.html)
      - Basic machine-learning library as part of the Scipy ecosystem
      - Good for beginners

### How Machine Learning Works

- Machine learning is either

  1. Supervised
      - Known data is used to train and test a model
      - Typical pipeline is,
        1. Transform
        2. Separate test and training data
        3. Train the model
        4. Test the model
  2. Unsupervised Learning
      - Data is fed in to discover insights without a pre-existing
        expected output

- We’ll look at how to implement a supervised-learning pipeline with
  `scikit-learn`

#### Transformations

- Many algorithms can be particular about the expectations of their
  input data
  - Often means we have to transform data to fit the algorithm we want
    to use
  - For example, we might have discretize a continuous variable e.g. age
    in age ranges
- We call the components that perform these data transformations
  *transformer*
  - There are many different types that target different parts of the ML
    pipeline

  - Some examples are,

    1. Cleaning
        - Used to tidy up a dataset into something that can be used
    2. Feature Extraction
        - Used for identifying import aspects or patterns in a dataset
    3. Reduction
    4. Expansion
- In Scikit-learn these are typically implemented as classes
  - Use a `fit` method to return a transformation matched to the data
  - `transform` method on this transformation then applies the
    transformation
- For example, to scale a dataset to a fixed range (typically between
  $0$ and $1$) we could use a `MinMaxScalar`

``` python
import numpy as np
from sklearn.preprocessing import MinMaxScaler

data = np.array(
    [[100, 34, 4], [90, 2, 0], [78, -12, 16], [23, 45, 4]]
)  # Array with data from -12 to 100

minMax = MinMaxScaler()  # create a transformer object
scalar = minMax.fit(data)  # Fit the transformer to the data

scalar.transform(data)  # Apply the transformation
```

    array([[1.        , 0.80701754, 0.25      ],
           [0.87012987, 0.24561404, 0.        ],
           [0.71428571, 0.        , 1.        ],
           [0.        , 1.        , 0.25      ]])

- Sometimes you may want to split data *before* using a transformation
  - Here the transformer won’t be fitted to the test data
  - Hence why `fit` and `transform` are different steps
    - We want to `fit` only to the training data
    - But want to `transform` both the training and the test data

#### Splitting Training and Test Data

- Fitted models can suffer from two conditions

  1. Under-fit
      - The model doesn’t have enough complexity to capture the true
        dynamics of supplied data
  2. Over-fit
      - The model has too much flexibility and becomes tightly fit to
        the training data, failing to generalise

- To ensure we can catch over-fitting we need to avoid training the
  model on test data

  - Otherwise the model might have just trained to match our test set
  - Employing it on real data then results in skewed results

- Scikit-learn provides in-built support for spitting data sets into
  test and train components

- For example, let’s consider working with the in-built `iris` dataset

  - We’ve already seen this dataset before in [chapter
    10](../Chapter_10/Chapter_10.qmd)
  - The dataset contains $150$ samples, each of $4$ characteristics
    - The goal is to predict the $150$ `iris` types

- The first step after loading the data set is to split the data into
  training and test sets via the `train_test_split` function

``` python
from sklearn import datasets  # load sample datasets
from sklearn.model_selection import train_test_split

source, target = datasets.load_iris(return_X_y=True)  # load source and targets

# source data
print(type(source))
print(source.shape)

# target data
print(type(target))
print(target.shape)

train_s, test_s, train_t, test_t = train_test_split(source, target)

print("train_s shape:", train_s.shape)
print("train_t shape:", train_t.shape)
print("test_s shape:", test_s.shape)
print("test_t shape:", test_t.shape)
```

    <class 'numpy.ndarray'>
    (150, 4)
    <class 'numpy.ndarray'>
    (150,)
    train_s shape: (112, 4)
    train_t shape: (112,)
    test_s shape: (38, 4)
    test_t shape: (38,)

- `train_test_split`, splits the `source` and `target` arrays into
  corresponding train and test subsets
  - Should see that the ratio of train to test is about 75:25 by default
  - The split is random to remove potential bias in how we split the
    sets

#### Training and Testing

- Machine Learning algorithms in Scikit-learn are represented as classes
  called *estimators*
- Estimators are typically tunable via various parameters at
  instantiation time
- Like with transforms they have a `fit` method
  - This trains the model

  - `fit` typically accepts two parameters

    1. `samples`
        - The training data
    2. results or targets
        - The expected outcome for each input in the training data set

  - Both should be [Numpy](../Chapter_07/Chapter_07.qmd) arrays
- Once trained the `predict` method can be used to use the model to
  predict results on a given dataset
- Returning to our `iris` example, we can use a `KNeighboursClassifier`
  estimator
  - K-nearest groups samples based on distance between characteristics
    - Predictions are made by comparing a new sample to close
      neighbouring samples
    - The K in *K-nearest* comes from the tunable parameter in this
      model, the number of neighbours considered
- Below shows the whole pipeline
  - Play around with the number of `n_neighbours` in the associated
    notebook version and examine the impact on the accuracy

``` python
from sklearn import datasets, metrics
from sklearn.model_selection import train_test_split
from sklearn.neighbors import KNeighborsClassifier


source, target = datasets.load_iris(return_X_y=True)  # load source and targets

train_s, test_s, train_t, test_t = train_test_split(
    source, target
)  # split into train and test sets

knn = KNeighborsClassifier(n_neighbors=3)  # Create a 3-neighbour classifier

knn.fit(train_s, train_t)  # Train the model on the training data
test_prediction = knn.predict(test_s)  # Test on the test set

# Assess the results
metrics.accuracy_score(test_t, test_prediction) # Accuracy against the test data
```

    0.9473684210526315

### Learning More About Scikit-Learn

- The above demonstrates a simple ML pipeline

- Scikit-learn contains more important features for designing these
  pipelines, e.g.

  - Cross-validation tools
    - Split a dataset multiple times
    - Reduces over-fit on test data
  - Pipelines
    - Wrap transformers, estimators and cross-validation into one
      process

- The best place to start is by reading through the
  [docs](https://scikit-learn.org/stable/user_guide.html)

## Summary

- Python has a large ML ecosystem including many major libraries
  - PyTorch and Tensorflow are the two common frameworks for large-scale
    production ML
  - Scikit-Learn and Keras provide more beginner-friendly APIs for
    getting started with machine learning

## Questions

1. In which step of training a supervised estimator would a
    Scikit-learn transformer be useful?

    - A transformer is usually useful *after* splitting a set into train
      and test data and before fitting the estimator
    - The transformer ensures the the input training data fits the
      expectations of the estimator
    - By doing so *after* the split, we ensure that the fit for the
      transformation depends only on the training data rather than
      including the test data
      - Reduces the potential for over-fit

2. Why is important to separate training and test data in machine
    learning?

    - Machine learning models will fit to any data that they are
      provided
    - If given the test data this will include the test data
    - If a model has sufficient flexibility it can “memorise” the test
      data while failing to adequately generalise to new inputs
      - This is called over-fit
    - Splitting the training and test data doesn’t remove the risk of
      over-fit to the training data, but *does* let us characterise the
      accuracy on new dataset

3. After you have transformed your data and trained your model, what
    should you do next?

    - You should run the model against the test set and generate metrics
      characterising the accuracy and precision of the results
    - Potentially re-run, fine-tuning the parameters of the estimator to
      see what provides the best accuracy
    - In theory, this should be extended with cross-validation to
      protect against over-fit
