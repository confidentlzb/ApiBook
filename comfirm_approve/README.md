##1. 接口简述
第三方机构调用还呗提供的借款接口，第三方机构将用户确认的贷款信息（贷款金额和期限）发送给机构。最终还呗告第三方机构其是否接收成功。



##2. 调用场景
审批通过后，用户操作确认最终申请的贷款信息后，第三方机构将调用还呗提供的推送审批确认结果接口。
##3. 接口要求
* https接口
* 接口采用post方式
* 数据组织采用json传递。
* 性能要求：1s，超过1s后判断为超时。超时后不会重试。

##4. 接口定义
http://openapi.dev.lattebank.com/xxxchannel/lend?r_c=XXX(渠道代码大写)
###4.1 请求说明
####4.1.1 场景1具体参数定义

参数|  名称|  值类型| 是否必填|  备注
----------- | ------------- | ------------
user_id| 第三方机构借款用户ID|  String|  Y|
order_no|  订单编号|  String|  Y |
loan_amount| 审批金额/分|  Long|  Y |
stage |审批期限 | Integer| Y| /
term_unit |期限单位|  Integer| Y |1：天2：月
loan_purpose | 贷款用途|  String|  N |
verify_code| 验证码| String|  Y| 验证码

####4.1.2 请求示例
```
{
  "user_id": "123123",
  "order_no": "245132241561415",
  "loan_amount": 100000,
  "stage": 12,
  "term_unit": 2,
  "loan_purpose":"消费",
  "verify_code":"2345"
   }
```
###4.2 响应说明
####4.2.1 参数定义

参数|   值类型 |是否必填|  备注
----------- | ------------- | ------------
code | Integer |Y| 200为推送成功，其他为失败，其他情况：602不支持的借款期数 400系统错误 801 验证码不正确 901 参数异常  
message| String|  Y| 成功为success,失败为原因
bizContent |Object| N| 响应数据

####4.2.1 响应示例
```
{
    "code": 200,
    "message": "success",
    "bizContent": {}
}
```
