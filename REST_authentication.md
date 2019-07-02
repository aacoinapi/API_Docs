# ApiKey获取
目前关于apikey申请和修改，请在“个人中心 - API管理”页面进行相关操作。其中AccessKey为API 访问密钥，SecretKey为用户对请求进行签名的密钥（仅申请时可见）。

**重要提示：这两个密钥与账号安全紧密相关，无论何时都请勿向其它人透露。**

# 调用频率限制
- API：单个`IP`、`用户`调用次数最多为30次/秒，超过30次后将返回Http响应码429，并记录一次违规，1分钟超过10次违规，将返回Http响应码403

# 合法请求结构
基于安全考虑，除行情API 外的 API 请求都必须进行签名运算。一个合法的请求由以下几部分组成：
- API服务器地址：`https://api.aacoin.com/api/[请求路径]`，比如`https://api.aacoin.com/api/v2/trade/getOpenOrders`。
- API 访问密钥（accessKey）： 您申请的 APIKEY 中的AccessKey。
- 必选和可选参数：每个方法都有一组用于定义 API 调用的必需参数和可选参数。可以在每个方法的说明中查看这些参数及其含义。其中必须的参数有：
  - 当前时间戳（timestamp）：当前时间的秒数。
  - 签名(sign)：签名计算得出的值，用于确保签名有效和未被篡改。

# 签名运算 #
API 请求在通过 getOpenOrders 发送的过程中极有可能被篡改。为了确保请求未被更改，我们会要求用户在每个请求中带上签名，来校验参数或参数值在传输途中是否发生了更改。

**接口参数**

symbol=BTC_USDT

**时间戳**

timestamp=1562039574（精确到秒）

**账户参数**

accessKey=xxx

**计算签名所需的步骤：**

1、按照ASCII码的顺序对参数名进行排序。

    accessKey=xxx;symbol=ETH_USDT;timestamp=1562039574

2、按照以上顺序，将各参数使用字符’&’连接，组成最终要进行签名的计算的字符串如下：

    accessKey=xxx&symbol=xxx&timestamp=xxx

3、生成签名（HmacSHA256）。

- 需要签名的字符串

    accessKey=xxx&symbol=xxx&timestamp=xxx

- 进行签名的密钥（SecretKey）

    b0xxxxxx-c6xxxxxx-94xxxxxx-dxxxx

- 得到签名计算结果并转成16进制编码

    ccc4c65891f1cd951510ea65e3f2dd87c024972c188f46c60216af2e9d3d9727

3、最终将计算出来的签名添加到 API 请求中。

例：

    https://api.aacoin.com/api/v2/trade/getOpenOrders?accessKey=xxx&symbol=xxx&timestamp=xxx&sign=ccc4c65891f1cd951510ea65e3f2dd87c024972c188f46c60216af2e9d3d9727
