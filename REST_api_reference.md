
# API #
----------

**/common/symbols 获取所有交易对**

响应数据：

参数名称|是否必须|数据类型|描述
---|---|---|---
status|true|String|状态值
msg|false|string|提示信息

data说明：

    [
    	{
    	"baseCurrency": "基础币种",
    	"quoteCurrency": "计价币种",
    	"priceDecimal": 价格的最大小数位数,
    	"quantityDecimal": 数量的最大小数位数,
    	"symbolName": "交易对名称"
    	}
    ]



**/common/currencies 获取所有币种**

响应数据：

参数名称|是否必须|数据类型|描述
---|---|---|---
status|true|String|状态值
msg|false|string|提示信息

data说明：

    ["币种代码"]



**/common/timestamp 获取系统当前时间戳**

响应数据为Number类型



**/account/accounts 获取所有的账户信息**

响应数据：

参数名称|是否必须|数据类型|描述
---|---|---|---
status|true|String|状态值
msg|false|string|提示信息

data说明：

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


**/account/statement 获取账户操作日志**

请求参数:

参数名称|是否必须|类型|描述
---|---|---|---
accountId|true|Number|账户ID
limit|false|Number|显示数量

响应数据：

参数名称|是否必须|数据类型|描述
---|---|---|---
status|true|String|状态值
msg|false|string|提示信息

data说明：

    [
	    {
		    "time": "时间（格式：yyyy-MM-dd HH:mm:ss）",
		    "event": "取值范围：提现、充值、交易、挂单、撤单、分发",
		    "amount": 本次操作金额,
		    "afterBalance": 本次操作之后的金额
	    }
    ]