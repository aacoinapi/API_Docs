
# 行情API #

----------
**/common/symbols 获取所有交易对**

响应数据：

参数名称|是否必须|数据类型|描述
---|---|---|---
status|true|String|状态值

data说明：
    [
    	{
    	"baseCurrency": "BTC",
    	"quoteCurrency": "BCB",
    	"priceDecimal": 8,
    	"quantityDecimal": 8,
    	"symbolName": "BCB/BTC"
    	}
    ]