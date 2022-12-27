# based on principles of:

* one buy and sell per day of a stock
    * buy will be placed before market open
    * an automatic sell will be placed if the stock goes up by the predicted 1 to 2% (don't get greedy....)
    * otherwise it will be sold at or close to market close to release funds for the next day
    * a stop loss will be placed of e.g. 2% to limit downside (this has the risk of missing upside if stock is volatile but is probably best in the long term)
* The prediction will be right more than it is wrong
    * only need to be right 51% of the time to make a long term profit, but aim for much higher than 51%!!!
* a relatively conservative upside being the target e.g. 1 to 2% - the higher I shoot for logically this suggests higher volatility and so a less reliable prediction (the model can be tested with different % of course....could cycle through multiple values to see which gives the best validation score before choosing a stock to trade)
    * basic experimentation with model training seems to confirm this is true - looking for 1% up side seems to be way easier to predict than 2% and so on
* keep prediction simplistic i.e. binary classification for a single stock - I'm not a high frequency trading house....
* compound 2% increase leads to a massive return over time


# assumptions

* stock market is not completely random and some datapoints can be used to predict with a better than 50% accuracy!
* can get a way of trading regularly with low or zero commission and no tax liability e.g. stocks & shares ISA
    * https://freetrade.io/stocks-and-shares-isa/stocks-and-shares-isa-charges - free trading, £10pm for stocks and shares ISA
    * IG  low commission - has access to more stocks
    * these need to be investigated
    * validate can place a stop loss on trades so any downside is minimal (should be able to, this is a basic feature)
* after a sell the funds are available for me to use to buy again the following day and not held until trade has been settled
* ~~can get enough stock data to train a model~~ - valid - some of Alpha Vantage is free again!!
* training the model will be simple enough with my limited ML skills - I'll use AutoGluon to make things as easy as possible
* since I'm only placing a trade once or twice a day then manual trades are OK - don't need to use API based trading


# TODO

* **top priority** initial model shows 93% accuracy but the data for the label is naturally imbalanced very heavily in favour of 0 (don't buy) - need to determine accuracy of 1 (buy) predictions
    * Accuracy is not the metric thing to be evalutating models on - since we have relatively lots of time and stocks available to work with, and we really care about buying something that will go up, we care about true positives primarily (i.e. how many times the model correctly predicted a buy) - therefore use **precision** as the evalutation metric - ths assumes Autogluon is treating 1 (buy) as positive - validate
* more features for dataset - there are a lot more indicators available from AlphaVantage - how many is too many - have to stay within API request rate limit and total per day for free account until I prove this works....
    * get positive and negative number for volume rather than just absolute number - can this be expressed as percentage of total stock as well?
    * more sophisticated ways of representing things like bollinger bands within the features?
* validate that data from Alpha Vantage is available quickly enough to do a prediction for the following day's market open (should be, it has intraday APIs as part of premium package)
* do some predictions based on existing model
* automate model training so that if a prediciton for a given stock is not a 'buy' it gets data for and trains a new model for a different symbol (which I can trade)
    * whole data gathering and training process is only about 15 mins with a decent CPU (and would be much faster if not using free Alpha vantage account) so if this is done overnight there is a decent time window for training models for multiple stocks
* validate which stocks are available from Alpha Vantage - e.g. is FTSE etc. data there - less restrictive to trade FTSE than e.g. NASDAQ from the UK
* identify broker to trade at (see assumptions) & open a trading account
    * determine which stocks can be traded via that broker and select data for only those symbols - feed into automation, see above


# build dataset from various things:

* ~~the previous or any day’s individual price probably has little predictive power since the its an arbitrary point in time~~
* ~~previous day analysis~~
    * ~~up or down~~
    * ~~RMA~~
* social sentiment - data seem difficult to get historically and probably doesn't exist anywhere far back enough?
* sector analysis - data is not available historically
* performance of other stock markets that have opened earlier than NASDAQ


datasources

* alpha vantage offers:
    * stock data - timeseries
    * news & sentiments
    * technical indicators - some are premium but various moving average indicators are available

