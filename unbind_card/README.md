##1. 接口简述

用户进入银行卡列表选择解绑操作后，第三方机构调用还呗解绑卡接口解绑。


第三方机构不支持解绑卡操作，需要还呗完成解绑卡操作。

##2. 调用场景
* 针对已经绑定的银行卡，可以选择解绑操作。

##3. 接口要求
* https接口
* 接口采用post方式
* 数据组织采用json传递
* 性能要求：2s，超过2s后判断为超时。超时后不会重试
* 幂等性要求：重复请求，还呗需返回相同结果。（由于可能出现网络超时，我们没有收到还呗返回的结果，我们会重试，若您返回非200，会导致用户无法完成后续操作）

##4. 接口定义

####4.1.1 请求参数定义
参数|  名称|  值类型| 是否必填|  备注
----------- | ------------- | ------------
user_id| 第三方机构借款用户ID|  String|  Y|用户ID
bank_card| 解绑卡卡号|  String|  Y| 解绑卡卡号（完整卡号）
bank_card_type|  绑卡的卡类型|  Integer| N| 新绑卡时有值，选择已有旧卡时为空。 0：储蓄卡，1：信用卡

####4.1.2 请求示例
* 解绑卡示例：
```
{
    "user_id": "246964109149933",
    "bank_card": "6222022005001212723",
    "bank_card_type": 0
}
```

####4.2.1 参数定义

参数|   值类型 |是否必填|  备注
----------- | ------------- | ------------
code | Integer |Y| 200为成功，其他为失败
message| String|  Y| 成功为success,失败为原因
biz_data |Object| N| 响应数据

####4.2.2 响应示例
```
{
    "code": 200,
    "message": "success",
    "biz_data": {}
}
```
