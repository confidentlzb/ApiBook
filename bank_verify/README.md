##1. 接口简述
第三方机构调用还呗提供的银行卡验证接口，机构验证银行卡三要素（卡号、姓名、身份证）是否一致。
##2. 调用场景
* 用户操作绑卡
* 用户进入账单页，想要操作还款

##3. 接口要求
* https接口
* 接口采用post方式
* 数据组织采用json传递。

##4. 接口定义
http://openapi.dev.lattebank.com/xxxchannel/verifyCard?r_c=XXX(渠道代码大写)
###4.1 请求说明
####4.1.1 
参数 | 名称|  值类型 |是否必填|  备注
----------- | ------------- | ------------
bank_card| 卡号|  String|  Y| 卡号（完整卡号）
user_name| 姓名|  String|  Y| 用户姓名
id_number| 身份证号|  String|  Y| 用户身份证号
user_mobile| 手机号| String|  N| 用户手机号
bank_card_type|  卡类型| Integer| N| 0：储蓄卡，1：信用卡
bank_address|  开户行地址| String|  N| 开户行地址
open_bank| 绑卡开户行| String|  N| 绑定银行卡的开户行（例如：ICBC，ABC，CMB，CCB等）

####4.1.2 请求示例
```
{
  "bank_card": "432700000000000",
  "user_name": "张三",
  "id_number": "身份证号",
  "user_mobile": "18100000000",
  "bank_card_type": 1,
  "bank_address": "",
  "open_bank": ""
}
```
###4.2 响应说明
####4.2.1 参数示例
```
{
  "code": 200,
  "message": ""
}
```
####4.2.2 异常说明

code为非200时，均为异常情况，请在msg中返回具体的可读的中文失败原因。
404 用户信息不存在 501 该卡未通过审核 400 系统异常
```
{
  "code": 400,
  "message": "您请求的参数有误"
}
```
