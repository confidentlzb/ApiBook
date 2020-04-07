##. 异常说明
### 异常编码

RESPONSE_OK(200, “请求成功”),<br>
LESS_PARAMS(101, “缺少必要参数”),<br>
ERROR_PARAMS(1002, “参数值错误”),<br>
ERROR_ENCRYPT(103, “加密错误”),<br>
ERROR_SERVER(104, “系统内部错误”),<br>
SERVER_ERROR(400, “系统忙不过来啦”),<br>
NOT_FOUND(404, “信息不存在”),<br>
STAGE_ORDER_REPEAT(301, “用户号重复”),<br>
CARD_ERROR_MSG(501, “该卡未通过审核”),<br>
BANK_CARD_IS_BOUND(502, “该卡被已被绑定”),<br>
BIND_CARD_FAILED(505, “绑卡失败”),<br>
LEND_AMOUNT_ERROR(601, “订单额度大于可用额度”),<br>
PERIOD_NOT_SUPPORT(602, “不支持的借款期数”),<br>
LEND_FAIL(603, “借款失败”),<br>
REPAY_FAIL(701, “还款失败”),<br>
REPAY_REPEAT(702, “重复还款”),<br>
CODE_ERROR(801, “验证码不正确”),<br>
ABNORMAL_PARAMETER(901, “参数异常”);<br>


### 内部系统交互可能出现的错误代码
ACCOUNT_NOT_EXISTS_CODE=7409;<br>
ACCOUNT_NOT_EXISTS_MSG="账户不存在";<br>
BIND_CARD_ERROR_CODE=7423;<br>
BIND_CARD_ERROR_MSG="绑卡失败！";<br>
BOC_CHECK_ERR_RESPONSE="由于银行原因,暂不支持还款,请于还款日还款";<br>
BOC_CHECK_ERR_RESPONSE=7438;<br>
CALCULATION_PLAN_EXCEPTION="计算每期还款异常";<br>
CALCULATION_PLAN_EXCEPTION=7404;<br>
CARD_BOUND_ALREADY_CODE=7418;<br>
CARD_BOUND_ALREADY_MSG="该卡已被绑定!";<br>
CARD_NOT_BOUND_CODE=7420;<br>
CARD_NOT_BOUND_MSG="该卡未绑定!";<br>
CARD_NOT_SUPPORT_ERROR_CODE=7416;<br>
CARD_NOT_SUPPORT_ERROR_MSG="不支持的卡片";<br>
CARD_UID_NOT_MAPPINT_CODE=7419;<br>
CARD_UID_NOT_MAPPINT_MSG="用户身份信息与卡信息不匹配！";<br>
CHANNEL_NOT_EXIST_THE_BUSINESSTYPE="该渠道不支持此商业类型";<br>
CHANNEL_NOT_EXIST_THE_BUSINESSTYPE=7406;<br>
DEBIT_CARD_NOT_BOUND_CODE=7410;<br>
DEBIT_CARD_NOT_BOUND_MSG="储蓄卡未绑定";<br>
ERR_ACCESS_LOG_EXTEND_NOT_EXIST=7426;<br>
ERR_ACCESS_LOG="准入落表失败";<br>
ERR_CARD_NOT_EXIST="该卡不存在！";<br>
ERR_CARD_NOT_EXIST=7401;<br>
ERR_GET_ORDER_INFO="获取订单详情异常";<br>
ERR_GET_ORDER_INFO=7427;<br>
ERR_LEND_CONFIRM="借款确认失败";<br>
ERR_LEND_CONFIRM=7503;<br>
ERR_LEND_SUBMIT="借款提交失败";<br>
ERR_LEND_SUBMIT=7502;<br>
ERR_NO_REPAY_PLAN="没有可以还款的分期计划";<br>
ERR_NO_REPAY_PLAN=7432;<br>
ERR_NO_SUCH_CONFIG="找不到对应配置！";<br>
ERR_NO_SUCH_CONFIG=7429;<br>
ERR_ORDER_BANK_INFO_NOT_FUND="找不到该订单对应的银行信息";<br>
ERR_ORDER_BANK_INFO_NOT_FUND=7426;<br>
ERR_ORDER_NO_NOT_FUND="缺少本地订单号";<br>
ERR_ORDER_NO_NOT_FUND=7434;<br>
ERR_REPEAT_REPAY_REQUEST="重复提交还款申请";<br>
ERR_REPEAT_REPAY_REQUEST=7439;<br>
ERR_TRANS_LOG_EXTEND_NOT_EXIST="找不到交易记录下的详情";<br>
ERR_TRANS_LOG_EXTEND_NOT_EXIST=7425;<br>
ERR_TRANS_LOG_NOT_EXIST="找不到交易记录";<br>
ERR_TRANS_LOG_NOT_EXIST=7424;<br>
ERR_USER_INFO_NOT_EXIST="该用户不存在";<br>
ERR_USER_INFO_NOT_EXIST=7402;<br>
ERR_USER_LEND_DATA="获取用户借款相关信息失败";<br>
ERR_USER_LEND_DATA=7501;<br>
ERR_VALID_LIGHT_ASSERT_BANK="无效的轻资产银行，请检查放款路由";<br>
ERR_VALID_LIGHT_ASSERT_BANK=7430;<br>
ERR_VALID_ORDER_INFO_NOT_FUND="无有效的订单";<br>
ERR_VALID_ORDER_INFO_NOT_FUND=7428;<br>
ERR_VALID_REPAY_PARAM="还款接口参数校验异常";<br>
ERR_VALID_REPAY_PARAM=7431;<br>
ERR_VALID_REPAY_PLAN_NOT_LENDING="还款计划不是Lending状态";<br>
ERR_VALID_REPAY_PLAN_NOT_LENDING=7433;<br>
ERR_VALID_REPAY_PLAN="V1版还款接口还款的分期计划参数不正确";<br>
ERR_VALID_REPAY_PLAN=7437;<br>
GET_BUSINESS_TYPE_EXCEPTION="获取商业类型异常";<br>
GET_BUSINESS_TYPE_EXCEPTION=7407;<br>
GET_CALCULATION_METHOD_EXCEPTION="获取计费方式异常";<br>
GET_CALCULATION_METHOD_EXCEPTION=7403;<br>
GET_FEE_RATE_EXCEPTION="获取费率异常";<br>
GET_FEE_RATE_EXCEPTION=7402;<br>
GET_PLAN_EXCEPTION="获取分期计划异常";<br>
GET_PLAN_EXCEPTION=7405;<br>
LAST_CARD_CAN_NOT_UNBIND_CODE=7422;<br>
LAST_CARD_CAN_NOT_UNBIND_MSG="最后一张卡片不允许解绑！";<br>
LEND_INFO_NOT_EXIST_CODE=7435;<br>
LEND_INFO_NOT_EXIST_MSG="获取借款数据失败";<br>
LEND_SOURCE_NOT_EXIST_CODE=7436;<br>
LEND_SOURCE_NOT_EXIST_MSG="借款渠道不存在";<br>
SIGN_CARD_NOT_FIND_VCODE_ERROR_CODE=7413;<br>
SIGN_CARD_NOT_FIND_VCODE_ERROR_MSG="不合法的动码验证";<br>
SIGN_CARD_SCENE_NOT_EXIST_CODE=7411;<br>
SIGN_CARD_SCENE_NOT_EXIST_MSG="动码场景不存在";<br>
SIGN_CARD_SCENE_VCODE_ERROR_CODE=7412;<br>
SIGN_CARD_SCENE_VCODE_ERROR_MSG="验证码发送失败,请重试";<br>
SIGN_CARD_VALID_FAILED_MSG="卡片验证失败";<br>
SIGN_CARD_VALID_FAILED=7417;<br>
SIGN_CARD_VALID_VCODE_INFO_ERROR_CODE="动码验证信息错误";<br>
SIGN_CARD_VALID_VCODE_INFO_ERROR_CODE=7415;<br>
SPREADSHEET_ERROR_CODE=7408;<br>
SPREADSHEET_ERROR_MSG="获取试算结果失败";<br>
UN_SPPORT_BUSINESS_TYPE="不支持的账户类型";<br>
UN_SPPORT_BUSINESS_TYPE=7504;<br>
UNBIND_CARD_FAILED_CODE=7421;<br>
UNBIND_CARD_FAILED_MAG="卡片解绑出错！";<br>
USER_NOT_FOUND_ERROR="用户不存在";<br>
USER_NOT_FOUND_ERROR=4051;<br>

REPAYMENT_CARD_NO_BIND_CODE = 1006;<br>
REPAYMENT_CARD_NO_BIND_MSG = "此用户未绑过扣款卡";<br>
REPAYMENT_NOT_IN_TERM_CODE = 1009;<br>
REPAYMENT_NOT_IN_TERM_MSG = "请按顺序还款，如有分期计划在还款中状态，请稍后再试";<br>
REPAYMENT_ORDER_STATUS_ERROR_CODE = 1010;<br>
REPAYMENT_ORDER_STATUS_ERROR_MSG = "订单状态错误";<br>
REPAYMENT_TIME_ERROR_CODE = 8427;<br>
REPAY_PIPELINE_INSERT_ERROR_CODE = 8978;<br>
REPAY_PIPELINE_INSERT_ERROR_MSG = "由于系统原因还款失败。请稍后再试！";<br>
USER_NOT_FOUND_ERROR_CODE = 1001;<br>
USER_NOT_FOUND_ERROR_MSG = "用户信息不存在";<br>
REPAYMENT_ORDER_AMOUNT_ERROR_CODE = 1008;<br>
REPAYMENT_ORDER_AMOUNT_ERROR_MSG = "请确认订单金额";<br>
UID_MISSED_ERROR_CODE = 1001;<br>
UID_MISSED_ERROR_MSG = "uid不能为空";<br>
LEND_BANK_MISSED_ERROR_CODE = 1001;<br>
LEND_BANK_MISSED_ERROR_MSG = "放款行不能为空";<br>
DEBIT_CARD_MISSED_ERROR_CODE = 1001;<br>
DEBIT_CARD_MISSED_ERROR_MSG = "储蓄卡不能为空";<br>
STAGE_PLAN_NOS_MISSED_ERROR_CODE = 1001;<br>
STAGE_PLAN_NOS_MISSED_ERROR_MSG = "还款分期计划不能为空";<br>
MERCH_SERIAL_MISSED_ERROR_CODE = 1001;<br>
MERCH_SERIAL__MISSED_ERROR_MSG = "merch seiral不能为空";<br>
ORDER_NO_MISSED_ERROR_CODE = 1001;<br>
ORDER_NO_MISSED_ERROR_MSG = "order no不能为空";<br>
DUPLICATED_TRANS_LOG_ERROR_CODE = 8901;<br>
DUPLICATED_TRANS_LOG_ERROR_MSG = "重复的订单号";<br>
NOT_ALLOWED_PRODUCT_CODE = 8902;<br>
NOT_ALLOWED_PRODUCT_MSG = "未被允许的借款产品";<br>
EXCEED_LIMIT_CODE = 8429;<br>
EXCEED_LIMIT_MESSAGE = "额度不足，借款失败";<br>
DEBIT_CARD_NOT_BOUND_CODE = 8903;<br>
DEBIT_CARD_NOT_BOUND_MSG = "没有绑定借记卡";<br>
CREDIT_CARD_NOT_BOUND_CODE = 8904;<br>
CREDIT_CARD_NOT_BOUND_MSG = "没有绑定信用卡";<br>
REQUEST_CHANNEL_NOT_ALLOWED_CODE = 8905;<br>
REQUEST_CHANNEL_NOT_ALLOWED_MSG = "无效的进件渠道";<br>
REQUEST_VERIFY_TYPE_NOT_ALLOWED_CODE = 8906;<br>
REQUEST_VERIFY_TYPE_NOT_ALLOWED_MSG = "无效的验证方式";<br>
ERR_BANK_GATEWAY_GET_ORDER_STAGE_CODE = 9000;<br>
ERR_BANK_GATEWAY_GET_ORDER_STAGE_MSG = "订单不存在,请还呗联系客服";<br>