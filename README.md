# MLPClassifier
In this notebook, we will use sklearn's MLPClassifier to create a trading strategy using neural networks.

### 1. Import the data
We import the data of Boeing Co (ticker: BA) from the csv file.

### 2. Define predictors and target variables
We define a list of predictors as listed below

1. ret1 is the one-day returns,
2. ret5 is the five-day returns,
3. std5 is the five-day standard deviation,
4. volume_by_adv20 is the volume divided by the 20-days average of daily volume and
5. price differences (High - Low) and (Open - Close).

We define the target variable which is the future one-day returns.

We drop the NaN values and store the predictor variables in X and target variable in y. The target variable is:
- 1 if the one-day future returns are positive and
- 0 if the returns are negative or zero.

The future returns for the last day in our dataset will not be calculated (since data for the next day is not known). Thus, the corresponding values will also be dropped when we use the dropna function. This can be overlooked as we will only be using the dataset to 'train' our model.

The trained model will be used on the new input data (appropriately transformed as the X input), and by using the predict method we can obtain the model predicted value.

### 3. Split the data into train and test dataset
This step is required to verify if the neural network we will create is any good. We will split the dataset into two parts, first 80% of the dataset will be used to create the model and the remaining 20% will be used as a test dataset.

Often, there is a regime change in time series data, so that a model trained on data randomly selected through the dataset will outperform a model trained on data selected from an earlier part of the data, but the latter will be more realistic. Also, if we are forced to select from an earlier part of the data, there is no way to ensure that the statistics of the train and test set are the same (i.e. equal % of classes in each), but that again is realistic. In trading applications, we normally don't have truly stationary statistics, and a ML strategy needs to be robust enough to withstand that.

### 4. Scale the data
In this step, we standardize the features stored in X using sklearn's StandardScaler function. The machine learning model might behave in unexpected fashion if the individual predictors are not standardized.

The StandardScaler function standardizes the predictors by removing the mean and scaling to unit variance.

### 5. Create a neural network using the train data
The MLPClassifier function from the sklearn.neural_network package is used to create a neural network model. The MLPClassifier takes following as input parameter

1. activation: The activation function such as logistic regression can be used to define the output from the neurons.
2. hidden_layer_sizes: A tuple to specify the number of hidden layers and number of neurons in each of the hidden layers. For example, the tuple (2,5) indicates two hidden layers with 2 neurons in the first layer and 5 neurons in the second layer.
3. random_state: It is used to initialize the weights and bias.
4. solver: A function such as stochastic gradient descent is used to optimize the weights during backpropagation. 'sgd' is specified to indicate 'stochastic gradient descent'.

#### 5.1 Training the model
The fit function is used to train the model using the train dataset which is stored in X_train and y_train. The parameters to the fit function are the predictor variables and target variable.

#### 5.2 Predict using the neural network
We define a trading rule based on the predicted value from the neural network. If the predicted value is positive then we buy (+1) the stock and otherwise, we don't buy the stock. This signal is stored in predicted_signal. The strategy returns are generated by multiplying the future one-day returns by the predicted signal and stored in the strategy_returns_nn.

### 6. Performance of the neural network

#### 6.1 Sharpe Ratio
Sharpe ratio represents how good the strategy performance is for the risk (standard deviation) taken to achieve it. The higher the Sharpe ratio the better is the strategy. Generally, a Sharpe ratio of greater than 1.5 is preferred.

We calculate the Sharpe ratio for the strategy in train and test dataset. The risk-free rate is assumed to be 5% p.a.

#### 6.2 Strategy CAGR
CAGR represents the compounded annual returns of the strategy.

We compute the strategy CAGR in train and test dataset.

#### 6.3 Plot cumulative returns in train dataset

#### 6.4 Plot cumulative returns in test dataset

#### 6.5 Strategy Results

#### 6.6 Accuracy of the neural network classifier
The classification_report function from sklearn.metrics package is used to analyze the performance of the neural network. The classification_report takes as input the actual output and the predicted output.

The output of classification report: 
1. Precision: tp / (tp + fp)
where tp is the number of true positives and fp the number of false positives. The precision is intuitively the ability of the classifier not to label as positive a sample that is negative.
2. Recall: tp / (tp + fn)
where tp is the number of true positives and fn the number of false negatives. The recall is intuitively the ability of the classifier to find all the positive samples.
3. F1-score: It can be interpreted as a weighted harmonic mean of the precision and recall, The F1-score reaches its best value at 1 and worst score at 0.
4. Support: It is the number of occurrences of each class in y_true.
