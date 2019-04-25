# OpenAPI Reference

## API Common Illustration

#### 1. Signature: 
The request params would be recognized as the dict, then you need to construct the canonical query string recognized as ‘keyvalue’, finally calculate the signature: sign=MD5(string+secretKey). Attention: if the ‘value’ is NULL, the sign should not contain it.
For example:
Request params as following:
```
{
'symbol': 'ltcbtc',
'api_key': '0816016bb06417f50327e2b557d39aaa',
'sign': 'a169740d0588141ef70b71cf11ff8bf3',
'time': '1522055680'
}
```
Construct canonical query string:
```
string=api_key0816016bb06417f50327e2b557d39aaasymbolltcbtctime1522055680
sign = MD5(string+secretKey) = MD5(api_key0816016bb06417f50327e2b557d39aaasymbolltcbtctime1522055680xxxxxxxxxxxxxxxxx) 
```
Finally:
```
sign=48105baf432a6626051a72fb160b617e
```

#### 2.  The field 'Content-Type' in request header should be 'application/x-www-form-urlencoded' for a POST method.
```
content-type:application/x-www-form-urlencoded
```

#### 3. Error Code
```
0 Success  
5 OrderFail 
6 LessLimitMin 
7 GreaterLimitMax 
8 OrderCancleFail  
9 TradeBeLock 
13  PleaseContactAdministrator  
19  BALANCE_NOT_ENOUGH  
22  ORDER_NOT_EXIST 
23  PARAMETER_VOLUME_NOT_EXIST  
24  PARAMETER_PRICE_NOT_EXIST  
100001  SYS_ERROR  
100002  SYS_UPDATE  
100004  PARAMETER_ILLEGAL 
100005  PARAMETER_SIGN_ILLEGAL  
100007  IP_ILLEGAL
```

## Server Address

#### Host
https://apiv3.upex.io/exchange-open-api/open/api


## API Reference

### **GET** - /common/symbols get reference information of trading instrument, including base currency, quote precision, etc.

#### Request
None

#### Response
Param|Example|Description
--|--|--
code|0|code>0Fail
msg|"suc"|
data|json|symbol<br/>base_coin<br/>count_coin<br/>price_precision<br/>amount_precision

Response Example:
```
{
  "code": "0",
  "msg": "suc",
  "data": [
    {
      "symbol": "ethbtc",
      "count_coin": "btc",
      "amount_precision": 3,
      "base_coin": "eth",
      "price_precision": 8
    },
    {
      "symbol": "ltcbtc",
      "count_coin": "btc",
      "amount_precision": 2,
      "base_coin": "ltc",
      "price_precision": 8
    },
    {
      "symbol": "bchbtc",
      "count_coin": "btc",
      "amount_precision": 3,
      "base_coin": "bch",
      "price_precision": 8
    },
    {
      "symbol": "etcbtc",
      "count_coin": "btc",
      "amount_precision": 2,
      "base_coin": "etc",
      "price_precision": 8
    },
    {
      "symbol": "ltceth",
      "count_coin": "eth",
      "amount_precision": 2,
      "base_coin": "ltc",
      "price_precision": 8
    },
    {
      "symbol": "etceth",
      "count_coin": "eth",
      "amount_precision": 2,
      "base_coin": "etc",
      "price_precision": 8
    }
  ]
}
```

#### CURL

```sh
curl -X GET "https://apiv3.upex.io/exchange-open-api/open/api/common/symbols"
```

---

### **GET** - /get_allticker get trade summary of a trading day for all symbols

#### Request
None

#### Response
Param|Example|Description
--|--|--
code|0|code>0 Fail
msg|"suc"|
data|json|date: Response timestamp of the service<br/>symbol: trading asset<br/>buy: ask1 price<br/>high: the maxmum price<br/>last: the current price<br/>low: the minimum price<br/>sell: bid1 price<br/>vol: trading amount(in 24 hours)<br/>rose:Change of the symbol

Response Example:
```
{
  "date": 1534335607859,
  "ticker": [
    {
      "symbol": "btcusdt",
      "high": 7408.35984546,
      "vol": 0.01,
      "last": 1,
      "low": 7408.35984546,
      "buy": "3700.00000000",
      "sell": "7408.35984546",
      "rose": 0
    },
    {
      "symbol": "ethusdt",
      "high": 535.96,
      "vol": 6366.8591,
      "last": 20,
      "low": 279.57,
      "rose": -0.44564773
    },
    {
      "symbol": "bchusdt",
      "high": 12,
      "vol": 100,
      "last": 1,
      "low": 12,
      "buy": "11.00",
      "sell": "9.00",
      "rose": 0
    },
    {
      "symbol": "ethbtc",
      "high": 1,
      "vol": 281261,
      "last": 0.1,
      "low": 0.044039,
      "buy": "0.044049",
      "sell": "0.044049",
      "rose": -0.00022701
    },
    {
      "symbol": "nobbtc",
      "high": 0.007419,
      "vol": 1998,
      "last": 1,
      "low": 0.007419,
      "sell": "0.00741900",
      "rose": 0
    },
    {
      "symbol": "ltceth",
      "last": 0.18519949,
      "buy": "1.00000000",
      "sell": "0.18328001"
    }
  ]
}
```

#### CURL

```sh
curl -X GET "https://apiv3.upex.io/exchange-open-api/open/api/get_allticker "
```

---

### **GET** - /get_ticker get trade summary of a trading day for a symbol

#### Request
Param|Required|Description
--|--|--
symbol|true|Symbol, eg: btcusdt

#### Response
Param|Example|Description
--|--|--
code|0|code>0 Fail
msg|"suc"|
data|json|

Response Example:
```
{
  "high": 1, // the maxmium price
  "vol": 10232.26315789, // trading amount
  "last": 173.60263169, // the current price
  "low": 0.01, // the minimum price
  "buy": "0.01000000", // ask1
  "sell": "1.12345680", // bid1
  "rose": -0.44564773, // Change of the symbol
  "time": 1514448473626
}
```

#### CURL

```sh
curl -X GET "https://apiv3.upex.io/exchange-open-api/open/api/get_ticker?symbol=btcusdt"
```

---

### **GET** - /get_trades get all trade records of a symbol

#### Request
Param|Required|Description
--|--|--
symbol|true|Symbol, eg: btcusdt

#### Response
Param|Example|Description
--|--|--
code|0|code>0 Fail
msg|"suc"|
data|json|

Response Example:
```
[
  {
    "amount": 0.55, // trading amount
    "price": 0.18519949, // deal price
    "id": 447121,
    "type": "buy" // type: buy or sell
  },
  {
    "amount": 16.45,
    "price": 0.18335468,
    "id": 447120,
    "type": "buy"
  },
  {
    "amount": 2,
    "price": 0.18335468,
    "id": 447119,
    "type": "buy"
  },
  {
    "amount": 2.92,
    "price": 0.183324003,
    "id": 447118,
    "type": "sell"
  }
]
```

#### CURL

```sh
curl -X GET "https://apiv3.upex.io/exchange-open-api/open/api/get_trades?symbol=btcusdt"
```

---

### **GET** - /get_records get K-Line records of a symbol

#### Request
Param|Required|Description
--|--|--
symbol|true|Symbol, eg: btcusdt
period|true|Uint: minute, eg: 1(1 minute), 1440(1 day)

#### Response
Param|Example|Description
--|--|--
code|0|code>0 Fail
msg|"suc"|
data|json|

Response Example:
```
{
  "data": [
    [
      1514445780, // timestamp
      1.12, // open
      1.12, // high
      1.12, // low
      1.12, // close
      0 // amount
    ],
    [
      1514445840,
      1.12,
      1.12,
      1.12,
      1.12,
      0
    ],
    [
      1514445900,
      1.12,
      1.12,
      1.12,
      1.12,
      0
    ]
  ]
}
```

#### CURL

```sh
curl -X GET "https://apiv3.upex.io/exchange-open-api/open/api/get_records?symbol=btcusdt&period=1"
```

---

### **GET** - /market_dept get market depth of a symbol

#### Request
Param|Required|Description
--|--|--
symbol|true|Symbol, eg: btcusdt
type|true|Depth data type: step0, step1, step2; step0 is the highest price precision

#### Response
Param|Example|Description
--|--|--
code|0|code>0 Fail
msg|"suc"|
data|json|

Response Example:
```
{
"tick": {
	"asks":[
    		    [22112.22,0.9332], 
    		    [22112.21,0.2], 
            [22112.21,0.2], 
            [22112.21,0.2], 
            [22112.21,0.2]],
	"bids":[
            [22111.22,0.9332],
            [22111.21,0.2],
            [22112.21,0.2],
            [22112.21,0.2],
            [22112.21,0.2]]
	}
}
```

#### CURL

```sh
curl -X GET "https://apiv3.upex.io/exchange-open-api/open/api/market_dept?symbol=btcusdt&type=step0"
```

---

### **GET** - /market get the current price of all symbols

#### Request
Param|Required|Description
--|--|--
api_key|true|api_key
time|true|timestamp
sign|true|api signture

#### Response
Param|Example|Description
--|--|--
code|0|code>0 Fail
msg|"suc"|
data|`{"btcusdt":15000,"ethusdt":800}`|

#### CURL

```sh
curl -X GET "https://apiv3.upex.io/exchange-open-api/open/api/market?api_key=[YourApiKey]&time=1543644542&sign=[YourSignResult]"
```

---

### **GET** - /all_trade get all trades of a symbol

#### Request
Param|Required|Description
--|--|--
symbol|true|Symbol, eg: btcusdt
pageSize|false|page size
page|false|page number
api_key|true|api_key
time|true|timestamp
sort|false|1 for descending
sign|true|api signture

#### Response
Param|Example|Description
--|--|--
code|0|code>0 Fail
msg|"suc"|
data|json|

Response Example:
```
{
  "count": 3,
  " resultList": [
    {
      "volume": "1.000",
      "side": "BUY",
      "feeCoin": "YLB",
      "price": "0.10000000",
      "fee": "0.16431104",
      "ctime": 1510996571195,
      "deal_price": "0.10000000",
      "id": 306,
      "type": "买入",
      "bid_user_id": 10001,
      "ask_user_id": 10001
    },
    {
      "volume": "0.850",
      "side": "BUY",
      "feeCoin": "YLB",
      "price": "0.10000000",
      "fee": "0.13966438",
      "ctime": 1510996571190,
      "deal_price": "0.08500000",
      "id": 305,
      "type": "买入",
      "bid_user_id": 10001,
      "ask_user_id": 10001
    },
    {
      "volume": "0.010",
      "side": "BUY",
      "feeCoin": "YLB",
      "price": "0.10000000",
      "fee": "0.00164311",
      "ctime": 1510995560344,
      "deal_price": "0.00100000",
      "id": 291,
      "type": "买入",
      "bid_user_id": 10001,
      "ask_user_id": 10001
    }
  ]
}
```

#### CURL

```sh
curl -X GET "https://apiv3.upex.io/exchange-open-api/open/api/all_trade?symbol=btcusdt&pageSize=10&page=1&api_key=[YourApiKey]&time=1543644542&sort=0&sign=[YourSignResult]"
```

---

### **GET** - /v2/all_order get all open orders of a symbol for an account

#### Request
Param|Required|Description
--|--|--
symbol|true|Symbol, eg: btcusdt
pageSize|false|page size
page|false|page number
api_key|true|api_key
time|true|timestamp
sign|true|api signture

#### Response
Param|Example|Description
--|--|--
code|0|code>0 Fail
msg|"suc"|
data|json|

Response Example:
```
{
  "count": 2,
  "orderList": [
    {
      "side": "BUY",
      "total_price": "0.10000000",
      "created_at": 1510993841000,
      "avg_price": "0.10000000",
      "countCoin": "btc",
      "source": 1,
      "type": 1,
      "side_msg": "买入",
      "volume": "1.000",
      "price": "0.10000000",
      "source_msg": "WEB",
      "status_msg": "完全成交",
      "deal_volume": "1.00000000",
      "id": 424,
      "remain_volume": "0.00000000",
      "baseCoin": "eth",
      "status": 2
    },
    {
      "side": "SELL",
      "total_price": "0.09900000",
      "created_at": 1510993715000,
      "avg_price": "0.10000000",
      "countCoin": "btc",
      "source": 1,
      "type": 1,
      "side_msg": "卖出",
      "volume": "1.000",
      "price": "0.09900000",
      "source_msg": "WEB",
      "status_msg": "完全成交",
      "deal_volume": "1.00000000",
      "id": 423,
      "remain_volume": "0.00000000",
      "baseCoin": "eth",
      "status": 2
    }
  ]
}
```

#### CURL

```sh
curl -X GET "https://apiv3.upex.io/exchange-open-api/open/api/v2/all_order?symbol=btcusdt&pageSize=10&page=1&api_key=[YourApiKey]&time=1543644542&sign=[YourSignResult]"
```

---

### **GET** - /v2/new_order get all current open orders of a symbol

#### Request
Param|Required|Description
--|--|--
symbol|true|Symbol, eg: btcusdt
pageSize|false|page size
page|false|page number
api_key|true|api_key
time|true|timestamp
sign|true|signture

#### Response
Param|Example|Description
--|--|--
code|0|code>0 Fail
msg|"suc"|
data|json|order status:<br/>INIT(0,"initial order")<br/>NEW_(1,"new order")<br/>FILLED(2,"Filled")<br/>PART_FILLED(3,"partial-filled")<br/>CANCELED(4,"canceled")<br/>PENDING_CANCEL(5,"wait-canceling")<br/>EXPIRED(6,"invalid")

Response Example:
```
{
  "count": 2,
  "resultList": [
    {
      "side": "BUY",
      "total_price": "0.10000000",
      "created_at": 1510993841000,
      "avg_price": "0.10000000",
      "countCoin": "btc",
      "source": 1,
      "type": 1,
      "side_msg": "买入",
      "volume": "1.000",
      "price": "0.10000000",
      "source_msg": "WEB",
      "status_msg": "完全成交",
      "deal_volume": "1.00000000",
      "id": 424,
      "remain_volume": "0.00000000",
      "baseCoin": "eth",
      "status": 2
    },
    {
      "side": "SELL",
      "total_price": "0.09900000",
      "created_at": 1510993715000,
      "avg_price": "0.10000000",
      "countCoin": "btc",
      "source": 1,
      "type": 1,
      "side_msg": "卖出",
      "volume": "1.000",
      "price": "0.09900000",
      "source_msg": "WEB",
      "status_msg": "完全成交",
      "deal_volume": "1.00000000",
      "id": 423,
      "remain_volume": "0.00000000",
      "baseCoin": "eth",
      "status": 2
    }
  ]
}
```

#### CURL

```sh
curl -X GET "https://apiv3.upex.io/exchange-open-api/open/api/v2/new_order?symbol=btcusdt&pageSize=10&page=1&api_key=[YourApiKey]&time=1543644542&sign=[YourSignResult]"
```

---

### **GET** - /order_info get the details of an order

#### Request
Param|Required|Description
--|--|--
order_id|true|order id
symbol|true|Symbol, eg: btcusdt
api_key|true|api_key
time|true|timestamp
sign|true|signture

#### Response
Param|Example|Description
--|--|--
code|0|code>0 Fail
msg|"suc"|
data|json|`{“order_info”:{},"trade_list":[]}`<br/><br/>order status:<br/>INIT(0,"initial order")<br/>NEW_(1,"new order")<br/>FILLED(2,"Filled")<br/>PART_FILLED(3,"partial-filled")<br/>CANCELED(4,"canceled")<br/>PENDING_CANCEL(5,"wait-canceling")<br/>EXPIRED(6,"invalid")

Response Example:
```
{
  "order_info": {
    "id":184880,
    "side":"BUY",
    "total_price":"340.00000000",
    "created_at":1544608016995,
    "avg_price":"0.00000000",
    "countCoin":"USDT",
    "source":3,"type":1,
    "side_msg":"买入",
    "volume":"0.10000000",
    "price":"3400.00000000",
    "source_msg":"API",
    "status_msg":"已撤单",
    "deal_volume":"0.00000000",
    "remain_volume":"0.10000000",
    "baseCoin":"BTC",
    "tradeList":[],
    "status":4
  },
  "trade_list": [
    {
      "id": 343,
      "created_at": "09-22 12:22",
      "price": 222.33,
      "volume": 222.33,
      "deal_price": 222.33,
      "fee": 222.33
    },
    {
      "id": 345,
      "created_at": "09-22 12:22",
      "price": 222.33,
      "volume": 222.33,
      "deal_price": 222.33,
      "fee": 222.33
    }
  ]
}
```

#### CURL

```sh
curl -X GET "https://apiv3.upex.io/exchange-open-api/open/api/order_info?order_id=184880&symbol=btcusdt&api_key=[YourApiKey]&time=1543644542&sign=[YourSignResult]"
```

---

### **GET** - /user/account get the balance of an account

#### Request
Param|Required|Description
--|--|--
api_key|true|api_key
time|true|timestamp
sign|true|signture

#### Response
Param|Example|Description
--|--|--
code|0|code>0 Fail
msg|"suc"|
data|json|

Response Example:
```
{
  "total_asset": 432323.23,
  "coin_list": [
    {
      "coin": "btc",
      "normal": 32323.233,
      "locked": 32323.233,
      "btcValuatin": 112.33
    },
    {
      "coin": "ltc",
      "normal": 32323.233,
      "locked": 32323.233,
      "btcValuatin": 112.33
    },
    {
      "coin": "bch",
      "normal": 32323.233,
      "locked": 32323.233,
      "btcValuatin": 112.33
    }
  ]
}
```

#### CURL

```sh
curl -X GET "https://apiv3.upex.io/exchange-open-api/open/api/user/account?api_key=[YourApiKey]&time=1543644542&sign=[YourSignResult]"
```

---

### **POST** - /create_order create an order

#### Request
Param|Required|Description
--|--|--
side|true|Side: BUY,SELL
type|true|Type，1:buy-limit or sell limit、2:buy-market or sell-market
volume|true|volume:<br/>type=1: Amount in buy-limit or sell-limitorder<br/>type=2: It presents how much quote-currency you trade in buy-market order, and how many base-currency you trade in sell-market order
price|false|The order price<br/>type=2:Param ‘price’ is not required
symbol|true|Symbol, eg: btcusdt
api_key|true|api_key
time|true|timestamp
time|true|signture

#### Response
Param|Example|Description
--|--|--
code|0|code>0 Fail
msg|"suc"|
data|`{"order_id":34343}`|return order id if success

#### CURL

```sh
curl -X POST "https://apiv3.upex.io/exchange-open-api/open/api/create_order" \
    -H "Content-Type: application/x-www-form-urlencoded; charset=utf-8" \
    --data-raw "api_key"="[YourApiKey]" \
    --data-raw "price"="3400" \
    --data-raw "side"="BUY" \
    --data-raw "symbol"="btcusdt" \
    --data-raw "time"="1544772142" \
    --data-raw "type"="1" \
    --data-raw "volume"="0.1" \
    --data-raw "sign"="[YourSignResult]"
```

---

### **POST** - /cancel_order request to cancel an order

#### Request
Param|Required|Description
--|--|--
order_id|true|order id
symbol|true|Symbol, eg: btcusdt
api_key|true|api_key
time|true|timestamp
sign|true|signture

#### Response
Param|Example|Description
--|--|--
code|0|code>0 Fail
msg|"suc"|
data||


#### CURL

```sh
curl -X POST "https://apiv3.upex.io/exchange-open-api/open/api/cancel_order" \
    -H "Content-Type: application/x-www-form-urlencoded; charset=utf-8" \
    --data-raw "api_key"="[YourApiKey]" \
    --data-raw "order_id"="184880" \
    --data-raw "symbol"="btcusdt" \
    --data-raw "time"="1544772142" \
    --data-raw "sign"="[YourSignResult]"
```

---

### **POST** - /cancel_order_all request to cancel a batch of orders by a symbol(up to 2000 orders)

#### Request
Param|Required|Description
--|--|--
symbol|true|Symbol, eg: btcusdt
api_key|true|api_key
time|true|timestamp
sign|true|signture

#### Response
Param|Example|Description
--|--|--
code|0|code>0 Fail
msg|"suc"|
data||


#### CURL

```sh
curl -X POST "https://apiv3.upex.io/exchange-open-api/open/api/cancel_order_all" \
    -H "Content-Type: application/x-www-form-urlencoded; charset=utf-8" \
    --data-raw "api_key"="[YourApiKey]" \
    --data-raw "symbol"="btcusdt" \
    --data-raw "time"="1544772142" \
    --data-raw "sign"="[YourSignResult]"
```