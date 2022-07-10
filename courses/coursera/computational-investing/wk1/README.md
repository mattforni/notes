## Metrics
### Annual Return
* return = (value[end] / value[start]) - 1

### Risk: STDEV of Return
In other words, volatility. Lower volatility is better

* daily_rets[i] = (value[i] - value[i-1]) - 1
* risk = stdev(daily_rets)

### Risk: Draw Down
When the portfolio went down, how much did it go down by

### Reward/Risk: Sharpe Ratio
Most important measure for asset performance. How well does an asset perform for the risk taken. Higher Sharpe Ratio means more return for the same risk

* S = Reward / Risk, _where Reward = E[R - R_f], Risk = σ_
* S = E[R - R_f] / σ
* sharpe = k * mean(daily\_rets) / stdev(daily\_rets)
* k = sqrt(250), _where 250 the number of trading days, 12 for monthly_
  
### Reward/Risk: Sortino Ratio
Different from Sharpe Ratio in that it only counts Risk when the fund goes down

### Jensen's Alpha

## Terminology
* **Return**
  
  The gain or loss of a security in a particular period. The return consists of the income and the capital gains relative on an investment. It is usually quoted as a percentage.

* **Risk**

  The chance that an investment's actual return will be different than expected. Risk includes the possibility of losing some or all of the original investment. Different versions of risk are usually measured by calculating the standard deviation of the historical returns or average returns of a specific investment. A high standard deviation indicates a high degree of risk.Many companies now allocate large amounts of money and time in developing risk management strategies to help manage risks associated with their business and investment dealings. A key component of the risk mangement process is risk assessment, which involves the determination of the risks surrounding a business or investment.

* **Arbitrage**

  The simultaneous purchase and sale of an asset in order to profit from a difference in the price. It is a trade that profits by exploiting price differences of identical or similar financial instruments, on different markets or in different forms. Arbitrage exists as a result of market inefficiencies; it provides a mechanism to ensure prices do not deviate substantially from fair value for long periods of time.

* **Order Book**

  A trading floor participant responsible for maintaining a list of public market or limit orders of a specific option class using the "market-marker" system of executing orders.