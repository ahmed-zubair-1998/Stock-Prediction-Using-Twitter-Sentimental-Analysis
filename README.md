# Stock-Prediction-Using-Twitter-Sentimental-Analysis

### Dataset:

The data set we used is StockNet dataset courtesy of Yumo Xu which can be obtained from [Github](1.	https://github.com/yumoxu/stocknet-dataset). The dataset includes two-year price movements from 01/01/2014 to 01/01/2016 of 88 stocks are selected to target, coming from all the 8 stocks in the Conglomerates sector and the top 10 stocks in capital size in each of the other 8 sectors. The full list of 88 stocks and their companies selected from 9 sectors is available in StockTable. It also contains tweet data of all these companies for two-year period separated by each day.

#### Preprocessing:

The stock data is present for working days and do not contain weekend price. We predicted this price by calculating average of previous and next present day. For n days we used the formula (x + y)/2 where x is previous present day and y is next present day for n days. We have also stored only closing price. Tweet data present is raw data generated from twitter. We have gathered tweet text, favorite count and retweet count. We have used TextBlob library to calculate sentiment of tweet. Favorite count and Retweet count are added together as weight. There are three types of sentiment - Positive, Negative, Neutral. We calculate the maximum sentiment value and assign sentiment to that day. We only used data of Apple for prediction. It contains sentiment value and closing price of stock for each day from 01/01/2014 to 31/03/2016.

### Methodology: 

We have used CNN for predicting stock prices.

#### Data Preparation:

Data is prepared by using a 30 day window and sliding it over the complete time period. For sentiment, neutral is assigned value of 0, positive 1 and negative -1. At start of each window a sentiment score is set to zero and value of each day is added to it. Score corresponding to each is added day is set as sentiment value of that day. The closing price of that day is also stored. Normal values of price and sentiment score is calculated and used. It is one instance of training data. The label for this instance is closing price of 31st day. Random 5% data is separated from training data as test data. 
In case of time series, our data is just 1D (the plot we usually see on the graph) and the role of channels play different values, close prices and tweet sentiment. You can also think about it from other point of view ,  on any time stamp our time series is represented not with a single value, but with a vector (price, sentiment). We don’t need to predict some exact value, so expected value and variance of the future isn’t very interesting for us ,  we just need to predict the movement up or down. That’s why we will risk and normalize our 30-days windows only by their mean and variance (z-score normalization) ((x−μ)/σ), supposing that just during single time window they don’t change much and not touching information from the future. But we are going to normalize every dimension of time window independently. But as we want to forecast movement of a price up or down next day, we need to consider the change of a single dimension. So, the data we will train on ,  are time windows of 30 days, but on every day we will consider whole price and sentiment data correctly normalized to predict the direction of close price movement.

#### Model:

For the purpose of prediction we have used CNN model. The architecture used has two 1d convolution layers with activation set to Leaky Relu and dropout set to 0.5. It is followed y a fuly connected layer with 64 neurons and activation set to Leaky Relu. Last layer is a Softmax layer. We have used Nadam as the optimizer with learning rate set to 0.002. Batch size of 128 is used for 50 epochs. 

### Results:

Maximum accuracy computed over the period of 50 epochs is 70%. 

