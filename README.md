# Bitcoin_arbitrage_application
Welcome to my Bitcoin Arbitrage application using Python language, Pandas modules and JupyterLab that uses historical data on Bitcoin and arbitrage opportunities to calculate theoretical trade profits. This application can be used on any assset that exist on any pair of exchanges by replacing the (2) .csv files in the /Resources folder and changing variable names and dates within the code. In this application, we will examine the the price of Bitcoin in USD listed on the Bitstamp and Coinbase exchnage and calulate thoeretical arbitrage trade opportunities that have a minimum return of greater than 1% threshold.

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

Using the read_csv function and Path module, we will import the data from bitstamp.csv and coinbase.csv and create two seperate dataframes.
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




