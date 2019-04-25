# OpenApi接口文档

## 通用规则

#### 1. 签名: 
请求参数按照字典排序，然后以keyvalue的形式拼接成字符串string，最后sign=MD5(string+secretKey)。注意:如果请求参数中value为NUL L的情况，则在拼接字符串时不计入签名字符串。
例如:
参数如下:
```
{
'symbol': 'ltcbtc',
'api_key': '0816016bb06417f50327e2b557d39aaa',
'sign': 'a169740d0588141ef70b71cf11ff8bf3',
'time': '1522055680'
}
```
拼接完成后:
```
string=api_key0816016bb06417f50327e2b557d39aaasymbolltcbtctime1522055680
sign = MD5(string+secretKey) = MD5(api_key0816016bb06417f50327e2b557d39aaasymbolltcbtctime1522055680xxxxxxxxxxxxxxxxx) 
```
最终:
```
sign=48105baf432a6626051a72fb160b617e
```

#### 2. post请求参数采用表单格式提交数据
```
content-type:application/x-www-form-urlencoded
```

#### 3. 错误码
```
0 成功  

5 下单失败  

6 数量小于最小值 

7 数量大于最大值 

8 订单取消失败  

9 交易被冻结 

13  对不起,程序出现系统错误,请和网站管理员联系  

19  可用余额不足  

22  订单不存在 

23  缺少交易数量参数  

24  缺少交易价格参数  

100001  系统异常  

100002  系统升级  

100004  请求参数不合法 

100005  参数签名错误  

100007  非法IP  
```

## 服务器地址

#### Host
https://apiv3.upex.io/exchange-open-api/open/api


## 接口说明

### **GET** - /common/symbols 查询系统支持的所有交易对及精度

#### 参数
无

#### 返回
字段|实例|解释
--|--|--
code|0|code>0失败
msg|"suc"|
data|json结构|symbol 交易对<br/>base_coin 基础币种<br/>count_coin 计价货币<br/>price_precision 价格精度位数(0为个位)<br/>amount_precision 数量精度位数(0为个位)

json返回实例:
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

### **GET** - /get_allticker 获取所有交易对行情

#### 参数
无

#### 返回
字段|实例|解释
--|--|--
code|0|code>0失败
msg|"suc"|
data|json结构|date: 返回数据时服务器时间<br/>symbol: 交易对(交易对1(base)简称_交易对2(quote)简称)<br/>buy: 买一价<br/>high: 最高价<br/>last: 最新成交价<br/>low: 最低价<br/>sell: 卖一价<br/>vol: 成交量(最近的24小时)<br/>rose:涨跌幅

json返回实例:
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

### **GET** - /get_ticker 获取当前行情

#### 参数
参数|填写类型|说明
--|--|--
symbol|必填|交易对，例如：btcusdt

#### 返回
字段|实例|解释
--|--|--
code|0|code>0失败
msg|"suc"|
data|json结构|

json返回实例:
```
{
  "high": 1, // 最高值
  "vol": 10232.26315789, // 交易量
  "last": 173.60263169, // 最新成交价
  "low": 0.01, // 最低值
  "buy": "0.01000000", // 买1
  "sell": "1.12345680", // 卖1
  "rose": -0.44564773, // 涨跌幅
  "time": 1514448473626
}
```

#### CURL

```sh
curl -X GET "https://apiv3.upex.io/exchange-open-api/open/api/get_ticker?symbol=btcusdt"
```

---

### **GET** - /get_trades 获取行情成交记录

#### 参数
参数|填写类型|说明
--|--|--
symbol|必填|交易对，例如：btcusdt

#### 返回
字段|实例|解释
--|--|--
code|0|code>0失败
msg|"suc"|
data|json结构|

json返回实例:
```
[
  {
    "amount": 0.55, // 成交量
    "price": 0.18519949, // 成交价
    "id": 447121,
    "type": "buy" // 买卖type，买为buy，买sell
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

### **GET** - /get_records 获取K线数据

#### 参数
参数|填写类型|说明
--|--|--
symbol|必填|交易对，例如：btcusdt
period|必填|单位为分钟，比如1分钟则为1，一天则为1440

#### 返回
字段|实例|解释
--|--|--
code|0|code>0失败
msg|"suc"|
data|json结构|

json返回实例:
```
{
  "data": [
    [
      1514445780, // timestamp
      1.12, // 开盘价
      1.12, // 最高
      1.12, // 最低
      1.12, // 收盘
      0 // 成交量
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

### **GET** - /market_dept 查询买卖盘深度

#### 参数
参数|填写类型|说明
--|--|--
symbol|必填|交易对，例如：btcusdt
type|必填|深度类型，step0, step1, step2(合并深度0-2);step0时，精度最高

#### 返回
字段|实例|解释
--|--|--
code|0|code>0失败
msg|"suc"|
data|json结构|

json返回实例:
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

### **GET** - /market 获取各个币对的最新成交价

#### 参数
参数|填写类型|说明
--|--|--
api_key|必填|api_key
time|必填|时间戳
sign|必填|签名

#### 返回
字段|实例|解释
--|--|--
code|0|code>0失败
msg|"suc"|
data|`{"btcusdt":15000,"ethusdt":800}`|

#### CURL

```sh
curl -X GET "https://apiv3.upex.io/exchange-open-api/open/api/market?api_key=[YourApiKey]&time=1543644542&sign=[YourSignResult]"
```

---

### **GET** - /all_trade 获取全部成交记录

#### 参数
参数|填写类型|说明
--|--|--
symbol|必填|交易对，例如：btcusdt
pageSize|选填|页面大小
page|选填|页码
api_key|必填|api_key
time|必填|时间戳
sort|选填|1表示倒序
sign|必填|签名

#### 返回
字段|实例|解释
--|--|--
code|0|code>0失败
msg|"suc"|
data|json结构|

json返回实例:
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

### **GET** - /v2/all_order 获取全部委托

#### 参数
参数|填写类型|说明
--|--|--
symbol|必填|交易对，例如：btcusdt
pageSize|选填|页面大小
page|选填|页码
api_key|必填|api_key
time|必填|时间戳
sign|必填|签名

#### 返回
字段|实例|解释
--|--|--
code|0|code>0失败
msg|"suc"|
data|json结构|

json返回实例:
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

### **GET** - /v2/new_order 获取当前委托

#### 参数
参数|填写类型|说明
--|--|--
symbol|必填|交易对，例如：btcusdt
pageSize|选填|页面大小
page|选填|页码
api_key|必填|api_key
time|必填|时间戳
sign|必填|签名

#### 返回
字段|实例|解释
--|--|--
code|0|code>0失败
msg|"suc"|
data|json结构|订单状态(status)说明:<br/>INIT(0,"初始订单，未成交未进入盘口"),<br/>NEW_(1,"新订单，未成交进入盘口")<br/>FILLED(2,"完全成交")<br/>PART_FILLED(3,"部分成交")<br/>CANCELED(4,"已撤单")<br/>PENDING_CANCEL(5,"待撤单")<br/>EXPIRED(6,"异常订单")

json返回实例:
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

### **GET** - /order_info 获取订单详情

#### 参数
参数|填写类型|说明
--|--|--
order_id|必填|订单ID
symbol|必填|交易对，例如：btcusdt
api_key|必填|api_key
time|必填|时间戳
sign|必填|签名

#### 返回
字段|实例|解释
--|--|--
code|0|code>0失败
msg|"suc"|
data|json结构|`{“order_info”:{},"trade_list":[]}`<br/><br/>订单状态(status)说明:<br/>INIT(0,"初始订单，未成交未进入盘口"),<br/>NEW_(1,"新订单，未成交进入盘口")<br/>FILLED(2,"完全成交")<br/>PART_FILLED(3,"部分成交")<br/>CANCELED(4,"已撤单")<br/>PENDING_CANCEL(5,"待撤单")<br/>EXPIRED(6,"异常订单")

json返回实例:
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

### **GET** - /user/account 资产余额

#### 参数
参数|填写类型|说明
--|--|--
api_key|必填|api_key
time|必填|时间戳
sign|必填|签名

#### 返回
字段|实例|解释
--|--|--
code|0|code>0失败
msg|"suc"|
data|json结构|total_asset:总资产<br/>normal:余额账户<br/>locked:冻结账户 <br/>btcValuatin:BTC估值

json返回实例:
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

### **POST** - /create_order 创建订单

#### 参数
参数|填写类型|说明
--|--|--
side|必填|买卖方向BUY、SELL
type|必填|挂单类型，1:限价委托、2:市价委托
volume|必填|购买数量(多义，复用字段)<br/>type=1:表示买卖数量<br/>type=2:买则表示总价格，卖表示总个数<br/>买卖限制user/me-用户信息
price|选填|委托单价:type=2:不需要此参数
symbol|必填|市场标记, btcusdt
~~fee_is_user_exchange_coin~~|~~选填~~|~~0，当交易所有平台币时，此参数表示是否使用用平台币支付手续费，0否，1是~~
api_key|必填|api_key
time|必填|时间戳
time|必填|签名

#### 返回
字段|实例|解释
--|--|--
code|0|code>0失败
msg|"suc"|
data|`{"order_id":34343}`|成功返回交易ID

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

### **POST** - /cancel_order 取消委托单

#### 参数
参数|填写类型|说明
--|--|--
order_id|必填|订单ID
symbol|必填|交易对，例如：btcusdt
api_key|必填|api_key
time|必填|时间戳
sign|必填|签名

#### 返回
字段|实例|解释
--|--|--
code|0|code>0失败
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

### **POST** - /cancel_order_all 根据币对取消全部委托单(最多取消两千条 ，多余两千请循环撤销)

#### 参数
参数|填写类型|说明
--|--|--
symbol|必填|交易对，例如：btcusdt
api_key|必填|api_key
time|必填|时间戳
sign|必填|签名

#### 返回
字段|实例|解释
--|--|--
code|0|code>0失败
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