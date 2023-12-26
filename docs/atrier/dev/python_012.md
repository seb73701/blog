# Train multiple Time Series Forecasting Models in one line of Python Code

![](https://miro.medium.com/max/1400/1*nAtoDsVR7ZZo-6YZQUAHhw.png)
*Image by Mediamodifier from Pixabay*

Automated Machine Learning (AutoML) refers to automating some of the components of the machine learning pipeline. AutoML speeds up the workflow of a data scientist by automating some of the model development processes. Automation allows the non-experts to train a basic machine learning model without being much knowledgeable in the field.
There are [various Auto-ML open-sourced Python libraries](https://medium.com/swlh/8-automl-libraries-to-automate-machine-learning-pipeline-3da0af08f636) including TPOT, MLBox, Auto-Sklearn, etc that can automate a classification or regression machine learning model. To automate an NLP problem, one can use the [AutoNLP library](https://medium.com/swlh/autonlp-sentiment-analysis-in-5-lines-of-python-code-7b2cd2c1e8ab).
In this article, we will discuss how to automate a time-series forecasting model implementation using an open-source model Auto-TS.

## What is Auto-TS:

![](https://miro.medium.com/max/2400/1*sqdTGFex4Lsew4a8WbtLKA.png)
*(Image by Author)*

Auto-TS is an open-source Python library with time series forecasting implementation. It can train multiple time series forecasting models including ARIMA, SARIMAX, FB Prophet, VAR, etc, in just one line of Python code, and then choose the best one out of it for predictions.
Some of the features of Auto-TS libraries are:
- Finds the optimal time series forecasting model using genetic programming optimization.
- Trains naive, statistical, machine learning, and deep learning models, with all possible hyperparameter configurations, and cross-validation.
- Performs data transformations to handle messy data by learning optimal NaN imputation and outlier removal.
- Choice of the combination of metrics for model selection.

## Installation:
Auto-TS can be installed using PyPl using the command:

```python
pip install autots
```

Import the library using:
```python
from auto_ts import auto_timeseries
```

## Usage:
> The dataset used in this case study is [Amazon Stock Price](https://www.kaggle.com/szrlee/stock-time-series-20050101-to-20171231?select=AMZN_2006-01-01_to_2018-01-01.csv) from Jan 2006 to December 2017, downloaded from [Kaggle](https://www.kaggle.com/szrlee/stock-time-series-20050101-to-20171231?select=AMZN_2006-01-01_to_2018-01-01.csv).

This library comes up with only train time series forecasting models. The dataset should have a 
time or data format column.
- Load the time series dataset with a time/date column
```python
df = pd.read_csv("AMZN.csv", usecols=['Date', 'Close'])
df['Date'] = pd.to_datetime(df['Date'])
df = df.sort_values('Date')
```

- Split the entire data into train and test data
```python
train_df = df.iloc[:2800]
test_df = df.iloc[2800:]
```

- Visualize the train test values for reference
```python
train_df.Close.plot(figsize=(15,8), title= 'AMZN Stock Price', fontsize=14, label='Train')
test_df.Close.plot(figsize=(15,8), title= 'AMZN Stock Price', fontsize=14, label='Test')
plt.legend()
plt.grid()
plt.show()
```
![](https://miro.medium.com/max/2400/1*2v1ia-wwQRAW_9HhYi3sqg.png)
*(Image by Author), Train Test Split, AMZN stock price visualization*

- Initialize the Auto-TS model object, and fit the training data.

```python
model = auto_timeseries(forecast_period=219, score_type='rmse', time_interval='D', model_type='best')
model.fit(traindata= train_df, ts_column="Date", target="Close")
```

- Compare the scores for different models
```python
model.get_leaderboard()
model.plot_cv_scores()
```
![](https://miro.medium.com/max/564/1*k62BASD1mxkD2wMHgTdiJw.png)
*(Image by Author), RMSE score for different models*

- Predict the test data

```python
future_predictions = model.predict(testdata=219)
```

- Visualize the test data values and predictions.

![](https://miro.medium.com/max/2400/1*7KnRXjtqD2Oi7A0Kojv2Uw.png)
*(Image by Author), Test vs Predictions, AMZN stock price visualization*

## Conclusion:
In this article, we have discussed how to implement time series modeling in one line of Python code. Auto-TS does the preprocessing of data, as it removes the outliers from the data and handles messy data by learning optimal NaN imputation. Using just a single line of code, by initializing the Auto-TS object and fitting the train data, you can train multiple time series models including ARIMA, SARIMAX, FB Prophet, VAR, and come up with the best performing model.

The result of the model does seem satisfactory, but if we try to increase the size of the dataset, the results may improve.

Apart from the Auto-TS library that trains models for times series dataset, we have various AutoML libraries, that can speed up the workflow of the data scientist, by automating the model development pipeline. Read the [below-mentioned article](https://medium.com/swlh/8-automl-libraries-to-automate-machine-learning-pipeline-3da0af08f636), to know 8 AutoML libraries.


## References:
[1] Auto-TS Documentation: https://pypi.org/project/AutoTS/