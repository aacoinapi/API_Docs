# 安全认证 #
----------
目前关于apikey申请和修改，请在“个人中心 - API管理”页面进行相关操作。其中AccessKey为API 访问密钥，SecretKey为用户对请求进行签名的密钥（仅申请时可见）。

- ** 重要提示：这两个密钥与账号安全紧密相关，无论何时都请勿向其它人透露。**


# 合法请求结构 #

----------
基于安全考虑，除行情API 外的 API 请求都必须进行签名运算。一个合法的请求由以下几部分组成：
- 方法请求地址 即访问服务器地址：api.aacoin.com后面跟上方法名，比如api.aacoin.com/v1/account/statement。
- API 访问密钥（AccessKeyId） 您申请的 APIKEY 中的AccessKey。
- 必选和可选参数 每个方法都有一组用于定义 API 调用的必需参数和可选参数。可以在每个方法的说明中查看这些参数及其含义。
- 签名 签名计算得出的值，用于确保签名有效和未被篡改。


# 签名运算 #
----------
API 请求在通过 Internet 发送的过程中极有可能被篡改。为了确保请求未被更改，我们会要求用户在每个请求中带上签名，来校验参数或参数值在传输途中是否发生了更改。

**计算签名所需的步骤：**
1、按照ASCII码的顺序对参数名进行排序。
例如，下面是请求参数的原始顺序

    accessKey=xxx
    symbol=xxx
    type=xxx
    quantity=xxx
    price=xxx

这些参数会被排序为：

    accessKey=xxx
    price=xxx
    quantity=xxx
    symbol=xxx
    type=xxx

按照以上顺序，将各参数使用字符’&’连接，组成最终要进行签名的计算的字符串如下：

    accessKey=xxx&price=xxx&quantity=xxx&symbol=xxx&type=xxx

2、计算签名（HmacSHA256）。

- 需要签名的字符串



    accessKey=xxx&price=xxx&quantity=xxx&symbol=xxx&type=xxx

- 进行签名的密钥（SecretKey）



    b0xxxxxx-c6xxxxxx-94xxxxxx-dxxxx

- 得到签名计算结果并转成16进制编码


    ccc4c65891f1cd951510ea65e3f2dd87c024972c188f46c60216af2e9d3d9727

3、最终将计算出来的签名添加到 API 请求中。

例：

    https://api.aacoin.com/v1/account/statement?accessKey=xxx&price=xxx&quantity=xxx&symbol=xxx&type=xxx&sign=ccc4c65891f1cd951510ea65e3f2dd87c024972c188f46c60216af2e9d3d9727