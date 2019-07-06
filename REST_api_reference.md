# API Reference #
所有接口的调用不论成功或失败，其响应数据格式为

```js
{
  resultCode: // ok为成功，其他错误代码详见REST_error_code,
  resultMessage: //错误信息
  data: //结果，为object或者array格式
}
```
下面的响应数据说明均只对`data`字段的数据结构进行描述。

# 行情API #

- **GET /api/v2/exchangeInfo 获取服务器时间**


响应数据说明：
```
{
    "serverTime": 服务器当前时间，精确到毫秒
}
```
- **GET /api/v2/market/config 获取交易配置**

请求参数：

参数名称|是否必须|类型|描述
---|---|---|---
symbol|true|String|交易对（例:BTC_USDT）

响应数据说明：
```
{
    "symbol": 交易对（例:BTC_USDT）,
    "baseAsset": 基础币种,
    "quoteAsset": 计价币种,
    "priceDecimal": 价格最大小数位数,
    "quantityDecimal": 数量最大小数位数,
    "minAmount": 最小下单金额
}
```
- **GET /api/v2/market/configAll 获取所有交易对的配置**


响应数据说明：
```
[{
    "symbol": 交易对（例:BTC_USDT）,
    "baseAsset": 基础币种,
    "quoteAsset": 计价币种,
    "priceDecimal": 价格最大小数位数,
    "quantityDecimal": 数量最大小数位数,
    "minAmount": 最小下单金额
}]
```    
- **GET /api/v2/market/tickers 获取交易对行情**

请求参数：

参数名称|是否必须|类型|描述
---|---|---|---
symbol|false|String|交易对（例：BTC_USDT）

响应数据说明：
```
[{
    "symbol": 交易对,
    "baseAsset": 基础币种,
    "quoteAsset": 计价币种,
    "priceChange": 24小时价格变化量,
    "priceChangePercent": 24小时价格变化百分比,
    "high": 最高价,
    "low": 最低价,
    "open": 开盘价,
    "latest": 最新成交价,
    "volume": 24小时成交量,
    "amount": 24小时成交额
}]
```    
- **GET /api/v2/market/kline 获取K线数据**

请求参数：

参数名称|是否必须|类型|描述|默认值|取值范围
---|---|---|---|---|---
symbol|true|String|交易对
period|true|String|K线类型||1m、5m、15m、30m、1h、2h、4h、6h、12h、1d、1w
size|false|Number|获取数量|150|1-1000

响应数据说明：
```
[
    [时间(ms),开盘价,最高价,最低价,收盘价,成交量 ],
    ...
]
```    
- **GET /api/v2/market/trades 获取最新成交数据**

请求参数：

参数名称|是否必须|类型|描述
---|---|---|---
symbol|true|String|交易对

响应数据说明：
```
[{
    "takerSide": taker方：buy买，sell卖,
    "price": 成交价格,
    "quantity": 成交数量,
    "amount": 成交金额,
    "tradeTime": 成交时间（格式：HH:mm:ss）,
    "tradeFullTime": 成交时间（格式：yyyy-MM-dd HH:mm:ss）
}]
```
- **GET /api/v2/market/depth 获取买卖盘数据**

请求参数：

参数名称|是否必须|类型|描述
---|---|---|---
symbol|true|String|交易对

响应数据说明：
```
{
    "buyOne": 最高买单的价格
    "sellOne": 最低卖单的价格
    "buyVol": 最高买单的量
    "sellVol": 最低卖单的量
}
```
    
# 交易API #

- **POST /api/v2/trade/placeOrder 下单**

请求参数：

参数名称|是否必须|类型|描述
---|---|---|---
symbol|true|String|交易对
side|true|String|buy：买入，sell：卖出
orderType|true|String|交易类型，limit限价
quantity|true|Number|数量
price|true|Number|价格

响应数据说明：
```
orderId
```
- **GET /api/v2/trade/cancelOrder 撤单**

请求参数：

参数名称|是否必须|类型|描述
---|---|---|---
orderId|true|String|订单ID号

- **GET /api/v2/trade/getOpenOrders 获取当前委托**

请求参数：

参数名称|是否必须|类型|描述
---|---|---|---
symbol|false|String|交易对

响应数据说明：
```
[{
    "orderId": 订单ID号,
    "baseAsset": 基础币种,
    "quoteAsset": 计价币种,
    "side": sell：卖出，buy：买入,
    "price": 委托价格,
    "quantity": 委托数量,
    "filledQuantity": 已成交数量,
    "filledAmount": 成交金额,
    "filledAvgPrice": 成交均价,
    "tradeCount": 成交次数,
    "orderType": 交易类型，limit：限价,
    "orderTime": 委托时间（格式：yyyy-MM-dd HH:mm:ss）
}]
 ```   
 - **GET /api/v2/trade/getAccounts 获取账户**

请求参数：

参数名称|是否必须|类型|描述
---|---|---|---
symbol|false|String|交易对可选，如果不为空，将只返回和这个交易对的相关的两个账户；否则返回所有的账户

响应数据说明：
```
[{
    "assetCode": 币种,
    "balance": 总余额,
    "frozenBalance": 冻结,
    "availableBalance": 可用余额
}]
 ```   
 
- **GET /api/v2/trade/get24hHistoryOrders 获取24h历史委托**

请求参数：

参数名称|是否必须|类型|描述
---|---|---|---
symbol|false|String|交易对

响应数据说明：
```
[{
    "orderId": 订单ID号,
    "baseAsset": 基础币种,
    "quoteAsset": 计价币种,
    "side": sell：卖出，buy：买入,
    "price": 委托价格,
    "quantity": 委托数量,
    "filledQuantity": 已成交数量,
    "filledAmount": 成交金额,
    "filledAvgPrice": 成交均价,
    "tradeCount": 交易次数,
    "orderType": 交易类型，limit：限价,
    "orderTime":  委托时间（格式：yyyy-MM-dd HH:mm:ss）,
    "lastFilledTime": 最后成交时间（格式：yyyy-MM-dd HH:mm:ss）,
}]
```   
- **GET /api/v2/market/getOrderDetails 获取交易详情**

请求参数：

参数名称|是否必须|类型|描述
---|---|---|---
orderId|true|String|订单号

响应数据说明：
```
[{
    "orderId": 订单号,
    "baseAsset": 基础币种,
    "quoteAsset": 计价币种,
    "takerSide": taker(买方buy，卖方sell),
    "price": 价格,
    "quantity": 数量,
    "amount": 金额,
    "tradeTime": 成交时间（格式：yyyy-MM-dd HH:mm:ss）,
    "finalFee": 实际手续费,
    "finalFeeAsset": 手续费币种,
    "side": 买入buy，卖出sell
}]
```
- **GET /api/v2/trade/getAccounts 获取账户信息**

请求参数：

参数名称|是否必须|类型|描述
---|---|---|---
symbol|false|String|交易对

响应数据说明：
```
[{
    "balance": 账户总额,
    "frozenBalance": 冻结金额,
    "assetCode": 币种,
    "estimateBtcValue": BTC估值,
    "availableBalance": 可用余额
}]
```  
- **GET /api/v2/trade/listenerKey 获取WebSocket Listener Key**

请求参数：

参数名称|是否必须|类型|描述
---|---|---|---
accessKey|true|String|api accessKey
sign|true|String|[HmacSHA256签名](https://github.com/aacoinapi/API_Docs/blob/master/REST_authentication.md)

响应数据：
```
listenerKey
```




