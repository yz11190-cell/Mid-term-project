# Mid-term-project
Midterm Project: Quantitative Analysis of Financial Indicators in S&P 500 Constituent Equities
Authors: Kashish Hundia & John Zhang

This project conducts an exploratory data analysis on the fundamental financial metrics of the S&P 500 constituent companies. The goal was to build a comprehensive, self-sourced dataset by scraping company information from Wikipedia and fetching real-time financial data via the Yahoo Finance API.
We revealed and interpreted the relationships between key financial indicators, such as how leverage (debt usage) interacts with profitability, and how operating performance influences valuation.

Evaluated and compared the financial health, growth, and risk profiles of different sectors within the US market to identify potential investment trends.

Developed an investment filter with a strict criteria for equities that meet a high standard of financial health, value, and profitability.

Step 1: Web Scraping Constituents
We used the requests library to access the Wikipedia page and pandas.read_html() to parse the HTML table containing the S&P 500 list. The initial request was blocked, which was resolved by adding a User-Agent header to mimic a real web browser.

Step 2: Data Cleaning for API Compatibility
The stock symbols from Wikipedia sometimes contain a dot (e.g., BF.B), but the yfinance API uses a hyphen (e.g., BF-B). We created a new Cleaned_Symbol column by replacing all dots with hyphens to ensure compatibility.

Step 3: Fetching Financial Data
For each of the 503 cleaned ticker symbols, we used yfinance.Ticker().info to download a wide range of financial metrics including market capitalization, price-to-earnings ratio (PE), forward PE, PEG ratio, price-to-book ratio (P/B), etc.
We utilized a try-except block to handle missing data, and a time.sleep() function to avoid being limited by the API.

Step 4: Data Integration
The scraped financial data was converted into a structured DataFrame and then merged with the original Wikipedia dataset using the Cleaned_Symbol as the key. As a result, we ended up with a single final_df containing both company metadata and their corresponding financial indicators.

Step 5: Additional Metrics
We derived additional metrics not directly provided by the API. For instance, Volatility was calculated as (52_week_high - 52_week_low) / 52_week_low, and a simplified Bollinger Band Width was derived from this volatility measure.

Dataset Description

Rows: 503
Features: Over 30 features of mixed data types, covering:
Categorical: 
Company Security name, GICS Sector, GICS Sub-Industry, Sector and Industry.
Numerical:
Market Data: Price, 52_week_high/low, marketCap.
Valuation: PE (Trailing and Forward), P/B, price_to_sales, ev_to_ebitda.
Profitability: profit_margin, operating_margin, return_on_equity (ROE), return_on_assets (ROA).
Growth: revenue_growth, earnings_growth.
Debt & Risk: Debt_to_equity, Beta, current_ratio.
Dividends: DividendYield.
Derived Metrics: Volatility, Bollinger_Band_Width.

Analysis
The analysis was conducted in three progressive layers, moving from broad correlations to specific filters.

Correlation Analysis of Financial Indicators
We examined the correlation matrix of key numerical metrics to understand how different aspects of a company's performance and structure are related.

Key Findings & Interpretation:
Debt & Profitability: A moderate positive correlation was observed between Debt_to_equity and profit_margin/return_on_equity. This suggests that, within the S&P 500, many profitable companies strategically use debt (leverage) to amplify their returns on equity.
EBITDA & Marketcap: A strong positive correlation was observed between EBITDA and Marketcap. EBITDA is the ultimate measure of a firm's operating profitability since non-operational elements such as income taxes, depreciation & amortization are excluded. Market cap reflects the collective market judgment of that company's worth. In financial valuation, an equity is valuated based on how much cash it will generate in the future. In our case, higher profitability reflected by high EBITDA shows more cashflow in the future, and thus investors think it's worth more, and thus market cap is higher as a result.

Quantitative Investment Filter
To translate our analysis into an actionable strategy, we designed two strict multi-criteria filters to identify high-quality companies trading at reasonable valuations. The filter criteria were:

ROE > 0.08
Debt_to_equity < 2
0 < PE < 30
Profit_margin > 0.05

And 

ROE > 0.15 
PE<25
Debt_to_equity < 100

Results & Interpretation:
Applying this strict filter to the 503 S&P 500 constituents resulted in a shortlist of approximately 30 companies. This list represents a concentrated group of companies that are profitable, financially sound, reasonably priced, and still growing.
