# How I Built My First Financial Dashboard in Tableau as a Year 1 Data Science Student

*A step-by-step story of sourcing real stock data, cleaning it, and publishing a live interactive dashboard — all in one day.*

---

## The Goal

As a first-year student majoring in Data Science with a minor in FinTech, I wanted a hands-on project that combined both disciplines. Reading about Tableau in class is one thing — actually building something real with it is another.

So I set myself a challenge: **build a complete, interactive stock market dashboard using real historical data and publish it online.**

Here's exactly how I did it, what went wrong, and what I learned.

---

## The Data

I used **Apple (AAPL)** historical stock price data from Kaggle — a free dataset with daily prices going all the way back to 1984. The dataset had 7 columns:

- **Date** — the trading day
- **Open** — price at market open
- **High** — highest price of the day
- **Low** — lowest price of the day
- **Close** — price at market close
- **Volume** — number of shares traded
- **OpenInt** — open interest (not relevant for stocks, always 0)

The data covered over **8,000 trading days** — more than 33 years of Apple's journey from a $0.42 stock to a $1,058 stock.

---

## The Challenges (Real Talk)

Nothing went perfectly. Here's what actually happened:

**Yahoo Finance blocked direct downloads.** The direct CSV download API now requires login. I had to switch to Kaggle instead.

**The files were .txt not .csv.** The Kaggle data came as `.txt` files. Turns out they're still comma-separated — Excel and Tableau can read them fine.

**Combining files was messy.** When I copy-pasted three company files together in Excel, the rows ended up wrapped in quote marks, which confused Tableau. A simple Find & Replace in Notepad (replacing `"` with nothing) fixed it instantly.

**Zero values in the data.** The dataset had some empty rows with 0 values for price. I filtered these out in Tableau using a simple "At least 1" filter on the Close field.

These aren't failures — they're exactly the kind of data cleaning challenges that real data analysts face every day.

---

## What I Built

The final dashboard has 4 components:

### 1. Price Chart with 20-Day Moving Average
A dual-axis line chart showing the daily closing price overlaid with a 20-day moving average. The moving average smooths out daily noise and makes the long-term trend much clearer.

The calculated field I used:
```
WINDOW_AVG(AVG([Close]), -19, 0)
```

This tells Tableau to look back 19 days and average the closing price — a standard technique in financial analysis.

### 2. Volume Chart
A bar chart showing daily trading volume colored by price direction — **green for days when the stock closed higher than it opened, red for days it closed lower.** This is the classic candlestick chart color convention used by every financial platform.

The calculated field:
```
IF [Close] >= [Open] THEN "Up" ELSE "Down" END
```

### 3. Daily Change % Histogram
This chart shows the distribution of daily percentage price moves. Most days Apple moved between -2% and +2%. The rare extreme moves on either tail are clearly visible — this is the famous "fat tails" phenomenon that financial risk analysts study.

The calculated field:
```
([Close] - [Open]) / [Open] * 100
```

### 4. KPI Summary Table
A simple text table at the top showing:
- Average daily trading volume: **78,825,353 shares**
- Maximum closing price: **$1,058**
- Minimum closing price: **$1**

---

## Key Insights from the Data

Looking at the charts, a few things immediately stood out:

**Apple's explosive growth happened after 2004.** For its first 20 years as a public company, Apple stock barely moved. The iPhone era changed everything.

**The 2008 financial crisis is clearly visible** as a sharp drop in both price and volume.

**Most daily moves are tiny.** The histogram shows that roughly 70% of all trading days saw Apple move less than 1% in either direction. The big dramatic moves that make headlines are actually very rare.

**Volume peaked around 2007–2009.** This was the period of maximum uncertainty — the iPhone launch excitement combined with the financial crisis created enormous trading activity.

---

## What I Learned

**Tableau is genuinely powerful for financial data.** The dual-axis feature, calculated fields, and dashboard actions make it possible to build things that look professional without writing a single line of code.

**Data cleaning takes longer than the actual analysis.** Of the time I spent on this project, roughly 40% was getting the data into the right format. This is exactly what working data scientists say — and now I believe them.

**Calculated fields are where the real analysis happens.** The moving average, direction flag, and daily change percentage transformed raw price data into meaningful financial metrics. Understanding what to calculate and why is the actual skill.

**Publishing makes it real.** Having a live URL I can share makes this feel like actual work, not just a class exercise.

---

## The Live Dashboard

You can explore the interactive dashboard here:

👉 [Apple Stock Performance Explorer 1984–2018](https://public.tableau.com/app/profile/thanisha.srinivasa.rao/viz/AppleStockPerformance_17809325823410/Dashboard1)

Click on any point in the price chart to filter the volume and histogram charts to that time period.

---

## Tools Used

- **Kaggle** — free historical stock price data
- **Microsoft Excel** — basic data cleaning and combining files
- **Notepad** — fixing CSV formatting issues
- **Tableau Public** — free data visualization and publishing

Total cost: **$0**

---

## What's Next

This project only covers Apple data up to 2018. Some ideas for extending it:

- Add Microsoft and Google data for a proper multi-company comparison
- Pull in more recent data (2018–2026) to see the COVID crash and recovery
- Add a sentiment analysis layer using news data
- Build a similar dashboard in Python using Plotly for a code-based version

---

*I'm Thanisha, a Year 1 Data Science student with a minor in FinTech. I'm documenting my learning journey one project at a time. If you found this useful or have suggestions, drop a comment below!*

---

**Tags:** Data Science, Tableau, FinTech, Stock Market, Data Visualization, Beginner, Portfolio Project
