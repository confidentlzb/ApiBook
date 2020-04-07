##1. 接口简述
第三方机构调用还呗提供的订单状态拉取接口，接口中告知机构订单号。最终还呗根据订单号返回其最新的订单状态给到第三方机构。

当我方拉取状态时，若机构还没有相应的状态，处理方式如下：

* 若还未出审批通过/拒绝状态的初始订单状态时，我方进行拉取时，订单状态order_status字段值返回为空；若已有上一步订单状态，拉取时下一步状态还不能给出，此时返回上一步状态即可。
* 如：目前状态是审批通过，下一步等待机构放款反馈放款结果，若拉取时还不能给出放款结果时，还返回上一步审批通过的状态; 以上的订单状态均指附录5.1 订单状态中的状态。对于审批中、放款中和机构内部状态都无需告知第三方机构。审批中、放款中第三方机构会自动置状态。
##2. 调用场景
* 每天针对一些订单（例如超过审批时长）定时调用，1天最多调用2次。
* 用户进到订单详情和订单列表时会调用接口。

##3. 接口要求
* 接口采用post方式
* 数据组织采用json传递。
* 性能要求：2s，超过2s后判断为超时。超时后不会重试。

##4. 接口定义
http://openapi.dev.lattebank.com/xxxchannel/queryLend?r_c=XXX(渠道代码大写)
###4.1 请求说明
参数|  名称|  值类型| 是否必填|  备注
----------- | ------------- | ------------
order_no|  订单编号|  String|  Y| 反馈订单的订单编号
####4.1.1 请求示例
```
{
    "order_no": "245132241561415"
}
```
###4.2 响应说明
####4.2.1 参数定义
参数|  名称|  值类型| 是否必填|  备注
----------- | ------------- | ------------
user_id| 第三方机构借款用户ID|  String|  Y|
order_no|  订单编号|  String|  Y| 反馈订单的订单编号
order_status|订单状态|String|N|需返回的订单状态见附录，机构需将内部状态对应我方状态返回结果
update_time|订单变更时间|Timestamp|Y|十位时间戳，秒；反馈订单流经至对应状态的时间，例如：推送LOAN_SUCCESS状态，此时间代表该订单实际放款时间；推送CLOSED，此时间代表该订单全部欠款还清的时间；

####4.2.2 响应示例
```
{
  "code": 200,
  "message": "success",
  “bizContent”: {
    “user_id”:"111222333",
    "order_no": "245132241561415",
    "order_status": "REPAYING",
    "update_time": 1473350400
  }
}
```

###4.2.3 异常说明
```
{
  "code": 400,
  "message": ""
}
```
##5. 接口定义
5.1 订单状态

订单状态(order_status)|状态说明| 备注
----------- | ------------- | ------------
AUDITING|审批中|正在审批的状态
AUDIT_PASS|审批通过|审批通过的状态
AUDIT_REJECT|审批不通过|审批不通过的状态
CREATED|放款处理中|放款处理中的状态
LOAN_FAILED|放款失败|<br>1. 用户已进入放款环节，但因资方等各种问题导致最终无法给用户打款,中途放款失败可重新放款的请不要回传该状态；<br>2. 此状态为终结状态，放款失败后不再允许流转至其它状态；
LOAN_SUCCESS|放款成功|1. 款项必须已成功打至用户卡中才可推送放款成功；<br>2. 进入放款成功的订单必须推送还款结果，且仅能流转至：CLOSED（到期结清）、OVERDUE（已逾期），若用户已展期订单状态仍为LOAN_SUCCESS即可，但需更新还款计划；
REPAYING|还款中|1. 仅针对多期产品，才有该状态；<br>2. 当某期逾期后结清时，并还需要还款剩余款项，则需要更新订单状态为该状态；
OVERDUE|逾期|1. pay_day产品，超过了最后还款日仍有未还清的借款且未办理延期还款；<br>2.多期产品，当某期超过了当期最后还款日仍有未还清的借款，则认为订单逾期，当期逾期后结清，需更新为还款中状态，更新时间为状态变更时间；
CLOSED|贷款结清|用户还清全部欠款

