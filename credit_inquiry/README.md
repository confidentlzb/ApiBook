##1. 授信结果查询接口
在经过一段时间后第三方机构没有收到审批结果，则可以调用还呗提供的授信结果查询接口来获取结果。
##2. 调用场景
第三方机构在未收到授信结果时调用。




##3. 接口要求
* https接口
* 接口采用post方式
* 数据组织采用json传递



##4. 接口定义
http://openapi.dev.lattebank.com/xxxchannel/queryAudit?r_c=XXX(渠道代码大写)
###4.1 请求说明
####4.1.1 参数定义
参数|  名称 | 值类型| 是否可空|  备注
----------- | ------------- | ------------
user_id| 第三方机构借款用户ID|  String|  否|


####4.1.2 请求示例
```
{
   "user_id":"245132241561415"
}
```
###4.2 响应说明
####4.2.1 参数定义

参数|  名称 | 值类型 |是否可空|  备注
----------- | ------------- | ------------
user_id |第三方机构借款用户ID|  String | 否|
conclusion|  审批结果|  Integer| 否 |1：通过，2：未通过  3.授信中
instructions|  结果说明|  String|  否 |成功或者失败都要返回，失败需要返回详细的原因说明。
approval_time | 审批时间 | Long | 否| 成功或者失败都要返回 毫秒时间戳
max_amount| 最大申请金额 | BigDecimal | 是| 最大申请金额
min_amount| 最小申请金额 | BigDecimal | 是| 最小申请金额
range_amount| 颗粒度 |Integer| 是 | 	单位：分，一般来说是100
loan_unit|  借款期限单位|  Integer |是| 1：日、2：月
loan_term | 借款可选期限|  array(Integer)|  是| 返回可选期限的数组，比如：[3,6,12]，如果是月的话，表示可选3，6，12月三种
loan_rate | 费率|  String|  是 |费率，同期数返回，每期对应


####4.2.2 
* 审批通过
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
			"max_amount": "10000.00",
			"min_amount": "1000.00",
			"range_amount": 100,
			"user_id": "9996687696699"
		}    
}
```
* 审批未通过
```
{
    "code": 200,
    "message": "success",
    "bizContent":{
        "user_id":"245132241561415",
        "conclusion":2,       
        "instructions":"信用评分过低",
        "approval_time":152323123946
    }
}
```

* 审核中

```

{
    "code": 200,
    "message": "success"
    "bizContent":{
         "user_id":"245132241561415",
        "conclusion":3,       
        "instructions":"正在审核中",
        "approval_time":152323123946
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


