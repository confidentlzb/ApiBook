##1. 接口简述
通过订单号查询还款计划
##2. 调用场景
* 借款后用户可以查看具体还款计划。

##3. 接口要求
* 接口采用post方式
* 数据组织采用json传递。
* 性能要求：2s，超过2s后判断为超时。超时后不会重试。

##4. 接口定义
http://openapi.dev.lattebank.com/xxxchannel/repay/plan?r_c=XXX(渠道代码大写)
###4.1 请求说明
参数|  名称|  值类型| 是否可空|  备注
----------- | ------------- | ------------
order_no|  订单编号|  String|  否| 订单编号
####4.1.1 请求示例
```
{
    "order_no": "245132241561415"
}
```
###4.2 响应说明
####4.1.1 参数定义
参数|  名称 | 值类型| 是否可空|  备注
----------- | ------------- | ------------
user_id| 第三方机构借款用户ID|  String|  否|
order_no|  订单编号|  String|  否| 
open_bank| 银行名称|  String | 否| 还款银行名，中文名，不要传代码，会展示给用户
bank_card| 银行卡号|  String|  否| 还款银行卡号（完整卡号）
repayment_plan|  还款计划 | Array|（JSON）| 否 
can_prepay|  是否支持提前全部结清|  Integer| 是| 仅多期产品需回传，1=支持；0=不支持。
can_prepay_time |可提前全部结清的开始时间 | Timestamp |是| 当支持提前全部结清时需回传。十位时间戳

还款计划（repayment_plan）

参数|  名称|  值类型 |是否可空|  备注
----------- | ------------- | ------------
period_no| 期数|  Integer| 否| 具体的第几期
due_time|  到期时间|  Timestamp| 否| 到期时间。十位时间戳
amount|  还款总金额/分 |Long | 否 |应该还的金额<br>1. 逾期后，需要加入逾期费用 <br>2.当该期完成还款后，该金额与之前保持一致，不要传0
paid_amount| 已还金额/分|  Long|  是| 当机构有部分扣款的时候，必传。已经还成功的金额，amount-paid_amount计算出待还金额
principal| 本金/分|  Long|  是| 本金
fee| 待还手续费/分|  Long|  是| 待还手续费
interest| 待还罚息/分|  Long|  是| 待还罚息
late_fee| 待还滞纳金/分|  Long|  是| 待还滞纳金
bill_status| 账单状态|  Integer| 否 |1：未到期<br>2：已还款<br>3：逾期
success_time|  还款成功时间 | Timestamp| 是| 时间戳，秒，当账单状态bill_status为2时必传
pay_type|  支持的还款方式类型| Integer| 是 |机构当前支持的还款方式，AUTO_DEDUCT自动还款 MANUAL_REPAY主动还款；<br>2=银行代扣；只有还款的才会显示还款方式
can_repay_time|  可以还款时间 | Timestamp |否| 时间戳，秒，当期最早可以还款的时间
remark|  描述注意：此字段内容金额单位为（元）|  String | 否 |当期还款金额的相关描述,需可读。例如：含本金473.10元，手续费172.40元，罚息&滞纳金20.00

####4.1.2 响应示例
```
{
  "user_id": "11122233",
  "order_no": "245132241561415",
  "open_bank": "中国工商银行",
  "bank_card": "621226230800431232",
  "can_prepay": 1,
  "can_prepay_time": 1473350400,
  "repayment_plan": [
    {
      "period_no": 1,
      "due_time": 1473350400,
      "amount": 66050,
      "paid_amount": 12000,
      "principal": 10000,
      "fee": 4000,
      "interest": 12000,
      "late_fee": 2000,
      "pay_type": "AUTO_DEDUCT",
      "bill_status": 1,
      "can_repay_time": 1476115200,
      "remark": "含本金473.10元，利息&手续费172.40元，初审费15.00元，逾期费20.00元。"
    },
    {
      "period_no": 2,
      "due_time": 1476115200,
      "amount": 64550,
      "paid_amount": 64550,
      "principal": 10000,
      "fee": 4000,
      "interest": 12000,
      "late_fee": 2000,
      "pay_type": "AUTO_DEDUCT",
      "bill_status": 2,
      "can_repay_time": 1476115200,
      "remark": "含本金477.83元，利息&手续费167.67元",
      "success_time": 1476115200,
    }
  ]
}
```

###4.2 异常说明
```
{
  "code": 400,
  "message": ""
}
```