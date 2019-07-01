# API Reference #
# 行情API #
- **/api/v2/market/groups 获取交易分组**

响应数据说明：

    {
    	"resultCode": "ok",
    	"data": [{
        	"groupCode": 分组代码,
        	"enabled": 启用状态,
        	"groupName": 分组名称
    	}]
    }

- **/api/v2/market/config 获取交易配置**

请求参数：

参数名称|是否必须|类型|描述
---|---|---|---
symbol|true|String|交易对（例:BTC_USDT）

响应数据说明：

    {
    	"resultCode": "ok",
    	"data": {
            "symbol": 交易对（例:BTC_USDT）,
        	"baseAsset": 基础币种,
        	"quoteAsset": 计价币种,
        	"priceDecimal": 价格最大小数位数,
        	"quantityDecimal": 数量最大小数位数,
        	"minDepthDecimal": 最小深度小数位数,
        	"minAmount": 最小交易金额
    	}
    }
    
- **/api/v2/market/assets 获取币种**

响应数据说明：

    {
    	"resultCode": "ok",
    	"data": [{
    		"assetCode": 币种代码,
    		"assetName": 币种名称,
    		"enabled": 是否可用,
    		"withdrawEnabled": 是否可提现,
    		"depositEnabled": 是否可充值
    	}]
    }
    
- **/api/v2/market/assetDetail 获取币种详情**

请求参数：

参数名称|是否必须|类型|描述
---|---|---|---
assetCode|true|String|币种（例：BTC）

响应数据说明：

    {
    	"resultCode": "ok",
    	"data": {	
        	"assetCode": 币种代码,
        	"assetName": 币种名称,
        	"blockExplorerUrl": 区块浏览器,
        	"officialSite": 官方网站,
        	"whitePaperUrl": 白皮书,
        	"issuePrice": 发行价格,
        	"issueAmount": 发行量,
        	"circulationAmount": 流通量,
        	"description": 币种描述
    	}
    }
    
- **/api/v2/market/tickers 获取交易对行情**

请求参数：

参数名称|是否必须|类型|描述
---|---|---|---
symbol|false|String|交易对（例：BTC_USDT）

响应数据说明：

    {
    	"resultCode": "ok",
    	"data": [{
    		"symbol": 交易对,
        	"baseAsset": 基础币种,
        	"quoteAsset": 计价币种,
        	"marketGroup": 交易组,
        	"priceChange": 24小时价格变化量,
        	"priceChangePercent": 24小时价格变化百分比,
        	"high": 最高价,
        	"low": 最低价,
        	"open": 开盘价,
        	"latest": 最新成交价,
        	"usdtValue": USDT价值,
        	"volume": 24小时成交量,
        	"amount": 24小时成交额
    	}]
    }
    
- **/api/v2/market/recent24hTrend 获取24小时价格趋势**

响应数据说明：

    {
    	"resultCode": "ok",
    	"data": {
    		"ETH_BTC": ["0.000000", "0.000000", "0.000000", "0.000000", "0.000000", "0.000000", "0.000000", "0.000000", "0.000000", "0.000000", "0.000000", "0.000000", "0.000000", "0.190000", "0.000000", "0.000000", "0.000000", "0.000000", "0.000000", "0.000000", "0.000000", "0.000000", "0.000000", "0.000000"],
            交易对2: 价格趋势2,
            ...
        }
    }
    
- **/api/v2/market/kline 获取K线数据**

请求参数：

参数名称|是否必须|类型|描述|默认值|取值范围
---|---|---|---|---|---
symbol|true|String|交易对
period|true|String|K线类型||1m、5m、15m、30m、1h、2h、4h、6h、12h、1d、1w
size|false|Number|获取数量|150|1-1000

响应数据说明：

    {
    	"resultCode": "ok",
    	"data": [
    		[时间(ms),开盘价,最高价,最低价,收盘价,成交量 ],
    		...
    	]
    }
    
- **/api/v2/market/trades 获取交易数据**

请求参数：

参数名称|是否必须|类型|描述
---|---|---|---
symbol|true|String|交易对

响应数据说明：

    {
    	"resultCode": "ok",
    	"data": [{
    		"takerSide": 成交类型：buy买，sell卖,
        	"price": 成交价格,
        	"quantity": 成交数量,
        	"amount": 成交金额,
        	"tradeTime": 成交时间（格式：HH:mm:ss）,
        	"tradeFullTime": 成交时间（格式：yyyy-MM-dd HH:mm:ss）
    	}, {
    	    ...
    	}]
    }
    
- **/api/v2/market/currencyRates 获取汇率**

响应数据说明：

    {
    	"resultCode": "ok",
    	"data": {
        	"usdt2cny": USDT兑换CNY的汇率,
        	"usdt2usd": USDT兑换USD的汇率
    	}
    }
    
# 交易API #

- **/api/v2/trade/placeOrder 下单**

请求参数：

参数名称|是否必须|类型|描述
---|---|---|---
symbol|true|String|交易对
side|true|String|buy：买入，sell：卖出
orderType|true|String|交易类型，limit限价
quantity|true|Number|数量
price|true|Number|价格

响应数据说明：订单ID号

- **/api/v2/trade/cancelOrder 撤单**

请求参数：

参数名称|是否必须|类型|描述
---|---|---|---
orderId|true|String|订单ID号

- **/api/v2/trade/getOpenOrders 获取当前委托**

请求参数：

参数名称|是否必须|类型|描述
---|---|---|---
symbol|false|String|交易对

响应数据说明：

    {
    	"resultCode": "ok",
    	"data": [{
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
            "orderTime": 委托时间（格式：yyyy-MM-dd HH:mm:ss）
    	}]
    }
    
- **/api/v2/trade/get24hHistoryOrders 获取24h历史委托**

请求参数：

参数名称|是否必须|类型|描述
---|---|---|---
symbol|false|String|交易对

响应数据说明：

    {
    	"resultCode": "ok",
    	"data": [{
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
    }
    
- **/api/v2/trade/getAccounts 获取账户信息**

请求参数：

参数名称|是否必须|类型|描述
---|---|---|---
symbol|false|String|交易对

响应数据说明：

    {
    	"resultCode": "ok",
    	"data": [{
            "balance": 账户总额,
            "frozenBalance": 冻结金额,
            "assetCode": 币种,
            "estimateBtcValue": BTC估值,
            "availableBalance": 可用余额
    	}]
    }
    
- **/api/v2/trade/listenerKey 获取WebSocket Listener Key**

请求参数：

参数名称|是否必须|类型|描述
---|---|---|---
accessKey|true|String|api accessKey
sign|true|String|[HmacSHA256签名](https://github.com/aacoinapi/API_Docs/blob/master/REST_authentication.md)

响应数据：

    {"resultCode":"ok","data": listenerKey}





