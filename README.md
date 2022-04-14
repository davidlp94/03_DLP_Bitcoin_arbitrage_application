# Bitcoin_arbitrage_application
Welcome to my Bitcoin Arbitrage application using Python language, Pandas modules and JupyterLab that uses historical data on Bitcoin and arbitrage opportunities to calculate theoretical trade profits. This application can be used on any assset that exist on any pair of exchanges by replacing the (2) .csv files in the /Resources folder and changing variable names and dates within the code. In this application, we will examine the the price of Bitcoin in USD listed on the Bitstamp and Coinbase exchnage and calulate thoeretical arbitrage trade opportunities that have a minimum return of greater than 1% threshold.

Live application link: https://github.com/davidlp94/03_DLP_Bitcoin_arbitrage_application/blob/main/crypto_arbitrage.ipynb

---

## Techologies

The following technology/software was used for this application:


-Python 3.7.11 (Programming language)

    Imported Python Libraries/Packages:
    
    -Path from pathlib
    
    -csv
    
    -Pandas
    
    -%matplotlib inline
    
-JupyterLab

-Git version 2.34.11.windows.1

-Windows 10 Operating System

---

## Installation Guide

To install this application on your machine, download (or clone using http link) all files contained in this repository davidlp94/03_DLP_Bitcoin_arbitrage_application.

Next, open GitBash (terminal for MacOS), change your directory to the 03_Bitcoin_arbitrage_application, open JupyterLab in a dev environment and open/run the crypto_arbitrage.ipynb file.

---

## Usage

This Bitcoin_arbitrage_application contains the following files/directories:
```
.ipynb_checkpoints/ - This directory contains checkpoint/auto-save files.
Reources/ - This directory contains (2) .csv files for Bitstamp and Coinbase exchange data for Bitcoin.
crypto_arbitrage.ipynb - This files contains the main source code for this application and can be ran in JupyterLab.
README.md - A README file that contains an overview of this application.
license.txt
```

---

## Main Source Code
This application is broken down into three parts, Collecting the data, Preparing the Data and Analyzing the data. Our very first step is importing the neccessary modules/dependencies.
```
import pandas as pd
from pathlib import Path
import csv
%matplotlib inline
```
(Note: the following code snippets below are an overview of the main source code and does not contain all code associated with crypto_arbitrage.ipynb.)
### Collect the Data

Using the read_csv function and Path module, we will import the data from bitstamp.csv and coinbase.csv and create two seperate dataframes. Here, we are setting the index column to the Timestamp column that contains DateTime format.
```
bitstamp_df = pd.read_csv(
    Path('Resources/bitstamp.csv'),
    index_col='Timestamp',
    parse_dates=True,
    infer_datetime_format=True
)

coinbase_df = pd.read_csv(
    Path('Resources/coinbase.csv'),
    index_col='Timestamp',
    parse_dates=True,
    infer_datetime_format=True
)
```
### Prepare the Data
For the bitstamp and coinbase DataFrames, we will drop all NaN values.
```
bitstamp_df = bitstamp_df.dropna()
coinbase_df - coinbase_df.dropna()
```
Using the str.replace function, we will remove the dollar sign from the values in the 'Close' column and convert the data type to a float.
```
bitstamp_df.loc[:, 'Close'] = bitstamp_df.loc[:, 'Close'].str.replace('$', '')
bitstamp_df.loc[:, 'Close'] = bitstamp_df.loc[:, 'Close'].astype('float')
coinbase_df.loc[:, 'Close'] = coinbase_df.loc[:, 'Close'].str.replace('$', '')
coinbase_df.loc[:, 'Close'] = coinbase_df.loc[:, 'Close'].astype('float')
```

### Analyze the Data
We are now ready to analyze the data. In this portion of code, we will use the loc or iloc function to choose the following columns of data: Timestamp (index) and 'Close'.
```
bitstamp_sliced = bitstamp_df.loc[:, 'Close']
coinbase_sliced = coinbase_df.loc[:, 'Close']
```
Using the plot function with specific parameters, we can now plot the data for both the Bitstamp and Coinbase DataFrames.
```
bitstamp_sliced.plot(figsize=(10, 5), title='Bitstamp BTC Value vs. Time', color='green')
coinbase_sliced.plot(figsize=(10, 5), title='Coinbase BTC Value vs. Time', color='blue')
```
To plot an overlay of the two DataFrames, we insert a 'legend' parameter into our plot function. See visualization chart below.
```
bitstamp_sliced.plot(legend=True, figsize=(15, 7), title='Bitstamp vs. Coinbase BTC value', color='green', label='BTC_BS')
coinbase_sliced.plot(legend=True, figsize=(15, 7), color='blue', label='BTC_CB')
```
![Bitstamp vs. Coinbase BTC Value](https://user-images.githubusercontent.com/96163075/149984509-b5c987d4-4739-4ecf-a6b7-7e190c628ae0.png)

Next we will take a closer look at an earlier period of time in the data. in this case, the month of January 2018.
```
bitstamp_sliced.loc['2018-01-01' : '2018-02-01'].plot(legend=True, figsize=(15, 7), title='Bitstamp vs. Coinbase BTC Value - January 2018', color='green', label='BTC_BS')
coinbase_sliced.loc['2018-01-01' : '2018-02-01'].plot(legend=True, figsize=(15, 7), color='blue', label='BTC_CB')
```
![Bitstamp vs. Coinbase BTC Value - January 2018](https://user-images.githubusercontent.com/96163075/149984440-5adc08de-a621-4b3f-8a9f-91415682ac25.png)

Previewing the chart above, there are some arbitrage opportunities where the price of BTC is valued more on the Bitstamp exchange on January 28th, 2018. We can use the loc function to take a closer look at this date.
```
bitstamp_sliced.loc['2018-01-28'].plot(legend=True, figsize=(15, 7), title='Bitstamp vs. Coinbase BTC Value - January 28th, 2018', color='green', label='BTC_BS')
coinbase_sliced.loc['2018-01-28'].plot(legend=True, figsize=(15, 7), color='blue', label='BTC_CB')
```
![Bitstamp vs. Coinbase BTC Value - January 28th, 2018](https://user-images.githubusercontent.com/96163075/149984837-b11e633d-c1b2-424e-92e4-764aef9273fb.png)

Using the selected date of January 28th, 2018, we can calulate the arbitrage spread and plot the data.
```
arbitrage_spread_early = bitstamp_sliced.loc['2018-01-28'] - coinbase_sliced.loc['2018-01-28']
```
![Arbitrage Spread for 1-28-2018](https://user-images.githubusercontent.com/96163075/149985219-aa050989-9deb-4aa6-be05-d8c9b69ee601.png)

Next, we can measure the arbitrage spread by subtracting the lower-priced exchange from the higher-priced exchange and use a conditional statement to generate the summary statistics for the arbitrage spread DataFrame, where the spread is greater than zero.
```
arbitrage_spread_early = bitstamp_sliced.loc['2018-01-28'] - coinbase_sliced.loc['2018-01-28']
arbitrage_spread_early = arbitrage_spread_early.loc[arbitrage_spread_early['2018-01-28'] > 0]
arbitrage_spread_early.describe()
```
Next, to calculate the spread returns, we can divide the high-priced exchange DataFrame at instances where there is a positive arbitrage spread by the price of Bitcoin from the the lower-priced exchange.
```
spread_return_early = arbitrage_spread_early / coinbase_sliced.loc['2018-01-28']
```
To further narrow down our DataFrame, we can determine the number of times our trades with positive returns meet the minimum 1% trading fee threshold.
```
profitable_trades_early = spread_return_early[spread_return_early > .01]
```
Using a describe function, we can see that the average profitable trade is around 2.2% on this date with around 1,378 trade opportunities.
```
profitable_trades_early.describe()
```
![image](https://user-images.githubusercontent.com/96163075/149986267-3841051d-5f39-4cbd-9ea3-a729e58db3d5.png)

Next, we can calculate the potential profit, in dollars, per trade and drop any NaN values..
```
profit_early = profitable_trades_early * coinbase_sliced.loc['2018-01-28']
profit_per_trade_early = profit_early.dropna()
profit_per_trade_early.describe()
```
The below summary shows that the average trade can generate around $253.00. The information can also be plotted.
![image](https://user-images.githubusercontent.com/96163075/149987121-bb56bb11-e320-42a7-a15f-97a3d99c8d72.png)
![image](https://user-images.githubusercontent.com/96163075/149987237-8f1e86b4-311c-4eb4-900a-0e6d92a5563b.png)

Next, we can calculate the potential arbitrage profits that you can make on each day using the sum() function and can also plot the data using the cumsum() function.
```
profit_per_trade_early.sum()
cumulative_profit_early = profit_per_trade_early.cumsum()
```
![Cumulative Profit Per Arbitrage Trade on January 28th, 2018](https://user-images.githubusercontent.com/96163075/149987572-21cc5e2f-8bde-4b1b-ad90-72fe6bc57574.png)

### Final Analysis
Based on the analyzed data above, arbitrage opportunities started to fade after January 2018. This can also be seen on the 'Bitstamp vs. Coinbase BTC value' chart. However, January 28th, 2018 had great opportunities for arbitrage trading based on the 'Bitstamp vs. Coinbase BTC Value - January 28th, 2018' chart.

Also, the 'Profit Per Arbitrage Trade on January 28th, 2018' chart shows a certain day trading trend on this day. The most profitable trades occured between 6:00 UTC and 9:00 UTC while the least profitable trades started to occur at 19:00 UTC.

---

## Contributors

David Lee Ping

email: davidleeping@gmail.com

Phone: 570.269.5973

LinkedIn: https://www.linkedin.com/in/david-lee-ping/

---

## License

MIT License

Copyright (c) [2022] [David Lee Ping]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
