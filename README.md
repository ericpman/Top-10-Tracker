# Top-10-Tracker
Tracks the performance of our Top 10 Basket of stocks across multiple trading strategies, namely Market Orders, Fair Value Orders, Beta Adjusted Orders, and ATR Adjusted Orders

Our goal is to track our top 10 longs and top 10 shorts stock picks. We want to see the total performance of our basket of stocks in aggregate (longs and shorts). It also shows us the performance of each basket (longs baskets and shorts baskets separately). Included is a capital allocation cell which allows us to adjust how much money we are allocating per trading symbol in our calculation. The different strategies for tracking our baskets are as follows: 

Market Orders. This strategy is the simplest. It involves all market orders at the opening price for the current trading day for all our longs and our shorts. Capital is even and based solely on how much we set to allocate.

Beta Adjusted: this strategy enters via market orders, but the amount of capital allocated per symbol is based on the stock's current beta. Beta is a measure of volatility relative to the market.

ATR Adjusted: This strategy is the same as beta, but it uses ATR (Average True Range) to measure volatility rather than beta. 

Fair Value (FV) OPG Limit Orders: This strategy uses limit OPG orders to enter into each trade. In this strategy, not every order gets filled, depending on the opening price of the stock. The price of our limit orders is based on a calculation related to the price of the SP500 index (SPY ETF) shortly before market open (9:15am EST). 

FV OPG Limit + Beta: The same as FV OPG Limit Orders, but it allocates capital based on the stock's beta just as in our beta adjusted sheet.

Let us take a more detailed look at our Sheets to see how these symbols are determined, and how they make their way into our tracking sheet.

The first step can be found in the sheet tab titled 'Copy/Paste' Here is a screenshot of what this sheet looks like:

![Screenshot 2024-11-07 103411](https://github.com/user-attachments/assets/7a069e36-c82b-4573-b72f-a642cc50810e)

In this sheet we have column A which contains our 10 stock symbols long (in green) and 10 stock symbols short (in red). Column B and C contain the company and sector respectively. Column H contains the stock's beta and Column I contain the stocks ATR. Within this sheet in columns K:O, there is an automatic calculation that is done to determine the capital allocation for our ATR adjusted strategy. The last closing price of the stock is listed in column K via our google finance formula =googlefinance(A2,"closeyest"). In Column L, the ATR expressed as a percentage of the price is then calculated by taking the ATR in column I and dividing by the price in column K. In column M, we calculate the capital allocation using the ATR as a % of price. This is found by taking the maximum ATR as a percentage of price in column L, and setting the corresponding capital in that row to the value we set in cell O1. For the rest of the cells, their ATR adjusted capital is found by returning a proportion of the value in cell O1 scaled by the ratio of the maximum value in column L to the value in column L for that corresponding row. A sample row utilizing this formula is =if(L2=max(L$2:L),O$1,(max(L$2:L)*O$1)/L2). Using this formula, we can find our ATR adjusted capital and list these values in column M. (Note in row 22, we are finding the ATR adj Capital for SPY based on the ATR of SPY listed in cell J23. 

Now that our data is inputted in or 'Copy/Paste' Sheet tab, this data is retrieved by our other sheets to effectively track the performance of our stocks. Let us start with our Market Orders OC sheets tab. Here is an image demonstrating this sheet:

![Screenshot 2024-11-07 114652](https://github.com/user-attachments/assets/782952bb-f070-47ac-8bfa-77d991bc7016)


This sheet contains a method of placement as mentioned earlier that provides the simplest form of order entry: Market Orders. This strategy dictates that you are to simply buy a stock long or sell a stock short at the open, no matter the opening price of the stock. An walkthrough of this sheet will also be beneficial to our foundational knowledge to the rest of the sheets, since many characteristics are found between this sheet and the other sheet tabs in our Google Sheet. 

Focusing on our performance tracking section in the top left of the sheet, we see key performance metrics for our observation. In cell B3 and C3 we have our Return on Capital (ROC) and Profil & Loss (PL). In cells B5 and C5 we have our ROC and PL for our longs, and in B6 and C6 our ROC and PL for shorts. In E3 we have our total capital deployed, and in F3 we have the number of symbols deployed. We also have our capital and # of symbols break down by longs and shorts in E5:E6 and F5:F6. Not that in Row 7 we have metrics for our hedge, which is currently empty. However if we find ourselves net long or net short, we can opt to add a hedge (such as the SPY) and its performance metrics can be found here.

Cell I2 contains our value for how much capital we are allocating per symbol.

In Rows 10:21 we have detailed tracking of our individual symbols. Contained within these rows, Column C contains our stock symbols we are trading long. These symbols are retriever from our 'Copy/Paste' tabs via our formula ={"Symbols";'Copy/Paste'!A2:A11}. Column D has the opening stock price (=GOOGLEFINANCE(C12,"priceopen" as our example for row 12). E hs the current stock price =GOOGLEFINANCE(C12,"price") as our example for row 12. F has the current difference between the current price and the open. G has the total capital allocated for the given symbol. H has the number of shares per symbol. I has the PL per symbol. J has the ROC per symbol. The same process and information is contained for our shorts in Columns N:U.

Additional metrics for our longs can be found in G10, I10, and J10 (capital deployed, PL, ROC) and for our shorts in cells R10, T10, and U10. 


In cell D26 we have the number of hedge units, calculated by taking the number of short symbols - long symbols. This tells us if we need to go long or short or hedge in order to balance our capital allocation. 

In D25:L28 we have our hedge inputs. In E28 we have the symbol for our hedge, in this case SPY. F28 contains the opening price, G28 current price, H28 Difference, I28 Capital deployed, J28 shares bought or sold, K28 is our PL, and L28 is our ROC. 

Our next sheet is our Beta Adjusted sheet: 

![Screenshot 2024-11-07 121156](https://github.com/user-attachments/assets/aa01906e-9486-473a-8e28-1a1459f92ba7)

The key differences in this sheet can be found Columns G11:I21 for longs and T11:V21 for shorts. Within these columns are the Beta, Beta Adj. Capital, and Actual Capital. The beta is obtained from our 'Copy/Paste' Sheet via our formula ={"Beta";'Copy/Paste'!H2:H11}. Using our long side as an example, The beta adjusted capital is obtained by dividing our capital allocating in cell K2 by the stocks corrresponding Beta in column G via our formula ={"Beta Adj. Capital";arrayformula(if(isnumber(G12:G21),DIVIDE(K2,G12:G21),""))}. From here, we use our beta adjusted capital to calculate the number of shares we will be purchasing. by dividing the beta adjusted capital by the opening price. Our actual capital is then calculated by multiplying the number of shares by the opening price. 

Our next sheet is ATR adjusted capital:

![Screenshot 2024-11-07 124534](https://github.com/user-attachments/assets/64c468a5-12c1-4c27-8dad-4ba54de2ee78)

The sheet is very similar to our beta sheet, however we use the ATR to measure the volatility of the individual symbol. The ATR Adjusted Capital can be found in column G for longs and column S for shorts. Using our longs as an example, the values for ATR Adjusted Capital are retrieved from our Copy/Paste tab with our formula ={"ATR Adj. Capital";'Copy/Paste'!M2:M11}. The actual capital is computed the same as in the Beta sheet.

Our next sheet is the FV OPG Limit Order Sheet:

![Screenshot 2024-11-07 132359](https://github.com/user-attachments/assets/ae57cb4f-2f54-4741-bb58-84eeaf5d70d9)

This strategy is unique from the others covered thus far in that it uses OPG limit orders as opposed to market orders. With this type of order, we either have our trade filled at the limit price set or better, or the order gets canceled entirely. The limit price is dictated by our Fair Value calculation, which is calculated based on the percentage difference between the premarket price of the SPY and the previous close of the SPY. Columns I1:L3 contain the formula needed to make this calculation. We enter in the closing date in cell J3, and the current price of SPY as of 9:15am EST into cell K2. The percentage change is then calculated via our formula: 
=(K2-index(googlefinance("SPY", "close",J3),2,2))/index(googlefinance("SPY", "close",J3),2,2)

This percentage difference is key to calculating the "expected open" which is the value found in columns D and Q for our longs and shorts respectively. Using our longs as an example, the formula for calculation is as follows:

=round(index(GOOGLEFINANCE(C12,"close",J$3),2,2)+$L$1*index(GOOGLEFINANCE(C12,"close",J$3),2,2),2)

In this formula, we are taking the closing price of our stock symbol in cell C12 and adjusting it based on the percentage value we calculated in cell L1. With this calculation applied to each of our stock symbols, we now have our Fair Value filled for our longs in column D. This Fair Value means that if the stock price of our longs opens at this price or lower, we should be interested in buying. Conversely, if the opening price for our short stocks is at the expected open price or higher, we should be interested in shorting. We use limit orders to enter in our prices at fair value. The open price determines if we get filled or not. For our longs, if the stock price opens at or below or fair value, we get filled at the opening price. For our shorts, if the stock opens at our above our fair value price, then we get filled at the opening price. 

Whether we get filled or not can be seen in our "Difference" Columns, which is column G for our longs and column T for our shorts. If we are successfully filled, we will see the price difference calculation in the corresponding cell. However , if we do not get a fill, then we see a value of "NO FILL" within the cell. 

For this sheet, we will most likely be getting an uneven amount of longs between our longs and shorts. For example, if the market is opening at a strong discount for a given morning, we would get filled on more longs than shorts in this scenario. For this reason, the HEDGE section of our sheet is more relevant in this FV OPG Limit Orders sheet than it has been in previous sheets. The user can choose to use SPY or any other vehicle to add to their long side or short side to balance out capital as needed. The hedging units is calculated as previously described, and performance will be displayed at the top of our sheet. 








