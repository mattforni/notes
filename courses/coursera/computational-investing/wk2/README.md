## Value
* **Future Returns/Dividends**

  A company that prints $1/yr for infinite years, is it worth inifinity? How much am I willing to give to get $1 in a year. If a bank's interest rate is 1%, then the _discount rate_ is .99 **or** _(1 - 0.01)_. Present value of a dollar in the future is _the sum of the dividend x (discount rate ^ i), where i is years_
  
  intrinsic value = dividend * 1 / (1 - gamma), _where gamma = discount rate_
  
* **Book Value**

  Total assets - intangible assets and liabilities. i.e. Value of the pieces of the company if you could break it up and sell it piece by piece.
  
* **Market**

  The market is a very efficient processor of information (Efficient Markets Hypothesis)
  
  _value = # of shares outstanding * price of stock_

## Capital Assets Pricing Model
Introduced by Jack Treynor, William Sharpe, John Litner and Jon Mossin (1966). Sharpe, Markowitz and Merton Miller received to Nobel Prize for this work. Let to index investing and *proved* that investing in individual stocks is pointless.

You can use a _linear regression_ to find _beta_ and _alpha_. Slope is _beta_, whereas _alpha_ is the y intercept.

Beta and Correlation are **different**. Corrcoeff represents how closely the points are to the line (_beta_). Correlation can range from -1 - 1, where 1 is prefect correlation, and -1 is perfect anti-correlation.

### Assumptions
Return on a stock has two components:

1. Systematic (the market)
2. Residual (Expected value = 0)
3. Market Return:
  * _r\_i_ = _beta\_i_ * _r\_m_ + _alpha\_i_ 
      * _r\_i_ = return on stock
      * _beta\_i_ = how responsive a stock is to the market
      * _r\_m_ = return on the market
      * _alpha\_i_ = residual(random) value (expected to be 0)

## Portfolio
Can only beat the market if Beta > 1.0. That said, greater Beta = greater Risk.

* return\_portfolio = weighted\_sum(r\_position), *where weight is proportion of portfolio*
* beta\_portfolio = weighted\_sum(beta\_position)

## Terminology
* **Intrinsic Value**

  1. The actual value of a company or an asset based on an underlying perception of its true value including all aspects of the business, in terms of both tangible and intangible factors. This value may or may not be the same as the current market value. Value investors use a variety of analytical techniques in order to estimate the intrinsic value of securities in hopes of finding investments where the true value of the investment exceeds its current market value.

  2. For call options, this is the difference between the underlying stock's price and the strike price. For put options, it is the difference between the strike price and the underlying stock's price. In the case of both puts and calls, if the respective difference value is negative, the intrinsic value is given as zero.

* **Capital Assests Pricing Model**

  A model that describes the relationship between risk and expected return and that is used in the pricing of risky securities.Capital Asset Pricing Model (CAPM)The general idea behind CAPM is that investors need to be compensated in two ways: time value of money and risk. The time value of money is represented by the risk-free (rf) rate in the formula and compensates the investors for placing money in any investment over a period of time. The other half of the formula represents risk and calculates the amount of compensation the investor needs for taking on additional risk. This is calculated by taking a risk measure (beta) that compares the returns of the asset to the market over a period of time and to the market premium (Rm-rf).Read more: http://www.investopedia.com/terms/c/capm.asp#ixzz3YFEJisisFollow us: @Investopedia on Twitter
