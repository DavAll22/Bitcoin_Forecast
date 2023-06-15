# Bitcoin_Forecast
Project comparing methods of handling time-series data to predict and forecast the price of Bitcoin. The aim is to understand how to handle time-series data and use it within models to predict values using windows and horizons, as well as build a model which replicates an algorithm used by a paper called N-BEATS using subclassing with residual stacking.

# This project uses:
* Tensorflow 2.X to create, train and evaluate the performance of models using time-series data [experimentation using windows and horizons]
* Sequential and Functional API to build the models, tf.data API to build and zip the datasets with training and testing data
* Creation of N-BEATS algorithm used for time-series forecasting using residual stacking (skip connections) and subclassing to build the block (https://arxiv.org/pdf/1905.10437.pdf)

The dataset used is the historical price of Bitcoin (https://coincodex.com/crypto/bitcoin/historical-data/) in USD. The values are recorded daily from 30th September 2013 to 12th June 2023, including the open, high, low and close price, as well as the market cap and volume. This project only focuses on the closing price of Bitcoin. The time-series data contains 3543 entries. 

For testing and evaluation of the models, a pseudo-future for forecasting was created and the dataset was split into 80% training and 20% testing, with the models forecasting on the test data. The dataset was split into windows and given a prediction horizon and evaluated using regression metrics. 2 versions of the dataset were created: Univariate (Using Bitcoin price to predict Bitcoin price) and Multivariate (using Bitcoin price and block reward size to predict Bitcoin price).

![image](https://github.com/DavAll22/Bitcoin_Forecast/assets/124359152/1ffb9fd7-1e28-444c-98f6-02f0e4623e96)

The models used and compared are:

0. Baseline - Naive forecast using the previous timestep to predict the next timestep
1. Simple Dense model - Window size = 7, Horizon size = 1 (past 7 days to predict next day)
2. Simple Dense model - Window size = 30, Horizon size = 1 (past 30 days to predict next day)
3. Simple Dense model - Window size = 30, Horizon size = 7 (past 30 days to predict next 7 days)
4. Conv1D - Window size = 7, Horizon size = 1
5. LSTM - Window size = 7, Horizon size = 1
6. Simple Dense model - Multivariate dataset
7. N-BEATS Algorithm - creation of class to replicate deep learning model (https://arxiv.org/pdf/1905.10437.pdf)
8. Ensemble - 15 randomly initialised Dense models with 3 different loss functions (5 models per loss function) aggregated into 1 prediction
9. Simple Dense model using the full dataset to forecast into the future (no pseudo-future dataset split)

# Evaluation
![image](https://github.com/DavAll22/Bitcoin_Forecast/assets/124359152/07c0dc9c-5f42-49f7-93fe-476eec9621cf)

It was found after the first 3 Dense models that a window size of 7 and a horizon size of 1 yielded in the best MAE score, so was used for the further experiments.
The naive prediction proves extremely hard to beat, with the next best models being the ensemble model and the multivatiate dataset model giving slightly higher mean absolute error values. 

However it was found that the ensemble model predictions lagged behind the actual data and indicated the model is overfitting by replicating what the naive model would do (predicting the previous time step value as the next value) and was seen in the 95% confidence preiction interval plot.

![image](https://github.com/DavAll22/Bitcoin_Forecast/assets/124359152/e9ccb974-48f8-4686-95f8-087ee616837c)


# Prediction
A simple Dense model like model_1 was created but utilised the entire time-series dataset without splitting into a pseudo-future to create a forecast.

![image](https://github.com/DavAll22/Bitcoin_Forecast/assets/124359152/020527e2-4471-4ffe-b914-5036d9262ab0)

The predictions seem cyclic up and down and could be a result of overfitting due to not generalising well for future data. Additional data to make the dataset m,ultivariate could help prevent this, was well as a deeper model with more layers with fine-tuning.
