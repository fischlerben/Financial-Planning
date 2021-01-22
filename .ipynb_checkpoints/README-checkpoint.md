# Financial Planning
This project, written in Python, creates two tools used for the purposes of financial planning: it creates Personal Finance Planner that allows for users to visualize their savings composed by investments in shares and cryptocurrencies, and it creates a Retirement Planning Tool that will use the Alpaca API to fetch historical closing prices for a retirement portfolio composed of stocks and bonds, then run Monte Carlo simulations to project the portfolio performance at 30 years.
![Logo](Pic.jpg)

---

## Personal Finance Planner:

The Alternative Free Crypto API was used to retrieve Bitcoin and Ethereum prices.  After connecting to the API, the following code was used to compute total value of crypto portfolio:

    # Fetch prices of both BTC and ETH in order to compute total value of crypto portfolio
    btc_price = btc_data['data']['1']['quotes']['USD']['price']
    eth_price = eth_data['data']['1027']['quotes']['USD']['price']

    # Compute current value of my crypto
    total_btc_value = my_btc * btc_price
    total_eth_value = my_eth * eth_price

    # Print current crypto wallet balance
    print(f"The current value of your {my_btc} BTC is ${total_btc_value:0.2f}.")
    print(f"The current value of your {my_eth} ETH is ${total_eth_value:0.2f}.")
    
    # Output = The current value of your 1.2 BTC is $15328.94.  The current value of your 5.3 ETH is $2087.56.
    
The Alpaca Markets API was used to pull historical stocks and bonds information.  After connecting to the API, the following code was used to compute total value of stocks/bonds portfolio:

    # Compute the current value of shares
    my_spy_value = my_spy * spy_close_price
    my_agg_value = my_agg * agg_close_price

    # Print current value of shares
    print(f"The current value of your {my_spy} SPY shares is ${my_spy_value:0.2f}.")
    print(f"The current value of your {my_agg} AGG shares is ${my_agg_value:0.2f}.")
    
An "Emergency Fund" can be created and an appropriate output generated with the following if/elif/else statement:
```
# Set ideal emergency fund
emergency_fund = monthly_income * 3

# Calculate total amount of savings
total_savings = df_savings.sum().item()

# Validate saving health
if total_savings > emergency_fund:
    print("Congratulations on having more than 3x your monthly income in your emergency fund.")
elif total_savings == emergency_fund:
    print("Congratulations on reaching your goal of 3x your monthly income in your emergency fund.")
else:
    print(f'You are ${emergency_fund - total_savings} away from reaching your goal of 3x your monthly income in your emergency fund.')
```
---

## Retirement Planning:

Five years' worth of historical data was obtained for SPY and AGG using the Alpaca API "get_barset()" function, and the following Monte Carlo simulation was configured to forecast 30 years cumulative returns (an MCForecastTools.py file was in same file path as notebook):

    MC_thirty_year = MCSimulation(portfolio_data = df_stock_data, weights = [.40,.60], num_simulation = 500, num_trading_days = 252*30)
    
### Monte Carlo Simulation Outcomes:

![monte_carlo_sim](monte_carlo_sim.png)

### Calculating the expected portfolio return at the 95% lower and upper confidence intervals based on a $20,000 initial investment:

    # Set initial investment
    initial_investment = 20000

    # Use the lower and upper `95%` confidence intervals to calculate the range of the possible outcomes of our $20,000 initial investment
    ci_lower = round(tbl[8]*initial_investment,2)
    ci_upper = round(tbl[9]*initial_investment,2)

    # Print results
    print(f"There is a 95% chance that an initial investment of ${initial_investment} in the portfolio over the next 30 years will end within in the range of ${ci_lower} and ${ci_upper}.")
    
    # Output = There is a 95% chance that an initial investment of $20000 in the portfolio over the next 30 years will end within in the range of $61695.16 and $632164.45.