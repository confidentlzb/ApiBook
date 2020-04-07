##1. 接口简述
还呗向第三方机构提供订单审批反馈信息，审批结果有二种情况：

1. 审批通过
   * 单期-固定金额-固定期限：例如审批结果中，审批的金额为1000元，审批期限为10天。
   * 单期-金额范围-期限范围：例如审批结果中，审批的金额为1000-2000元，审批期限为10-20天。
   * 单期-固定金额-期限范围：例如审批结果中，审批的金额为1000元，审批期限为10-20天。
   * 单期-金额范围-固定期限：例如审批结果中，审批的金额为1000-2000元，审批期限为10天。
   * 多期-固定金额-固定期限： 例如审批结果中，需要用户多期还款，审批的金额为5000元，审批期限为1个月。
   * 多期-金额范围-期限范围：例如审批结果中，需要用户多期还款，审批的金额为5000-8000元，审批期限为1-3个月。
   * 多期-固定金额-期限范围： 例如审批结果中，需要用户多期还款，审批的金额为5000元，审批期限为1-3个月。
   * 多期-金额范围-固定期限： 例如审批结果中，需要用户多期还款，审批的金额为5000-8000元，审批期限为1个月。
2. 审批不通过

##2. 调用场景
当还呗完成审批时，调用第三方机构接口反馈审批结果。

##3. 接口要求
* 接口采用post方式
* 数据组织采用json传递。
* 性能要求：2s，超过2s后判断为超时。超时后不会重试。

##4. 接口定义
###4.1 请求说明
参数|  名称|  值类型| 是否必填|  备注
----------- | ------------- | ------------
order_no|  订单编号|  string|  Y| 反馈订单的订单编号
####4.1.1 请求示例
```
{
    "order_no": "245132241561415"
}
```
###4.2 响应说明
####4.1.1 参数定义
参数|  名称|  值类型| 是否必填|  备注
----------- | ------------- | ------------
order_no|  订单编号|  string|  Y| 反馈订单的订单编号
conclusion|  审批结论|  int| Y| 0=审批不通过<br>1=审批通过
approval_time| 审批时间|  timestamp| Y| 反馈订单审批的时间戳,10位数字
amount_type| 审批金额是否固定|  int| Y| 0=固定金额；1=金额范围（如500-1000）；
max_loan_amount| 审批金额最大值/分| long|  N| 此字段在amount_type=1（金额范围）返回，单位分；
min_loan_amount| 审批金额最小值/分 |long | N| 此字段在amount_type=1（金额范围）返回，单位分；
range_amount|  金额变更粒度|  long|  Y |此字段在amount_type=1（金额范围）返回，代表在最大审批金额和最小审批金额间的最小变更金额； 例如，审批金额最大值为200，审批金额最小值为100，金额变更粒度为50，则表示审批范围值为：100,150,200。
approval_amount| 审批金额/分|  long|  Y| 审批金额最大值/分，approval_amount=max_loan_amount
term_type| 审批期限是否固定|  int| Y |0=固定期限；1=可选期限(（如7天、14天等))
term_unit| 期限类型|  int |Y| 1=单期产品（按天计息）,2=多期产品（按月计息）
approval_term| 审批期限|  int| Y| 最大审批期限，例如：loan_term_option取值为[3,6,12], approval_term取值为12
loan_term_option|  审批天/月数|  string | Y |此字段代表审批天/月数；string需要按照如"[3,6,12]"格式返回
approval_rate| 审批利率|  string|  N| 此字段代表审批利率
rate_unit| 审批利率单位|  int |N |此字段代表审批利率的单位。<br>0:日<br>1:月<br>2:年
repay_amount|  总还款额/分 | long | N| 用户的总还款额（包括本金利息管理费手续费等一切费用）
receive_amount|  总到账金额/分| long|  N| 实际打款到银行卡的金额
period_amount| 每期应还金额/首期应还金额/分| long|  N |1.若多期账单在还款时某期（如首期）需要额外付一些手续费，则此费用不计算在每期应还金额中；<br>2.对于等额本金的方式，该字段为首期应还金额。
remark  |总还款额组成说明|注意：此字段内容金额单位为（元）  |string | 是 1. 当conclusion=1 示还呗情况传参，说明还呗的总还款金额组成，金额加起来为总还款额。例如：本金：100.00元，利息：300.00元，其他手续费：10.00元。<br>2. 当conclusion=0 备注（例如：信用评分过低##拒绝客户，如果没备注，信用评分过低##）

* 备注：repay_amount、receive_amount、period_amount、remark未传值的情况下，第三方机构通过试算接口获取。

####4.1.2 响应示例
* 单期-固定金额-固定期限：例如审批结果中，审批的金额为1000元，审批期限为10天。
```
{
    "code": 200,
    "msg": "success",
    "bizContent": {
        "order_no": "2018012115043300223878",
        "conclusion": 1,
        "approval_time": 1499222340,
        "amount_type": 0,
        "approval_amount": 100000,
        "range_amount": 0,
        "term_unit": 1,
        "term_type": 0,
        "approval_term": 10,
        "loan_term_option": "[10]",
        "pay_amount": 101000,
        "receive_amount": 100000,
        "period_amount": 0,
        "remark": "本金：1000.00元，利息：5.00元，其他手续费：5.00元"
    }
}
  ```
* 多期-固定金额-固定期限：例如审批结果中，需要用户多期还款，审批的金额为1000元，审批期限为1个月。
```
{
    "code": 200,
    "msg": "success",
    "bizContent": {
        "order_no": "2018012115043300223878",
        "conclusion": 1,
        "approval_time": 1499222340,
        "amount_type": 0,
        "approval_amount": 100000,
        "range_amount": 0,
        "term_unit": 2,
        "term_type": 0,
        "approval_term": 1,
        "loan_term_option": "[1]",
        "pay_amount": 101000,
        "receive_amount": 100000,
        "period_amount": 0,
        "remark": "本金：1000.00元，利息：5.00元，其他手续费：5.00元"
    }
}
  ```
* 单期-范围金额-范围期限：例如审批结果中，审批的金额为1000-2000元，审批期限为10,20天。 此种情况下，第三方机构通过试算接口展示对应的总还款金额和到帐金额
```
{
    "code": 200,
    "msg": "success",
    "bizContent": {
        "order_no": "2018012115043300223878",
        "conclusion": 1,
        "approval_time": 1499222340,
        "amount_type": 1,
        "max_loan_amount": 200000,
        "min_loan_amount": 100000,
        "approval_amount": 200000,
        "range_amount": 100,
        "term_unit": 1,
        "term_type": 1,
        "approval_term": 20,
        "loan_term_option": "[10,20]",
        "pay_amount": 0,
        "receive_amount": 0,
        "period_amount": 0,
        "remark": "本金：1000.00元，利息：5.00元，其他手续费：5.00元"
    }
}
  ```
* 多期-范围金额-范围期限：例如审批结果中，需要用户多期还款，审批的金额为1000-2000元，审批期限为1，3个月。 此种情况下，第三方机构通过试算接口展示对应的总还款金额和到帐金额 
```
{
    "code": 200,
    "msg": "success",  
    "bizContent": {
        "order_no": "2018012115043300223878",
        "conclusion": 1,
        "approval_time": 1499222340,
        "amount_type": 1,
        "max_loan_amount": 200000,
        "min_loan_amount": 100000,
        "approval_amount": 200000,
        "range_amount": 10000,
        "term_unit": 2,
        "term_type": 1,
        "approval_term": 3,
        "loan_term_option": "[1，3]",
        "pay_amount": 0,
        "receive_amount": 0,
        "period_amount": 0,
        "remark": "本金：1000.00元，利息：5.00元，其他手续费：5.00元"
    }
}
  ```
* 审批不通过
```
{
    "code": 200,
    "msg": "success",
    "bizContent": {
        "order_no": "2018012115043300223878",
        "conclusion": 0,
        "approval_time": 1499222340,
        "remark": "信用评分过低"
    }
}
  ```


###4.2 异常说明
```
{
  "code": 400,
  "msg": ""
}
```
