###<span id="jump4">还款详情接口</span>

http://openapi.dev.lattebank.com/xxxchannel/repay/preTry?r_c=XXX(渠道代码大写)

第三方机构调用合作还呗提供的还款详情获取接口，将告知对应的订单号和还款期数。最终合作还呗根据订单号和还款期数返回其还款金额给到第三方机构。

####1.1.1 请求参数定义
参数 | 名称|  值类型| 是否可空|  备注
----------- | ------------- | ------------
order_no | 订单编号|  String|  否| /
period_nos|  还款期数|  String|  否| 期数：1,2,3...all。all表示所有期。只能是以上枚举值中的一种，不会出现1,2的情况。例如第一期，回传1。第二期，回传2。

####1.1.2 请求示例
```
{
  "order_no": "245132241561415",
  "period_nos": "1"
}
```
####1.1.3 响应参数说明
参数|  名称|  值类型| 是否可空|  备注
----------- | ------------- | ------------
open_id|  用户号| String|  是 |
order_no|  订单号| String| 否|
amount|  当前所需还款额/分| Long|  否 |1. 返回当前时间下，该期账单的时间所需还款额；<br>2. 若为提前结清有费用减免则返回减免后的所需还款额；<br>3. 若已逾期有逾期费用，则反馈包含逾期费用的所需还款额；
fee|  费用/分| Long| 否| 
principal|  本金/分| Long|否|
interest| 待还罚息/分|  Long|  是| 待还罚息
late_fee| 待还滞纳金/分|  Long|  是| 待还滞纳金
remark|  费用明细注意：此字段内容金额单位为（元）|  String | 否 |1. 当期还款金额的相关描述；<br>2. 举例：含本金477.83元，利息&手续费167.67元

####1.1.4 响应示例
```
{
    "code": 200,
    "message": "success",
    “bizContent”: {
    "open_id":"383882",
    "order_no":"222222",
    "amount": 100000,
    "fee": 10000,
    "principal": 10000,
    "interest": 12000,
    "late_fee": 2000,
    "remark": "含本金477.83元，利息&手续费167.67元",

  }
}
```
####1.1.5 异常说明
若没有匹配到对应的订单号，请当做异常情况返回。并在msg中告知没有找到订单号 code为非200时，均为异常情况，请在msg中返回具体的可读的中文失败原因。
```
{
  "code": 400,
  "message": "您请求的参数有误"
}
```