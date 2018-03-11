
# KStock

**This is still in Alpha, please report all bugs!**


KStock is a Robinhood Day-Trading Bot written in Python >3.5. It uses the Robinhood API, as well as live data from NASDAQ.com to continuously monitor stocks and the users portfolio, determining when the most opportune time would be to sell/buy. I wanted to make this because I've seen countless queries about a stock trading bot but with no product. 

For those of you wondering, no. This does not and will not have any cryptocurrency or options integration. Robinhood has yet to update their API to handle their live crypto/option trading, but once it becomes available I'll work on the update.

![Live Tab](https://i.imgur.com/hlqzxp6.png) ![Background Tab](https://i.imgur.com/49ipYtt.png)

### Features

KStock features two Tabs: **Live**, **Backend**. 
* The Live tab features the current holdings of the user. It also features how much equity (both afterhours and intraday),as well as the Non-Margin Limit. This can be set to anything above $25,000. Why $25,000? The SEC forbids anyone from placing four or more day trades within a five day swinging period, without $25,000 in hand. Robinhood shrunk that limitation to three trades in five days, but it's basically the same thing. If someone breaks that rule, they are suspended from day-trading for 90 days. There are multiplie redundancies built in to help KStock not hit that limit, but if that limit is broken, KStock will automatically stop trading. 
* The Backend Tab features the Transaction and Queue tables. The transaction table shows all transactions that have happened. If a Tick is bought and sold, it will display the appropriate color for that row (Green=profitable trade, Red=non-profitable). The Queue Table holds the list of stocks that are currently waiting to be bought, assuming they meet the criteria. 

The following features are currently working:

* Monitor stocks that might be purchased, waiting for the most opportune time to purchase.
	* Multiple trading strategies are automatically triggered depending on the time of day. If the markets just opened (09:15-10:15) it will use the Price Swing Strategy. The rest of the day, it's looking for a smaller range price reversal by Short Trading.
* Closes out all positions at the end of the day, regardless of their prices
* Testing is capable using the global varialbe `TESTING`, it allows the user to play with the live data and the logic without making a real-world trade. If `TESTING=False` it will execute the commands and send the execution order to Robinhood to purchase/sell, make sure `TESTING` is set to how you want it to.

### Installation

KStock requires a few dependencies to get it up and running.
* PyQt5
* holidays
* pandas
* pyqtgraph
* h5py
* demjson
* bs4
* html5lib
* [Robinhood](https://github.com/Jamonek/Robinhood)

All of the requiremens are covered in `requirements.txt`

```sh
$ pip3 install -r requirements.txt
```

To run:
```sh
$ python KStock.py
```

When this is run, a test function is also run. This looks for a proper connection to Robinhood and NASDAQ:

A dialog will appear:

![Pass Window](https://i.imgur.com/8DQVi9d.png)

Both NASAQ and Robinhood both have a valid connection, so by closing this dialog, KStock will run.

If a proper connection couldn't be established, one or both tests will fail and a dialog like this will pop up:

![Fail Window](https://i.imgur.com/f8oaCNK.png)

Don't be alarmed if this occurs. Close out the window and try re-running KStock.

Now if this occurs multiple times in a row, there may be a bigger problem going on. NASDAQ has been known to change their website format so if that does occur, KStock will not run properly-hence this testing window.

On first initialization, a couple error windows will pop up. That's expected, seeing as how you haven't inputted your Robinhood data yet.
You'll then need to input your Robinhood info by going to Settings -> API. Assuming everything is correct, you'll be good to go.

KStock was built on Ubuntu 16.04 with Python 3.5.2 but technically this can be run cross-platform. 

### Development

Want to contribute?

Clone this repo and go for it. My commenting skills aren't the greatest but I tried to capture everything as best as I could.

There are a lot of people smarter than me in the world, so if you think you can improve on the trading logic, please contribute, I made the logic as more of a proof of concept than a permanent model. The trading logic is found in `Tick.py`. 

### Todos

 - MORE TESTING
 - Actually implement the stop-loss (the logic is there, just need to add the RH call)
  
License
----

GNU GPL v3.0


