AAcoin对外提供的Rest API接口分为两类：行情类接口和交易类接口。 其中交易类接口必须使用ApiKey进行身份认证，行情类接口则不需要。
# 基本信息

- API服务器BaseUrl地址：https://api.aacoin.com
- 所有接口的调用不论成功或失败，其响应数据格式为

```js
{
  resultCode: // ok为成功，其他错误代码详见REST_error_code,
  resultMessage: //错误信息
  data: //结果，为object或者array格式
}
```
- API调用传参可以通过url的query string（`GET`方法也只能通过这个方式），通过`POST`方法调用时，也可以用request body。

- 可能的http响应码有：
  - `200`：调用正常返回
  - `429`：频率超限
  - `403`：ip被封
  - `500`：服务器内部处理错误


# 访问频率限制
每秒限制不能超过20次调用，超过此限制将返回`429`http状态码，如果在一分钟内连续违反此限制10次，ip将被封禁一分钟（返回`403`http状态码），一分钟后自动解封。

# 交易接口签名规则
使用ApiKey调用交易类接口时，每个请求除了按照对应的接口参数要求传参外，都必须带上`timestamp`、`accessKey`和`sign`这3个参数。其中`timestamp`是当前的服务器时间戳，精确到毫秒。当请求到达服务器时，如果timestamp < 服务器当前时间 - 5000毫秒，或者timestamp > 服务器当前时间 + 1000，则认为是无效请求。
下面就获取用户当前委托的接口（`GET` `/api/v2/trade/getOpenOrders`）举例如何计算签名`sign`：

## 接口参数
symbol=ETH_USDT

## 时间戳，精确到毫秒
timestamp=1562140774138

## accesssKey
accesssKey=xxxx

## 计算签名所需的步骤：
1. 按照ASCII码的顺序对参数名进行排序，并将参数用`&`连接，组成最终需要签名的字符串。

 ```
 accessKey=xxx&symbol=ETH_USDT&timestamp=1562140774138
 ```

1. 用私钥secretKey对上述字符串进行签名（HmacSHA256算法），生成签名字符串：

```
ccc4c65891f1cd951510ea65e3f2dd87c024972c188f46c60216af2e9d3d9727
```
将其带到请求参数中：
```
https://api.aacoin.com/api/v2/trade/getOpenOrders?accessKey=xxx&symbol=xxx&timestamp=xxx&sign=ccc4c65891f1cd951510ea65e3f2dd87c024972c188f46c60216af2e9d3d9727
```
