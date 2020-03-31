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

参数|  名称|  值类型| 是否可空|  备注
----------- | ------------- | ------------
user_id| 第三方机构借款用户ID|  String|  否|
order_no|  订单编号|  String|  否 |
loan_amount| 审批金额/分|  Long|  否 |
stage |审批期限 | Integer| 否| /
term_unit |期限单位|  Integer| 否 |1：天2：月
loan_purpose | 贷款用途|  String|  是 |
verify_code| 验证码| String|  否| 验证码

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

参数|   值类型 |是否可空|  备注
----------- | ------------- | ------------
code | Integer |否| 200为推送成功，其他为失败，其他情况：602不支持的借款期数 400系统错误 801 验证码不正确 901 参数异常  
message| String|  否| 成功为success,失败为原因
bizContent |Object| 是| 响应数据

####4.2.1 响应示例
```
{
    "code": 200,
    "message": "success",
    "bizContent": {}
}
```
