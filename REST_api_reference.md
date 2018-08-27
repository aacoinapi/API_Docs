
# API Reference#

所有请求响应数据：

参数名称|是否必须|数据类型|描述
---|---|---|---
status|true|String|状态值
msg|false|string|提示信息
data|false|Object|数据
----------

# 行情API #
- **/market/kline 获取K线数据**

请求参数:


参数名称|是否必须|类型|描述|默认值|取值范围
---|---|---|---|---|---
symbol|true|String|交易市场||
period|true|String|K线类型||1m、5m、15m、60m、1d、1w
size|false|Number|获取数量|150|1~1000


响应数据中的data说明：

	[
	    [
	        时间(ms),
	        开盘价,
	        最高价,
	        最低价,
	        收盘价,
	        成交量
	    ]
	]

- **/market/detail 获取市场详情**

请求参数:

参数名称|是否必须|类型|描述
---|---|---|--
symbol|true|String|交易市场

响应数据中的data说明：

	{
	    "symbol": "交易市场",
	    "highest": "24小时最高价",
	    "lowest": "24小时最低价",
	    "current": "最新价",
	    "totalTradeAmount": "24小时成交量",
	    "price24HourBefore": 24小时前的开盘价,
	    "time": "时间（例：2018-03-04 16:21:24）"
	}


- **/market/tickers 获取所有行情数据**

响应数据中的data说明：

	{
	    "ticker": [{
			symbol: 交易对（交易对1简称_交易对2简称） 
			buy: 买一价 
			high: 最高价 
			last: 最新成交价 
			low: 最低价 
			sell: 卖一价 
			vol: 成交量(最近的24小时)
		}],
	    "date": 时间（例：2018-03-04 16:21:24）
	}


- **/market/depth 获取 Market Depth 数据**

请求参数:


参数名称|是否必须|类型|描述|默认值|取值范围
---|---|---|---|---|---
symbol|true|String|交易市场||

响应数据中的data说明：

	{
	    "bids": 买盘,[price(成交价), amount(成交量)],
	    "asks": 卖盘,[price(成交价), amount(成交量)]
	}


- **/market/trade 获取 Trade Detail 数据**

请求参数:


参数名称|是否必须|类型|描述|默认值|取值范围
---|---|---|---|---|---
symbol|true|String|交易市场||

响应数据中的data说明：

	[
	    {
	        "quantity": "成交数量",
	        "price": "成交价",
	        "transactionTime": "成交时间（格式：HH:mm:ss）",
	        "mainMarket": "成交类型：buy买，sell卖".
		"fullTransactionTime": "成交时间（格式：yyyy-MM-dd HH:mm:ss）"
	    }
	]

# 公共API #
- **/common/symbols 获取所有交易市场**

响应数据中的data说明：

    [
    	{
    	"baseCurrency": "基础币种",
    	"quoteCurrency": "计价币种",
    	"priceDecimal": 价格的最大小数位数,
    	"quantityDecimal": 数量的最大小数位数,
    	"symbolName": "交易市场名称"
    	}
    ]



- **/common/currencies 获取所有币种**

响应数据中的data说明：

    ["币种代码"]



- **/common/timestamp 获取系统当前时间戳**

响应数据中的data说明：响应数据为Number类型


# 用户资产API #
- **/account/accounts 获取所有的账户信息**


响应数据中的data说明：

    [
	    {
		    "currencyCode": "币种代码",
		    "accounts": [
			    {
				    "accountId": 账户ID,
				    "balance": 余额,
				    "type": "账户类型（trade：交易账户；frozen：冻结账户）"
			    }
		    ]
	    }
    ]


- **/account/statement 获取账户操作日志**

请求参数:

参数名称|是否必须|类型|描述
---|---|---|---
accountId|true|Number|账户ID
limit|false|Number|显示数量（取值范围为0~100）


响应数据中的data说明：

    [
	    {
		    "time": "时间（格式：yyyy-MM-dd HH:mm:ss）",
		    "event": "取值范围：提现、充值、交易、挂单、撤单、分发",
		    "amount": 本次操作金额,
		    "afterBalance": 本次操作之后的金额
	    }
    ]


# 交易API #
- **/order/place 下单**

请求参数:

参数名称|是否必须|类型|描述
---|---|---|---
symbol|true|String|交易市场（例：BCC_ETH）
type|true|String|交易类型（buy-limit：限价买入；buy-market：市价买入；sell-limit：限价卖出；sell-market：市价卖出）
quantity|true|Number|数量
price|false|Number|价格


响应数据中的data说明：订单ID号


- **/order/detail 获取订单详情**

请求参数:

参数名称|是否必须|类型|描述
---|---|---|---
orderId|true|String|订单ID号
响应数据：
```
{
    "orderId": 订单ID号,
    "symbol": "交易市场",
    "price": 委托价格,
    "type": "sell：卖出；buy：买入",
    "priceType": "limit：限价；market：市价",
    "orderTime": "委托时间（格式：yyyy-MM-dd HH:mm:ss）",
    "filledQuantity": 已成交数量,
    "quantity": 委托数量,
    "status": "cancelling：等待取消",
    "filledAmount": 已成交金额
}
```

- **/order/cancel 撤单**

请求参数:

参数名称|是否必须|类型|描述
---|---|---|---
orderId|true|String|订单ID号


- **/order/batchCancel 批量撤单**

请求参数:

参数名称|是否必须|类型|描述
---|---|---|---
orderIds|true|String|多个订单ID号（以,隔开）

响应数据中的data说明：

    {
    	failed: [
    		{
    			orderId："订单ID号",
    			code:"错误代码",
    			msg:"错误提示"
     		}
    	],
    	success: [
    		{
    			orderId："订单ID号",
    			code:"提示代码"
     		}
    	]
    }


- **/order/currentOrders 获取当前委托订单**

请求参数:

参数名称|是否必须|类型|描述
---|---|---|---
symbol|true|String|交易市场（例：BCC_ETH）
type|false|String|交易类型（buy-limit：限价买入；buy-market：市价买入；sell-limit：限价卖出；sell-market：市价卖出）
page|false|Number|页数（默认1）
size|false|Number|每页条数（默认10）

响应数据中的data说明：
	
	{
	    "pageNo": 当前页数,
	    "pageSize": 每页条数,
	    "total": 总数,
	    "list": [
	        {
	            "orderId": 订单ID号,
	            "symbol": "交易市场",
	            "price": 委托价格,
	            "type": "sell：卖出；buy：买入",
	            "priceType": "limit：限价；market：市价",
	            "orderTime": "委托时间（格式：yyyy-MM-dd HH:mm:ss）",
	            "filledQuantity": 已成交数量,
	            "quantity": 委托数量,
	            "status": "cancelling：等待取消",
	            "filledAmount": 已成交金额
	        }
	    ]
	}


- **/order/historyOrders 获取历史委托订单**

请求参数:

参数名称|是否必须|类型|描述
---|---|---|---
symbol|true|String|交易市场（例：BCC_ETH）
type|false|String|交易类型（buy-limit：限价买入；buy-market：市价买入；sell-limit：限价卖出；sell-market：市价卖出）
page|false|Number|页数（默认1）
size|false|Number|每页条数（默认10）

响应数据中的data说明：
	
	{
	    "pageNo": 当前页数,
	    "pageSize": 每页条数,
	    "total": 总数,
	    "list": [
	        {
	            "orderId": 订单ID号,
	            "symbol": "交易市场",
	            "price": 委托价格,
	            "type": "sell：卖出；buy：买入",
	            "priceType": "limit：限价；market：市价",
	            "orderTime": "委托时间（格式：yyyy-MM-dd HH:mm:ss）",
	            "filledQuantity": 已成交数量,
	            "quantity": 委托数量,
	            "status": "cancelling：等待取消",
	            "filledAmount": 已成交金额
	        }
	    ]
	}


- **/order/trades 获取成交记录**

请求参数:

参数名称|是否必须|类型|描述
---|---|---|---
symbol|true|String|交易对（例：BCC_ETH）
page|false|Number|页数（默认1）
size|false|Number|每页条数（默认10）

响应数据中的data说明：
	
	{
	    "pageNo": 当前页数,
	    "pageSize": 每页条数,
	    "total": 总数,
	    "list": [
	        {
	            "tradeId": 订单ID号,
	            "tradeTime": "交易时间（格式：yyyy-MM-dd HH:mm:ss）",
	            "tradeQuantity": 成交数量,
	            "fee": 手续费,
	            "amount": 成交金额,
	            "price": 成交价格,
	            "symbol": "交易市场"
	        }
	    ]
	}
