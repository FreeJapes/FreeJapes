import ccxt
import time

# Initialize the trading API
exchange = ccxt.your_exchange({
    'apiKey': 'your_api_key',
    'secret': 'your_api_secret',
    'enableRateLimit': True,
})

# Define trading parameters
symbol = 'NAS100/USD'
buy_threshold = 0.5  # Example: Buy if the price drops by 0.5%
sell_threshold = 0.5  # Example: Sell if the price increases by 0.5%
investment_amount = 1000  # Example: Amount to invest in USD

# Main trading loop
while True:
    try:
        # Fetch ticker information
        ticker = exchange.fetch_ticker(symbol)

        # Get the current price
        current_price = ticker['last']

        # Example: Buy if the price drops by buy_threshold
        if current_price < (1 - buy_threshold / 100) * current_price:
            # Calculate the amount of assets to buy
            amount_to_buy = investment_amount / current_price

            # Execute the buy order
            order = exchange.create_market_buy_order(symbol, amount_to_buy)
            print('Buy order executed:', order)

        # Example: Sell if the price increases by sell_threshold
        elif current_price > (1 + sell_threshold / 100) * current_price:
            # Calculate the amount of assets to sell
            amount_to_sell = investment_amount / current_price

            # Execute the sell order
            order = exchange.create_market_sell_order(symbol, amount_to_sell)
            print('Sell order executed:', order)

        # Wait for a specific time interval (e.g., 1 hour)
        time.sleep(3600)

    except Exception as e:
        print('Error:', e)
        # Handle errors gracefully

