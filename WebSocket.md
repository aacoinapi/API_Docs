# WebSocket 数据 #

## 基本信息 ##

接入url: wss://aacoin.com/stream

成功建立与Websocket服务器的连接后，返回数据：

    {"topic":"info","data":"Welcome to AAcoin!"}
    

- **订阅主题**

Websocket客户端发送如下请求以订阅特定主题：

    {"cmd":"sub","args":[arg0, arg1, arg2]}
    
其中sub为开启订阅，arg0为订阅主题

- **取消订阅**

        {"cmd":"sub","args":[arg0, arg1, arg2]}
    
    
其中unsub为取消订阅命令

- **权限验证**

        {"cmd":"auth","args":[arg0, arg1, arg2]}

其中auth为权限验证命令

## K线数据 ##

- **订阅k线**

Websocket客户端发送如下请求以订阅k线主题：

    {"cmd":"sub","args":["kline","BTC_USDT","15m"]}
    
BTC_USDT为交易对，15m为k线时间周期，周期范围[1m、5m、15m、30m、1h、2h、4h、6h、12h、1d、1w]。
订阅成功后，将收到服务端推送数据：

    {
    	"topic": "kline",   //kline主题
    	"symbol": "BTC_USDT",   //交易对
    	"period": "15m",    //k线时间周期
    	"scope": "full",    //  full全数据，partial部分数据
    	"data": [
    		[时间戳, 开盘价格, 最高价格, 最低价格, 收盘价格, 交易量]
    	]
    }
开启订阅后，首次推送full全数据，后退只推送partial当前数据
    
- **取消订阅k线**

Websocket客户端发送如下请求以取消订阅k线主题：

    {"cmd":"unsub","args":["kline","BTC_USDT","15m"]}
    

## Ticker行情数据 ##

- **订阅**

Websocket客户端发送如下请求以订阅行情数据主题：

    {"cmd":"sub","args":["ticker"]}
    
订阅成功后，将收到服务端推送数据：

    {
    	"topic": "ticker",  //ticker行情
    	"scope": "full",    //full所有交易对
    	"data": [{
    		"symbol": 交易对,   
    		"baseAsset": 基础币种,     
    		"quoteAsset": 计价币种, 
    		"marketGroup": 所属分组, 
    		"priceChange": 涨跌,
    		"priceChangePercent": 涨跌比例,
    		"high": 最高价,
    		"low": 最低价,
    		"open": 开盘价,
    		"latest": 最新价,
    		"usdtValue": usdt估值,
    		"volume": 成交量（计价币种）,
    		"amount": 成交量（基础币种）
    	}]
    }
    
- **取消订阅**

Websocket客户端发送如下请求以取消订阅行情数据主题：

    {"cmd":"unsub","args":["ticker"]}
    

## 最新成交 ##

- **订阅**

Websocket客户端发送如下请求以订阅最新成交数据主题：

    {"cmd":"sub","args":["trade"]}
    
订阅成功后，将收到服务端推送数据：

    {
    	"topic": "trade",
    	"symbol": "ETH_USDT",   //full交易对
    	"scope": "full",    //full所有数据
    	"data": [{
    		"takerSide": sell卖buy买,
    		"price": 价格,
    		"quantity": 数量,
    		"amount": 金额,
    		"tradeTime": 交易时间（格式：hh:mm:ss）,
    		"tradeFullTime": 交易时间（格式：yyyy-MM-dd hh:mm:ss）
    	}]
    }
    
- **取消订阅**

Websocket客户端发送如下请求以取消订阅最新成交数据主题：

    {"cmd":"unsub","args":["trade"]}
    
## 深度 ##

- **订阅**

Websocket客户端发送如下请求以订阅深度主题：

    {"cmd":"sub","args":["orderbook","ETH_USDT"]}
    
其中orderbook为当前委托主题，ETH_USDT为交易对
    
订阅成功后，将收到服务端推送数据：

    {
    	"topic": "orderbook",
    	"symbol": "ETH_USDT",   //交易对
    	"scope": "full",    //全数据
    	"data": {
    		"bids": [{          //买入
    			"price": 价格,
    			"qty": 数量,
    			"w": ""
    		}],
    		"asks": [{          //卖出
    			"price": 价格,
    			"qty": 数量,
    			"w": ""
    		}]
    	}
    }
    
- **取消订阅**

Websocket客户端发送如下请求以取消订阅深度主题：

    {"cmd":"unsub","args":["orderbook","ETH_USDT"]}
    
## 个人信息变更 ##

- **订阅**

Websocket客户端发送如下请求以获取权限：

    {"cmd":"auth","args":["listenerKey"]}
    
    
其中listenerKey可通过apikey获取, 请参考API接口：[/trade/listenerKey](https://github.com/aacoinapi/API_Docs/blob/master/REST_api_reference.md)
    
- **委托单变更**
    
授权成功后，服务端推送订单变动数据：

    {
    	"topic": "order_event",
    	"data": {
    		"eventType": order_update订单更新（新订单或部分成交），order_finish订单已成交,
    		"orderId": 订单ID,
    		"baseAsset": 基础币种,
    		"quoteAsset": 计价币种,
    		"side": buy买入sell卖出,
    		"price": 价格,
    		"quantity": 数量,
    		"filledQuantity": 成交量,
    		"filledAmount": 成交额,
    		"filledAvgPrice": 成交均价,
    		"tradeCount": 交易次数,
    		"orderType": limit限价交易,
    		"orderTime": 下单时间,
    		"via": "",
    		"version": 
    	}
    }
   
- **资产变更**

    
授权成功后，服务端推送账户变更数据：

    {
    	"topic": "account_event",
    	"data": [{
    		"assetCode": 币种,
    		"balance": 账户总额,
    		"frozenBalance": 冻结,
    		"availableBalance": 可用余额
    	}]
    }
     
