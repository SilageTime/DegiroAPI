# DegiroAPI
An unofficial API for the trading platform Degiro written in Python

## Getting Started

### Installing
```
pip install degiroapi
```
### Dependecies
```
pip install requests
```
### Imports
```
import degiroapi
from degiroapi.product import Product
from degiroapi.order import Order
from degiroapi.utils import pretty_json
```
### Logging into your account
```
degiro = degiroapi.DeGiro()
degiro.login("username", "password")
```
### Logging out

```
degiro.logout()
```

## Available Functions
* login
* logout
* getdata
* search_products
* transactions
* orders
* get_stock_list
* buyorder
* sellorder
## getdata
Printing your current cach funds:
```
cachfunds = degiro.getdata(degiroapi.Data.Type.CACHFUNDS)
for data in cachfunds:
    print(data)
```
Printing your current portfolio:
```
portfolio = degiro.getdata(degiroapi.Data.Type.PORTFOLIO)
for data in portfolio:
    print(data)
```
## search_products
Searching for a product:
```
products = degiro.search_products('Pfizer')
print(Product(products[0]).id)
```
## transactions
Printing your transactions in a given time interval:
```
from datetime import datetime, timedelta

transactions = degiro.transactions(datetime(2019, 1, 1), datetime.now())
print(pretty_json(transactions))
```
## orders
Printing your order history(the maximum timespan is 90 days)
```
from datetime import datetime, timedelta

orders = degiro.orders(datetime.now() - timedelta(days=90), datetime.now())
print(pretty_json(orders))
```
## get_stock_list
Get the symbols of the S&P500 stocks:
```
sp5symbols = []
products = degiro.get_stock_list(14, 846)
for product in products:
    sp5symbols.append(Product(product).symbol)
```
Get the symbols of the german30 stocks:
```
daxsymbols = []
products = degiro.get_stock_list(6, 906)
for product in products:
    daxsymbols.append(Product(product).symbol)
```
## buyorder
Placing a buy order is dependent on the order Type:

### Limit order 
You have to set a limit order price to which the order gets executed.
**arguments**: order type, product id, execution time type (either 1 for "valid on a daily basis", or 3 for unlimited, size, limit(the limit price)
```
degiro.buyorder(Order.Type.LIMIT, Product(products[0]).id, 3, 1, 30)
```

### StopLimit order
Sets a limit order when the stoploss price is reached (not bought for more than the limit at the stop loss price):
**arguments**: order type, product id, execution time type (either 1 for "valid on a daily basis", or 3 for "unlimited"), size, limit(the limit price), stop_loss(stop loss price)
```
degiro.buyorder(Order.Type.STOPLIMIT, Product(products[0]).id, 3, 1, 38, 38)
```

### Market order
Bought at the market price:
**arguments**: order type, product id, execution time type (either 1 for "valid on a daily basis", or 3 for "unlimited"), size
```
degiro.buyorder(Order.Type.MARKET, Product(products[0]).id, 3, 1)
```

### StopLoss order
The stop loss price has to be higher than the current price, when current price reaches the stoploss price the order is placed:
**arguments**: order type, product id, execution time type (either 1 for "valid on a daily basis", or 3 for "unlimited"), size
```
degiro.buyorder(Order.Type.STOPLOSS, Product(products[0]).id, 3, 1, None, 38)
```

## sellorder
Placing a sell order is dependent on the order Type:
Equivalent to the buy orders:
```
degiro.sellorder(Order.Type.LIMIT, Product(products[0]).id, 3, 1, 40)
```

```
degiro.sellorder(Order.Type.STOPLIMIT, Product(products[0]).id, 3, 1, 37, 38)
```

```
degiro.sellorder(Order.Type.MARKET, Product(products[0]).id, 3, 1)
```

```
degiro.sellorder(Order.Type.STOPLOSS, Product(products[0]).id, 3, 1, None, 38)
```


## Usage
For documented examples see [examples.py](https://github.com/lolokraus/DegiroAPI/blob/master/examples/examples.py)




