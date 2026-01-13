# Stock Options Guide

## Basic Concepts

### 1. Call Options (Right to Buy)
Think of a "Call" as "calling" the stock away from someone.

*   **Buy a Call (Long Call)**
    *   **Outlook:** Bullish (You think the stock will go **UP**).
    *   **Action:** You pay a premium to get the right to buy the stock at a fixed price (Strike).
    *   **Result:** If the stock price skyrockets, you can buy it cheap at the strike price. Your potential profit is unlimited.
    *   **Risk:** Limited to the premium you paid.

*   **Sell a Call (Short Call / Covered Call)**
    *   **Outlook:** Neutral to Bearish (You think the stock will stay flat or go down slightly).
    *   **Action:** You receive a premium and take on the obligation to sell your stock at the strike price if asked.
    *   **Result:** If the stock stays below the strike, you keep the premium as profit.
    *   **Risk:** If the stock skyrockets, you have to sell your shares at the lower strike price, missing out on the gains (if you own the shares). If you don't own the shares (Naked Call), the risk is unlimited.

### 2. Put Options (Right to Sell)
Think of a "Put" as "putting" the stock to someone else.

*   **Buy a Put (Long Put)**
    *   **Outlook:** Bearish (You think the stock will go **DOWN**).
    *   **Action:** You pay a premium to get the right to sell the stock at a fixed price (Strike).
    *   **Result:** If the stock crashes, you can sell it at the higher strike price.
    *   **Risk:** Limited to the premium you paid.

*   **Sell a Put (Short Put)**
    *   **Outlook:** Neutral to Bullish (You think the stock will stay flat or go up).
    *   **Action:** You receive a premium and take on the obligation to buy the stock at the strike price if asked.
    *   **Result:** If the stock stays above the strike, you keep the premium. This is often used to get paid to wait to buy a stock you like.
    *   **Risk:** If the stock crashes, you are forced to buy it at the strike price, which is now higher than the market price.

### Summary Matrix

| Strategy | You Pay / Receive | Your Right / Obligation | Market View |
| :--- | :--- | :--- | :--- |
| **Buy Call** | Pay Premium | **Right** to Buy | Bullish (Go Up) |
| **Sell Call** | Receive Premium | **Obligation** to Sell | Bearish / Neutral |
| **Buy Put** | Pay Premium | **Right** to Sell | Bearish (Go Down) |
| **Sell Put** | Receive Premium | **Obligation** to Buy | Bullish / Neutral |

## 3. Volatility Strategies (Straddle & Strangle)
These strategies are used when you expect a big move but don't know the direction.

### Straddle (Long Straddle)
*   **Concept:** Buy a Call AND a Put at the **same** strike price and expiration.
*   **Outlook:** You expect a **massive** move (e.g., earnings report), but you don't know if it will be up or down.
*   **Cost:** Expensive (you pay two premiums).
*   **Result:** You profit if the stock moves far enough in *either* direction to cover the cost of both premiums.

### Strangle (Long Strangle)
*   **Concept:** Buy a Put at a lower strike and a Call at a higher strike.
*   **Outlook:** Similar to a Straddle (expecting a big move), but you want it to be cheaper.
*   **Cost:** Cheaper than a Straddle (options are "out of the money").
*   **Result:** The stock needs to move **even more** than in a Straddle to be profitable, but your upfront cost is lower.

## 4. Vertical Spreads (Defined Risk)
Spreads involve buying one option and selling another to limit risk and cost.

### Bull Call Spread (Debit Spread)
*   **Concept:** Buy a Call at a lower strike, Sell a Call at a higher strike.
*   **Outlook:** Moderately Bullish.
*   **Benefit:** Cheaper than buying a straight call.
*   **Trade-off:** Your profit is capped at the higher strike price.

### Bear Put Spread (Debit Spread)
*   **Concept:** Buy a Put at a higher strike, Sell a Put at a lower strike.
*   **Outlook:** Moderately Bearish.
*   **Benefit:** Cheaper than buying a straight put.
*   **Trade-off:** Your profit is capped at the lower strike price.

## 5. Income Strategies (The "Wheel" & Condors)

### Iron Condor
*   **Concept:** Sell a Put Spread AND Sell a Call Spread. (4 legs total).
*   **Outlook:** Neutral (Stock stays in a range).
*   **Goal:** You want the stock to stay boring, between your two short strikes, so all options expire worthless and you keep the premium.

### The Wheel Strategy
*   **Step 1:** Sell Cash-Secured Puts on a stock you like until you get assigned (buy the stock).
*   **Step 2:** Sell Covered Calls on that stock until it gets called away (sell the stock).
*   **Step 3:** Repeat.
*   **Goal:** Generate consistent income from premiums while holding a stock.

## 6. Protective Strategies

### Protective Put (Married Put)
*   **Concept:** Own the stock + Buy a Put.
*   **Outlook:** Bullish long-term, but fearful of a short-term crash.
*   **Function:** Acts like an insurance policy. If the stock crashes, the Put gains value, offsetting the loss on the shares.

### Covered Call
*   **Concept:** Own the stock + Sell a Call.
*   **Outlook:** Neutral / Slightly Bullish.
*   **Function:** Generate income from an existing portfolio. You cap your upside but get paid cash now.

## 7. Advanced / Complex Strategies

### Butterfly Spread
*   **Concept:** Buy 1 Call (low strike), Sell 2 Calls (middle strike), Buy 1 Call (high strike).
*   **Outlook:** Neutral. You want the stock to pin exactly at the middle strike.
*   **Benefit:** Very low cost to enter. High potential reward vs risk.
*   **Trade-off:** Very narrow window of profit.

### Calendar Spread (Time Spread)
*   **Concept:** Sell a short-term option, Buy a long-term option (same strike).
*   **Outlook:** Neutral.
*   **Goal:** Profit from the faster time decay (theta) of the short-term option you sold.

### Collar
*   **Concept:** Own Stock + Buy Put (Protection) + Sell Call (Income).
*   **Outlook:** Conservative Bullish.
*   **Goal:** Low risk, low reward. The sold Call pays for the bought Put. You are "collared" between a floor and a ceiling.

### Ratio Spread
*   **Concept:** Buy 1 option, Sell 2 (or more) options at a different strike.
*   **Outlook:** Directional but with a safety buffer.
*   **Risk:** Unlimited risk if the stock moves too far past your short strikes (because you sold more than you bought).

## 8. The Greeks (Risk Metrics)
Understanding "The Greeks" is essential for advanced trading.

*   **Delta ($\Delta$):** How much the option price changes for a $1 move in the stock. (e.g., Delta 0.50 means option gains $0.50 if stock goes up $1). Also approximates the % probability of expiring in-the-money.
*   **Gamma ($\Gamma$):** How much Delta changes for a $1 move in the stock. (Acceleration of Delta).
*   **Theta ($\Theta$):** Time decay. How much value the option loses **per day** as expiration approaches.
*   **Vega ($\nu$):** Sensitivity to Volatility. How much the option price changes for a 1% change in Implied Volatility.
*   **Rho ($\rho$):** Sensitivity to Interest Rates. (Less important for short-term trading).

## 9. Synthetic Strategies
Creating a position that mimics a stock or another option using a combination of calls and puts.

### Synthetic Long Stock
*   **Concept:** Buy Call + Sell Put (same strike).
*   **Result:** Mimics owning 100 shares of stock exactly, but with much less capital upfront.
*   **Risk:** Same as owning the stock (can go to zero).

### Synthetic Short Stock
*   **Concept:** Sell Call + Buy Put (same strike).
*   **Result:** Mimics shorting 100 shares of stock.

## 10. Other Notable Strategies

### Diagonal Spread
*   **Concept:** A spread with different strikes AND different expiration dates. (Mix of Vertical and Calendar).
*   **Poor Man's Covered Call (PMCC):** A specific diagonal spread. Buy a deep ITM Long-Term Call (LEAPS) and sell a short-term OTM Call against it. Mimics a covered call with less capital.

### Box Spread
*   **Concept:** Bull Call Spread + Bear Put Spread.
*   **Result:** Arbitrage strategy. Should yield a risk-free return (interest rate). Mostly for market makers.

### Jade Lizard
*   **Concept:** Sell Short Put + Sell Call Spread.
*   **Goal:** Collect enough premium so that if the stock shoots up through the Call Spread, you still don't lose money. No upside risk, only downside risk.

## 11. Order Terminology
How to actually place the trades.

*   **Buy to Open (BTO):** You are buying an option to start a new position. (Long).
*   **Sell to Open (STO):** You are selling an option to start a new position. (Short).
*   **Sell to Close (STC):** You are selling an option you own to exit the position.
*   **Buy to Close (BTC):** You are buying back an option you sold to exit the position.

## 12. Assignment & Exercise
What happens at the end.

*   **Exercise:** The buyer of the option chooses to use their right to buy/sell the stock.
*   **Assignment:** The seller of the option is randomly chosen to fulfill their obligation.
*   **Expiration:** The date the contract ends.
*   **Pin Risk:** The risk that the stock closes exactly at the strike price, leaving you unsure if you will be assigned.

## 13. Even More Variations

### Backspread (Reverse Ratio Spread)
*   **Concept:** Sell 1 option, Buy 2 (or more) options further OTM.
*   **Outlook:** Expecting a massive move.
*   **Benefit:** Can be done for a credit (get paid to enter). Unlimited profit potential if the move is huge.

### Broken Wing Butterfly
*   **Concept:** A Butterfly spread where one "wing" is further away (skipping a strike).
*   **Goal:** To make the trade zero cost or a credit to enter, removing risk on one side of the trade.

### Zebra (Zero Extrinsic Back Ratio)
*   **Concept:** Buy 2 ITM calls, Sell 1 ATM call.
*   **Goal:** Mimic owning the stock with 100 delta, but with zero extrinsic value (no time decay).

### Strap & Strip
*   **Strap:** Buy 2 Calls + 1 Put (Bullish Volatility).
*   **Strip:** Buy 1 Call + 2 Puts (Bearish Volatility).

## 14. How to Practice (Risk-Free)

### Paper Trading (Simulators)
The best way to learn is to trade with fake money in a real market environment.
*   **Thinkorswim (Schwab):** Has a powerful "OnDemand" feature to replay past market days and a live paperMoney account.
*   **Webull / E*TRADE / Interactive Brokers:** All offer paper trading accounts.
*   **OptionStrat / OptionsProfitCalculator:** Visual tools to build trades and see how they perform over time and price changes without placing orders.

### Backtesting
*   **Concept:** Testing your strategy on historical data to see how it *would have* performed.
*   **Tools:** Lookback, Tastytrade, or coding your own scripts (Python/Pandas) to analyze past option chains.

### The "1-Lot" Method
*   **Concept:** Once you are comfortable with paper trading, trade the smallest possible size (1 contract) on a low-priced stock or ETF.
*   **Goal:** To experience the *emotions* of trading real money (fear/greed) which simulators cannot replicate, but with minimal financial risk.
