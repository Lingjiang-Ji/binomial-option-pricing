# 🌲Binomial Option Pricing Model

A simple implementation of the **Binomial Tree Model** for pricing **European** and **American** options (both call and put).  
This project demonstrates backward induction for option valuation in discrete time.

---

## Features
- Supports **European** and **American** style options
- Handles both **call** and **put**
- Configurable parameters:
  - Initial stock price `S0`
  - Strike price `K`
  - Risk-free rate `r`
  - Volatility `sigma`
  - Time to maturity `T`
  - Number of steps `n`
- Clean, well-documented Python class

It’s a class that prices both European and American options under the **Cox–Ross–Rubinstein** framework.  
I made it a class to keep everything organized and reusable.

In the `__init__` function, I initialize all the model parameters —  
the initial stock price, strike, risk-free rate, volatility, maturity, and the number of time steps.  
Then I compute the time step `dt`, the up and down factors `u` and `d`, the discount factor `R`,  
and finally the risk-neutral probability \( p = \frac{R - d}{u - d} \).

The `_payoff()` method is an internal helper that defines the terminal payoff —  
either `max(S − K, 0)` for a call or `max(K − S, 0)` for a put.  
I defined it this way to keep the payoff logic encapsulated within the class,  
so the public interface only exposes the main pricing function.

The core logic is the **backward induction**:  
I start from the final step, where I know the payoff, and move backward step by step,  
discounting at each stage with the risk-neutral probability.

In the `price()` method, I first build the stock-price tree at maturity, compute all terminal payoffs,  
and then roll the values backward through the tree.  
At each node, I take the discounted expected value under the risk-neutral probability.  
If it’s an **American option**, I also compare the exercise value with the continuation value and take the maximum —  
that’s the **early-exercise feature**.  
Finally, the value at the root node is the option’s **fair price today**.

This implementation clearly shows how the **risk-neutral assumption** and **backward induction** work in discrete-time pricing.

## Sample Usage
American put option:

put_option = BinomialOption(S0=100, K=100, r=0.05, sigma=0.2, T=1, n=100, option_type='put', american=True)

print(f"American Put Option Price: {put_option.price():.4f}")

American Put Option Price: 6.3678

European call option :

call_option = BinomialOption(S0=100, K=100, r=0.05, sigma=0.2, T=1, n=100, option_type='call', american=False)

print(f"European Call Option Price: {call_option.price():.4f}")

European Call Option Price: 10.4306

European put option:

put_option = BinomialOption(S0=100, K=100, r=0.05, sigma=0.2, T=1, n=100, option_type='put', american=False)

print(f"European Put Option Price: {put_option.price():.4f}")

European Put Option Price： 5.5536
