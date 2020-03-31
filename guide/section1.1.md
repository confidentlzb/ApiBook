##API接入综述
###1.引言
还呗通过合作机构API接口获取贷款订单的详细信息（贷款人基本信息、征信信息、审核材料等），完成贷款订单的资质审核，同时还呗向合作机构提供API实现用户的绑卡和还款操作，并通过接口最终反馈回合作机构。

###2. 接入平台条件
* 还呗合作机构
* 约定双方交互的公钥和私钥

###3. 数据交互规范

* 报⽂共分三部分组成:公共数据(双向请求、响应)、业务数据、签名。
* 双⽅报⽂均采⽤用HTTP POST⽅式提交。
* 报⽂数据以 JSON 格式写⼊请求Body中。
* 报文头信息中 Content-Type 指定为"application/json"类型。

###4. 加密规范

* 业务数据先json化，并形成json字符串;
* 发送⽅使用私钥和接收方公钥对json业务数据加密; 
* 将密⽂字节转化为base64字符串赋值给bizContent字段；
* 把内容进行签名，详⻅“签名”部分。

###5. 解密规范

* 接收⽅使用签名算法进行数据校验，详⻅“签名”部分。
* 接收⽅使用私钥和发送⽅公钥对报文bizContent字段(base64解码后的字节码)进行解密; 
* 解密通过后取出明文Json字符串;
* 对json字符串json化，并转化为业务对象。

###6.公共请求参数

参数名称|  类型|  是否必传 | 说明
----------- | ------------- | ------------
transSeqNo | String|  Y |送⼊⽅交易流水号
transReqTime| String | Y |交易请求时间, 时间格式 "20060102150405"
bizContent |String | Y |对业务数据进行RSA加密，base64(RSA(业务明文json内容)),RSA加密过程简要描述: A和B⽅互为接收和送入方，A⽅⽣成⼀对密钥:私钥(KA)和公钥(KPA)，B⽅也⽣成⼀对密钥:(KB)和(KPB)，其中(KPA)和(KPB)是公开的。送⼊⽅⽤算法: E=ENC(M，接收方公钥)进行加密，接收⽅⽤算法:M=DEC(E，接收方私钥)进行解密，即可得到原文。其中约定如下:<br>1.RSA密钥采⽤2048位，可以用openssl⽣成。<br>2.padding模式采⽤PKCS #1 v1.5 padding
sign| String | Y |将返回值键值对(除去sign)按照字段名的 ASCII 码从⼩到⼤排序(字典序，其中bizContent是 加密后的数据)组装成⼀个json格式的字符串，记为sortedJson字符串，然后MD5(salt+sortedJson+salt)即为结果，结果为16进制的32位字符，其中salt双⽅约定.


####代码示例
明文数据：
```
{
    "transReqTime": "20171016163402",
    "transSeqNo": "1508142842478",
   "bizContent": "{"name":"测试","idcardno":"530113198710211919","idcardaddr":"上海海市漕河泾经济开发
区","idcardduration":"20","phone":"13818554350","bankaccount":"621322011571972","companyaddr":"上 海海市漕河泾经济开发区宜⼭山路路988号1901室","contactname":"廉系 仁","contactphone":"13818554351","creditchecking":1}"
}
```

密文数据+签名：
```
{
"transReqTime": "20171016163402",
"transSeqNo": "1508142842478",
"bizContent": "MIIFpAYJKoZIhvcNAQcDoIIFlTCCBZECAQAxgbo....wgbcCAQAI0=", 
"sign:" 098f6bcd4621d373cade4e832627b4f6"
}
```


###7. 接口返回数据格式规范
服务端返回的数据总是包含code和msg字段，根据需要包含bizContent字段。

参数名称|  类型|  是否必传 | 说明
----------- | ------------- | ------------
code | Integer|  Y |返回码(200:成功 1001:缺少必要参数 1002:参数值错误 1003:加密错误 1004:系统 内部错误)
message| String | Y |消息，错误的写明错误原因，成功为success
transSeqNo | String|  Y |送⼊⽅交易流⽔号
transReqTime| String | Y |交易请求时间, 时间格式 "20060102150405"
bizContent |String | Y |对业务数据进⾏RSA加密，base64(RSA(业务明⽂json内容)),RSA加密过程简要描述: A和B⽅互为接收和送⼊方，A⽅⽣成⼀对密钥:私钥(KA)和公钥(KPA)，B⽅也生成⼀对密钥:(KB)和(KPB)，其中(KPA)和(KPB)是公开的。送⼊方⽤算法: E=ENC(M，接收方公钥)进⾏加密，接收⽅用算法:M=DEC(E，接收方私钥)进⾏解密，即可得到原⽂。其中约定如下:<br>1.RSA密钥采⽤2048位，可以用openssl⽣生成。<br>2.padding模式采用PKCS #1 v1.5 padding
sign| String | Y |将返回值键值对(除去sign)按照字段名的 ASCII 码从⼩到⼤排序(字典序，其中bizContent是加密后的数据)组装成⼀个json格式的字符串，记为sortedJson字符串，然后MD5(salt+sortedJson+salt)即为结果，结果为16进制的32位字符，其中salt双⽅约定.



####返回的数据结构为：
```
{
    "code": 200,
    "message": "",
    "transReqTime": "20171016163402",
    "transSeqNo": "1508142842478",
    "bizContent": "MIIFpAYJKoZIhvcNAQcDoIIFlTCCBZECAQAxgbo....wgbcCAQAI0=", 
    "sign:" 098f6bcd4621d373cade4e832627b4f6"
}
```
