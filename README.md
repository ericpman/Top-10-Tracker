# Top-10 Tracker

The **Top-10 Tracker** is a comprehensive tool designed to evaluate and compare the performance of a "Top 10" portfolio of stocks using multiple trading strategies, including Market Orders, Fair Value Orders, Beta Adjusted Orders, and ATR Adjusted Orders. By tracking both long and short positions across various allocation strategies, the tool offers insights into the portfolio’s total and individual basket performance.

[Access the Google Sheet here](https://docs.google.com/spreadsheets/d/1Q2TxpCMEJM3xRL62TuPfSDHrdnqo6-lJw3gpthWFM4E/edit?usp=sharing)

## Objectives

The primary goal of the Top-10 Tracker is to monitor and analyze the performance of selected stocks in both long and short baskets. The tool:
- Calculates the overall return on the combined long and short baskets.
- Tracks the individual performance of the long and short baskets separately.
- Enables dynamic allocation adjustments for each trading symbol, facilitating tailored performance insights.

## Strategy Breakdown

The Top-10 Tracker supports multiple trading strategies to meet diverse investment objectives and market conditions. Each strategy allocates capital based on specific criteria, allowing for a comparison of results across different market environments:

1. **Market Orders**
   - **Description**: A straightforward approach involving market orders at the opening price for all stocks in the portfolio.
   - **Allocation**: Evenly distributes capital across all symbols based on the amount set in the capital allocation cell.

2. **Beta Adjusted**
   - **Description**: This strategy allocates capital according to each stock's beta, a measure of its volatility relative to the broader market.
   - **Benefit**: Beta-adjusted capital allocation increases exposure to stocks with a higher market correlation while reducing exposure to stocks with lower correlation, balancing portfolio risk based on volatility.

3. **ATR Adjusted**
   - **Description**: Similar to the Beta Adjusted strategy, this approach allocates capital based on the stock's Average True Range (ATR), a volatility measure that considers both historical highs and lows.
   - **Benefit**: ATR-based allocation allows for capital adjustments based on each stock's price volatility, potentially improving stability in uncertain market conditions.

4. **Fair Value (FV) OPG Limit Orders**
   - **Description**: Uses limit OPG (Opening Only) orders to set entry prices, with a focus on value relative to the SP500 index (SPY ETF) before market open (around 9:15 AM EST).
   - **Allocation**: Limit orders are placed based on expected opening prices and may not be filled if prices don’t match set thresholds, allowing selective participation based on pre-market signals.

5. **FV OPG Limit + Beta**
   - **Description**: Combines the Fair Value OPG Limit Order approach with Beta Adjusted capital allocation.
   - **Benefit**: This strategy merges selective entry based on fair value with beta-scaled capital allocation, integrating both price sensitivity and volatility-based weighting.




## Sheet Breakdown

Below is a detailed breakdown of each sheet in the Top-10 Tracker, explaining how symbols are chosen, the calculations involved, and how data is aggregated to evaluate each strategy’s performance.

### Copy/Paste Sheet

The first step of the tracking process is in the **Copy/Paste** sheet, which contains essential data on our selected stocks. Here’s an example screenshot of this sheet:

![Screenshot of Copy/Paste Sheet](https://github.com/user-attachments/assets/7a069e36-c82b-4573-b72f-a642cc50810e)

This sheet organizes information as follows:
- **Column A**: Our top 10 long stock symbols are highlighted in green, and our top 10 short stock symbols are in red.
- **Columns B and C**: Show each stock’s company name and sector.
- **Column H**: Lists each stock's beta, providing a measure of market volatility.
- **Column I**: Lists each stock’s Average True Range (ATR), another measure of volatility.

#### ATR Adjusted Capital Calculation

In **Columns K through O**, automatic calculations are performed to determine the ATR-adjusted capital allocation for each stock:
- **Column K**: Displays each stock’s last closing price using the formula `=googlefinance(A2,"closeyest")`.
- **Column L**: Calculates the ATR as a percentage of the stock's price by dividing the ATR in **Column I** by the price in **Column K**.
- **Column M**: Computes ATR-adjusted capital based on the percentage in Column L. The capital for the stock with the highest ATR-to-price percentage (found in Column L) is set to the value in cell **O1**. The formula `=if(L2=max(L$2:L),O$1,(max(L$2:L)*O$1)/L2)` scales other values proportionally to this maximum, giving each stock an adjusted capital allocation in **Column M**.

The ATR-adjusted capital for SPY, used as a benchmark, is based on its ATR in **cell J23** and is calculated in **Row 22**.

With all data loaded into the **Copy/Paste** sheet, subsequent sheets retrieve this information for performance tracking. Let’s explore the **Market Orders OC** sheet in more detail:

---

### Market Orders OC Sheet

The **Market Orders OC** sheet provides a straightforward approach to tracking market orders. Here’s a screenshot to illustrate the layout:

![Screenshot of Market Orders OC Sheet](https://github.com/user-attachments/assets/782952bb-f070-47ac-8bfa-77d991bc7016)

This strategy enters trades at the open price for the current trading day, regardless of price movements. Key performance metrics are also included to track capital deployment, returns, and profit/loss.

#### Key Sections in Market Orders OC Sheet

- **Performance Summary (Top-Left)**
  - **Cells B3 and C3**: Display the overall Return on Capital (ROC) and Profit & Loss (P&L) for the portfolio.
  - **Cells B5 and C5**: ROC and P&L for long positions.
  - **Cells B6 and C6**: ROC and P&L for short positions.
  - **Cells E3 and F3**: Show the total capital deployed and the number of symbols traded.
  - **Row 7**: Placeholder for hedge metrics, in case a hedge (e.g., SPY) is added to balance net long or short positions.

- **Capital Allocation (Cell I2)**: Determines the capital allocated per symbol.

- **Detailed Symbol Tracking (Rows 10–21)**
  - **Column C**: Displays stock symbols for long positions, sourced from the **Copy/Paste** sheet via the formula `={"Symbols";'Copy/Paste'!A2:A11}`.
  - **Columns D to J**: Track each long position’s opening price, current price, price change, capital allocation, shares, P&L, and ROC.
  - **Columns N to U**: Contain the same information for short positions.

Additional metrics for longs and shorts are found in **G10, I10, and J10** for longs and in **R10, T10, and U10** for shorts.

#### Hedge Calculations

In **Cell D26**, the tracker calculates hedge units by taking the difference between the number of short and long symbols. This value helps determine whether additional hedge positions, like SPY, are necessary to balance allocation.

- **Hedge Data (D25:L28)**: Information on hedge positions is organized here.
  - **E28**: Symbol for the hedge (e.g., SPY).
  - **Columns F to L**: Track the hedge’s opening price, current price, price change, capital, shares, P&L, and ROC.


## Beta Adjusted Sheet

The **Beta Adjusted** sheet adjusts capital based on each stock’s beta.

![Screenshot of Beta Adjusted Sheet](https://github.com/user-attachments/assets/aa01906e-9486-473a-8e28-1a1459f92ba7)

### Key Differences in Columns

The beta adjustment data is located in:
- **Columns G11:I21** for longs
- **Columns T11:V21** for shorts

#### Columns Overview:
- **Beta**: Retrieved from the **Copy/Paste** sheet using the formula `={"Beta";'Copy/Paste'!H2:H11}`.
- **Beta Adjusted Capital**: For longs, this is calculated by dividing the allocated capital (cell **K2**) by the beta in **Column G**. Formula: `={"Beta Adj. Capital";arrayformula(if(isnumber(G12:G21),DIVIDE(K2,G12:G21),""))}`.
- **Actual Capital**: Calculated by multiplying the number of shares (based on beta-adjusted capital and opening price) by the opening price.

---

## ATR Adjusted Capital Sheet

The **ATR Adjusted** sheet uses ATR to adjust capital based on each stock's volatility.

![Screenshot of ATR Adjusted Capital Sheet](https://github.com/user-attachments/assets/64c468a5-12c1-4c27-8dad-4ba54de2ee78)

This sheet is similar to the Beta sheet, but it uses **ATR** as the volatility measure:
- **ATR Adjusted Capital** is in **Column G** for longs and **Column S** for shorts.
- ATR data is retrieved from the **Copy/Paste** sheet using the formula `={"ATR Adj. Capital";'Copy/Paste'!M2:M11}`.

**Actual Capital** is calculated in the same way as in the Beta Adjusted sheet.

---

## FV OPG Limit Order Sheet

The **FV OPG Limit Order** sheet uses OPG limit orders based on a Fair Value (FV) calculation.

![Screenshot of FV OPG Limit Order Sheet](https://github.com/user-attachments/assets/ae57cb4f-2f54-4741-bb58-84eeaf5d70d9)

This strategy differs from the previous ones as it uses **OPG limit orders**. If an order fills at the limit price (or better), it executes; otherwise, it is canceled. The limit price is determined by the **Fair Value calculation** based on the SPY’s premarket percentage difference from its previous close.

#### Key Calculations
- **Fair Value Calculation**: Found in **Cells I1:L3**.
  - **Close Date**: Entered in **Cell J3**.
  - **SPY Premarket Price**: Entered in **Cell K2**.
  - **Percentage Difference**: `=(K2-index(googlefinance("SPY", "close",J3),2,2))/index(googlefinance("SPY", "close",J3),2,2)`

- **Expected Open** (Longs in **Column D** and Shorts in **Column Q**): This uses the formula `=round(index(GOOGLEFINANCE(C12,"close",J$3),2,2)+$L$1*index(GOOGLEFINANCE(C12,"close",J$3),2,2),2)`, adjusting the prior close based on the calculated percentage in **Cell L1**.

#### Order Execution
- **Longs**: Buy if the stock price opens at or below the Fair Value.
- **Shorts**: Short if the stock price opens at or above the Fair Value.

Fill status is shown in the **Difference** columns:
- **Column G** for longs
- **Column T** for shorts

If filled, the price difference appears in the cell; if not, it displays "NO FILL".

**Hedge Calculations** are more relevant here due to potential uneven fills between longs and shorts.

---

## FV OPG Limit + Beta Adjusted Sheet

This sheet combines the FV OPG Limit Order strategy with **Beta Adjusted Capital Allocation**.

![Screenshot of FV OPG Limit + Beta Adjusted Sheet](https://github.com/user-attachments/assets/5d3fc8a3-01e0-4947-8fa9-3971af334a9f)

In this strategy, limit orders use the **Fair Value calculation** but capital allocation is based on each stock's beta, as in the **Beta Adjusted** sheet.

---

## Results Tab

The **Results** tab provides an end-of-day breakdown of the performance for each strategy, showing totals as well as the individual performance of longs, shorts, and the hedge. Here’s a screenshot:

![Screenshot of Results Tab](https://github.com/user-attachments/assets/909cfc79-b653-435e-ada4-945d218cd617)

---

This Google Sheet is designed to provide a comprehensive comparison of five trading strategies. This kind of comparative tracking is crucial for traders to refine their decision-making and optimize strategy selection. By automating the tracking process, this tool becomes a powerful asset for improving a trader’s strategy development and evaluation.










