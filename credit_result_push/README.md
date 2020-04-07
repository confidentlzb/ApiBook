##1. 授信结果推送接口

##2. 调用场景
审批完用户资料后，需要将用户的授信信息同步到第三方机构




##3. 接口要求
* https接口
* 接口采用post方式
* 数据组织采用json传递



##4. 接口定义
###4.1 请求说明
####4.1.1 参数定义

参数|  名称 | 值类型| 是否必填|  备注
----------- | ------------- | -----
user_id| 第三方机构借款用户ID|  String|  Y|
conclusion|  审批结果|  Integer| Y |1：通过，2：未通过  3.授信中
instructions|  结果说明|  String|  Y| 成功或者失败都要返回，失败需要返回详细的原因说明。
approval_time|  审批时间|  Long | Y| 成功或者失败都要返回 毫秒时间戳
max_amount| 最大申请金额 | BigDecimal | N |最大申请金额
min_amount| 最小申请金额 | BigDecimal | N |最小申请金额
range_amount| 颗粒度 |Integer |N|  单位：分 一般来说是100
loan_unit | 借款期限单位 | Integer |N |1：日、2：月
loan_term | 借款可选期限|  array(Integer)|  N| 返回可选期限的数组，比如：[3,6,12]，如果是月的话，表示可选3，6，12月三种
loan_rate | 费率 | String | N |费率，同期数返回，每期对应


####4.1.2 请求示例
* 审批通过
```
    {
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
```
* 审批未通过
```
{  
    "user_id":"245132241561415",        
    "conclusion":2,       
    "instructions":"信用评分过低",
    "approval_time":152323123946   
}
```

###4.2 响应说明
####4.2.1 参数定义

参数|   值类型 |是否必填|  备注
----------- | ------------- | ------------
code | Integer |Y| 200为成功，其他为失败
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

