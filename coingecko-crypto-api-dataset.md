# Collecting Cryptocurrency Market Data with the CoinGecko API

Cryptocurrency markets generate enormous amounts of public data. Prices, trading volumes, supply levels, and market capitalizations are constantly changing and are openly available through financial APIs. Because of this, crypto markets provide a great environment to learn how to collect and curate datasets programmatically.

In this project, I built a dataset of cryptocurrency market statistics using the **CoinGecko API** and Python. The goal was to gather structured financial data that could be used for analysis, modeling, or visualization.

---

## Motivation

Many financial datasets used in research or investment analysis come from APIs rather than static files. Learning how to retrieve and clean data programmatically is therefore an important skill for anyone working in data science or quantitative finance.

Cryptocurrency markets are especially useful for this because:

- Market data is public
- APIs provide structured JSON responses
- The dataset updates constantly
- The data includes many useful financial variables

This project demonstrates how a simple Python script can collect and organize this data into a clean dataset.

---

## Question of Interest

How do price, market capitalization, trading volume, and short-term price changes vary across major cryptocurrencies?

The dataset created in this project provides a snapshot of the cryptocurrency market and could be used to explore questions such as:

- How concentrated is the crypto market among the largest coins?
- Do larger market cap coins experience smaller price swings?
- What relationship exists between trading volume and price volatility?

---

## Data Source

The dataset was collected using the **CoinGecko API**.

CoinGecko provides free access to cryptocurrency market data including prices, market capitalization, supply levels, and trading activity.

API documentation:  
https://www.coingecko.com/en/api/documentation

The script specifically uses the `/coins/markets` endpoint, which returns market information for hundreds of cryptocurrencies in a single request.

---

## Responsible API Usage

When collecting data from APIs it is important to avoid excessive requests or scraping practices that overload servers.

To follow responsible API usage:

- Only a small number of API requests were made.
- A short delay was added between requests.
- Only publicly available data was collected.
- No private API keys or credentials are stored in the repository.

This ensures the script respects the API provider while still allowing reproducible data collection.

---

## Data Collection Process

The dataset was created using Python with the `requests` and `pandas` libraries.

The process works as follows:

1. Send a request to the CoinGecko `/coins/markets` endpoint
2. Receive a JSON response containing market data for many cryptocurrencies
3. Convert the JSON response into a pandas DataFrame
4. Select relevant variables
5. Rename variables for clarity
6. Save the cleaned dataset as a CSV file

Because each API request returns up to 250 coins, two pages of results were retrieved to create a dataset with approximately 500 cryptocurrencies.

---

## Dataset Overview

The final dataset contains:

- **500 observations**
- **12 variables**

Each row represents a cryptocurrency and its market statistics at the time the data was collected.

### Variables

| Variable | Description |
|--------|-------------|
| coin_id | Coin identifier used by CoinGecko |
| ticker | Trading symbol |
| coin_name | Name of the cryptocurrency |
| price_usd | Current price in USD |
| market_cap_usd | Market capitalization in USD |
| market_cap_rank | Rank by market capitalization |
| volume_usd_24h | 24-hour trading volume |
| high_usd_24h | 24-hour price high |
| low_usd_24h | 24-hour price low |
| pct_change_24h | 24-hour percent price change |
| circulating_supply | Number of coins currently circulating |
| last_updated_utc | Timestamp of the last update |

---

## Data Cleaning

The raw API response contains many variables that are not needed for analysis. The dataset was cleaned by:

- selecting relevant columns
- renaming variables to more readable names
- converting timestamps to datetime format
- ensuring ticker symbols are uppercase
- removing duplicates if present

These steps produced a clean dataset suitable for analysis.

---

## Limitations

This dataset represents a **snapshot in time**. Cryptocurrency prices and volumes change continuously, so the dataset may look different if collected later.

Additionally:

- Smaller coins may be excluded depending on API filters
- Market values depend on CoinGecko’s aggregated data sources
- The dataset reflects only one moment in the market

Despite these limitations, the dataset provides a useful overview of cryptocurrency market structure.

---

## Code and Repository

The full code used to collect and clean the dataset is available here:

**GitHub Repository**  
https://github.com/iampyrotech/CryptoAPI

The repository contains:

- the Python script used to retrieve the data
- the cleaned dataset
- documentation explaining the project

---

## Conclusion

APIs are one of the most powerful tools for collecting real-world datasets. With only a few lines of code, it's possible to retrieve hundreds of observations and transform them into a structured dataset ready for analysis.

Projects like this show how data science workflows often begin with data aquisition, not just analysis.

Understanding how to retrieve, clean, and structure data is a foundational skill for statisticians, analysts, and quantitative researchers!
