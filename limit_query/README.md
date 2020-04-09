##1. 额度查询接口
返回最新的额度信息
##2. 调用场景
在授信后或者借款前可以调用该接口获取最新的额度，减少借款数超出额度的情况。




##3. 接口要求
* https接口
* 接口采用post方式
* 数据组织采用json传递



##4. 接口定义
http://openapi.dev.lattebank.com/xxxchannel/queryLimit?r_c=XXX(渠道代码大写)
###4.1 请求说明
####4.1.1 参数定义
参数|  名称 | 值类型| 是否必填|  备注
----------- | ------------- | ------------
user_id| 第三方机构借款用户ID|  String|  Y|


####4.1.2 请求示例
```
{
   "user_id":"245132241561415"
}
```
###4.2 响应说明
####4.2.1 参数定义

参数|  名称 | 值类型 |是否必填|  备注
----------- | ------------- | ------------
user_id |第三方机构借款用户ID|  String | Y|
conclusion|  审批结果|  Integer| Y |1：通过，2：未通过  3.授信中
instructions|  结果说明|  String|  Y |成功或者失败都要返回，失败需要返回详细的原因说明。
approval_time | 审批时间 | Long | Y| 成功或者失败都要返回 毫秒时间戳
max_amount| 最大申请金额 | BigDecimal | N| 最大申请金额
min_amount| 最小申请金额 | BigDecimal | N| 最小申请金额
range_amount| 颗粒度 |Integer| N | 	单位：分，一般来说是100
loan_unit|  借款期限单位|  Integer |N| 1：日、2：月
loan_term | 借款可选期限|  array(Integer)|  N| 返回可选期限的数组，比如：[3,6,12]，如果是月的话，表示可选3，6，12月三种
loan_rate | 费率|  String|  N |费率，同期数返回，每期对应


####4.2.2 

```
{     
    "code": 200,
    "message": "success",
    "bizContent":{
			"approval_time": 1566269965000,
			"conclusion": 1,
			"instructions": "授信成功",
			"loan_term": "[3,6,12]",
			"loan_unit": 2,
			"max_amount": "1000000",
			"min_amount": "100000",
			"range_amount": 100,
			"user_id": "9996687696699"
		}    
}
```

####4.2.3 异常说明

code为非200时，均为异常情况，请在msg中返回具体的可读的中文失败原因。
```
{
  "code": 400,
  "message": ""
}
```


