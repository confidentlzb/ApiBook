##1. 接口简述
第三方机构调用还呗提供的接口，将用户填写的信息等推送给合作还呗。接口参数中包括订单的基本信息、申请人填写的基本信息、抓取信息、补充信息等。还呗获取到订单信息后可自行做一些处理，例如生成还呗对应订单、注册还呗用户等。最终合作还呗返回推送结果给第三方机构。

##2. 调用场景
当用户申请还呗产品，并提交基础信息、抓取信息和补充信息后，第三方机构判断是否符合还呗的要求，若用户符合还呗要求，将会生成订单，并调用合作还呗提供接口，

* 基础信息包含用户基本信息；
* 抓取信息包含手机运营商、信用卡(邮箱)、芝麻信用分等；
* 补充信息包含用户信息、关联人信息等。

##3. 接口要求
* https接口
* 接口采用post方式
* 数据组织采用json传递
* 数据采用gzip压缩传输
* 性能要求：
   * 5s，超过5s后判断为超时
   * 超时后会重新进行调用，前三次间隔为0s调用，4、5次间隔为600秒后调用，6、7次间隔1200秒后调用，8、9次间隔1800秒后再次调用。
   * 数据包一般2M左右，若还呗所要抓取认证项较多，可达到5-8M。
* 幂等性要求：重复请求，还呗需返回相同结果。（由于可能出现网络超时，我们没有收到还呗返回的结果，我们会重试，若您返回非200，会导致用户无法完成后续操作）

<font color=#DC143C >
建议：由于推单成功率会直接影响用户后续流程，因此希望还呗及时响应，接口仅做接收请求操作，将下载数据等复杂操作异步处理。
另，订单信息推送接口可根据还呗要求推送数据，分为以下三种情况：
1. 默认情况下，推送订单信息、申请人填写基本信息、身份信息、补充信息、手机信息 + 抓取原始数据 + 抓取报告；
2. 仅推送订单信息、申请人填写基本信息、身份信息、补充信息、手机信息 + 抓取原始数据，抓取报告数据可通过拉取数据接口异步拉取；
3. 仅推送订单信息、申请人填写基本信息、身份信息、补充信息、手机信息，抓取原始数据和抓取报告数据可通过拉取数据接口异步拉取；
如需特殊要求需与第三方机构对接人员沟通确认。
温馨提示：建议采用第三种方式，订单推送时仅推送订单信息、申请人填写基本信息、身份信息、补充信息、手机信息，其它信息通过拉取订单数据接口获取。</font>

##4. 接口定义
###4.1 请求说明
####4.1.1 


参数|	名称|	值类型|	是否可空|	备注
------------ | ------------- | ------------
is_reloan|	用于标识订单是否为复贷简化流程|	int	|否|	对于有复贷简化流程的还呗会有，当有该字段，并且值为1时标识为复贷简化流程，否则为正常流程
order_info|	订单基本信息|	object(JSON)|	否	|
apply_detail|	申请人填写的基本信息|	object(JSON)|	否|	
add_info|	抓取数据，其中包含：<br>1. 京东信息（jd）<br>2. 支付宝信息（alipay）<br>3. 运营商信息(mobile) <br>4. 信用卡邮箱账单（ec）：包含账单明细信息 <br>5. 储蓄卡网银信息（ibank）：包含网银交易流水信息 <br>6. 信用卡网银信息（icredit）：包含网银交易流水信息 <br>8. 社保（insure）：包含社保缴费流水信息 <br>9. 公积金（fund）<br>10. 淘宝（taobao）<br>11. 学信网（chsi）<br>|	object(JSON)|	否|	根据还呗要求的抓取项配置返回
add_report_info	|抓取数据报告，其中包含：<br>1. 京东信息（jd）<br>2. 支付宝信息（alipay）<br>3. 运营商信息(mobile) <br>4. 信用卡邮箱账单（ec）：包含账单明细信息 <br>5. 储蓄卡网银信息（ibank）：包含网银交易流水信息 <br>6. 信用卡网银信息（icredit）：包含网银交易流水信息 <br>8. 社保（insure）：包含社保缴费流水信息 <br>9. 公积金（fund）<br>10. 淘宝（taobao）|	object(JSON)|	否|	根据还呗要求的抓取项配置返回
idcard_info|	身份信息|	object(JSON)|	是|	身份证、活体识别
append_info|	补充信息|	object(JSON)|	是|	补充字段信息针对各还呗配置
contacts|	手机信息|	object(JSON)|	是|	通讯录、设备等信息
extra_info|	其他信息，其中包含：1. 第三方机构数据（fanli_data）|	object(JSON)|	是|	三方数据等信息

**订单基本信息(order_info)**

参数|	名称|	值类型|	是否可空|	备注
------------ | ------------- | ------------
order_no|	订单编号|	string|	否	|
user_name|	用户名|	string|	否|	
user_id|用户Id|	string|	否|	
user_mobile|	用户手机号码|	string|	否|	
application_amount|	申请金额(单位：分)|	long|	否|	
application_term|	申请期限|	int	|否|	
application_term_unit|	期限单位（1：天，2：月）|	int|	否|	
order_time|	订单创建时间(精确到秒)|	timestamp|	否|	
status|	订单状态|	string|	否|	
province|	省|	string|	否|	
city|	市|	string|	否|	
county|	区|	string|	否|	
bank|	还呗名|	string|	否|	
product|	产品名|	string|	否|	
product_id|	产品ID|	int|	否|	
authed_data	|认证项|	array(string)|	是|	用户已认证的第三方数据

**申请人基本信息说明（apply_detail）**

参数|	名称|	值类型|	是否可空|	备注
------------ | ------------- | ------------
bureau_user_name|	本人姓名|	string|	否|	张三
idcard_no|	本人身份证号码	|string	|否|	300000000002000000
phone_number_house|	本人手机号|	string|	否|	18100000000
asset_auto_type|	车辆情况|	枚举	|否|	1. 无车 <br>2.本人名下有车，无贷款<br>3.本人名下有车，有按揭贷款<br>4. 本人名下有车，但已被抵押<br>5.其它
user_education|	教育程度|	枚举|	是|	1. 硕士及以上<br>2. 本科<br>3.大专<br>4. 中专/高中及以下
is_op_type|	职业类别|	枚举|	是|	1. 企业主<br>2. 个体工商户<br>4. 上班人群<br>5. 学生<br>6. 无固定职业
operating_year|	经营年限|	枚举	|是|	is_op_type=1&2该字段存在；<br>1. 不足3月<br>2. 3-6个月<br>3. 7-12个月<br>4. 1-2年<br>5. 3-4年<br>6. 5年及以上
corporate_flow|	经营流水(对公流水)（单位：分）|	long|	是	|只有is_op_type=1&2该字段存在
work_period|	现单位工作年限	|枚举|	是|	只有is_op_type=4该字段存在；<br>1. 0-5个月<br>2. 6-11个月<br>3. 1-3年<br>4. 3-7年<br>5. 7年
user_income_by_card|	月工资收入（单位：分）|	long|	是|	只有is_op_type=4该字段存在
max_monthly_repayment|	可接受最高月还款额（单位：分）|	long|	否	
user_social_security|	现单位是否缴纳社保|	枚举	|是|	仅当is_op_type=4时，用户为上班族时才有<br>1. 缴纳本地社保<br>2. 无社保
monthly_average_income|	月平均收入（单位：分）|	long|	是|	只有当is_op_type=10才有
ip_address|	IP地址|	string|	是|	117.136.84.9
platform|	手机平台|	string|	否|	android / ios
device_num|	手机设备号（iOS是IDFA，Andr是IMEI）|	string|	是|	862466034655845
phone_brand	|手机品牌	|string	|是	|"vivo"
device_info|	设备信息|	string|	是|	"vivo X7Plus"
gps_location|	GPS信息|	string|	是|	102.596323,25.088703
gps_address	|GPS地址|	string|	是|	中国 浙江省 杭州市 


* 备注：基本信息字段基于商家需要返回，商家仅解析所需字段，第三方机构新增返回字段时应无影响。


**身份信息(idcard_info)**

参数|	名称|	值类型|	是否可空|	备注
------------ | ------------- | ------------
assay_type|	活体来源|	string|	否|	目前仅支持face++
delta|	face++ 上传数据的校验字符串的base64值的下载地址|	string	|是|	在配合MegLive SDK使用时，用于校验上传数据的校验字符串，此字符串会由MegLive SDK直接返回
id_data_zip|	影像件打包地址	|string|	是|	获取时，通过文件流的形式下载。<br>影像资料文件包名：文件包名(渠道编号+order_no).gz<br>各影像文件命名规范：影像类型编号（4位）+”_”+序号（2位01开始）如：0101_01<br>0102 身份证正面<br>0103 身份证反面<br>0188 人脸识别或手持身份证照片
id_positive	|ID正面	|array(String)|	否|	"http://l0.51fanli.net/fun/2018/01<br>/8d3f8020eb2855b5e5aa.jpg"
id_negative	|ID反面|	array(String)|	否|	"http://l0.51fanli.net/fun/2018/01<br>/8d3f8020eb2855b5e5aa.jpg"
photo_assay	|活体识别|	array(String)|	否|	活体识别照片，一般为6张，默认第一张为最佳照片，第二张为半身照，后面是动作照。"http://l0.51fanli.net/fun/2018/01<br>/8d3f8020eb2855b5e5aa.jpg"<br>......
name_ocr|	姓名（OCR）|	string|	否|	张三|
id_number_ocr|	身份证号（OCR）|	string|	否|	14120219802061530
id_due_time_ocr|	身份证有效期（OCR）|	string|	否	|2011.11.28-2021.11.28
id_address_ocr|	身份证地址（OCR）|	string|	否	
id_sex_ocr|	性别（OCR）|	string|	否|	男
id_ethnic_ocr|	民族（OCR）|	string|	否	|汉
id_issue_org_ocr|	签发机关（OCR）|	string|	否	
scores_assay_face|	活体检测分face++	|string	|否	|从face++获取的活体比对分数，取值[0,100]，数字越大越可能是同一人。
scores_range_assay_face|	活体检测分数线face++|	string|	否|	从face++获取的不同误识率下活体比对分数阈值，还呗可自行选择，第三方机构默认按照1e-5进行比对。<br>1e-3：误识率为千分之一的置信度阈值；<br>1e-4：误识率为万分之一的置信度阈值；<br>1e-5：误识率为十万分之一的置信度阈值;<br>1e-6：误识率为百万分之一的置信度阈值。<br>阈值不是静态的，每次比对返回的阈值不保证相同，需要用同一次返回的scores_assay_face与scores_range_assay_face对比来进行判断是够通过
face_response|	face++返回内容|	object(JSON)|	是	|face++人脸对比返回结果，具体字段见附录

**补充信息(append_info)**

参数|	名称|	值类型|	是否可空|	备注
------------ | ------------- | ------------
addr_detail|	现居住地址|	string|	是|	
addr_code_detail|	现居住地址行政区码|	string|	是|	
family_phone_number|	家庭电话|	string|	是|	
company_addr_detail|	现公司/单位地址|	string|	是	|
company_addr_code_detail|	现公司/单位地址行政区码|	string|	是	|
company_name|	现公司/单位名称|	string|	是|	
company_number|	现公司/单位电话|	string|	是|	
emergency_contact_personA_relationship|	紧急联系人A关系|	string|	是|	1:父母；2:配偶；3:兄弟；4:姐妹；
emergency_contact_personA_name|	紧急联系人A姓名|	string|	是|	
emergency_contact_personA_phone|	紧急联系人A电话|	string|	是|	
emergency_contact_personB_relationship|	紧急联系人B关系|	string|	是|	3:兄弟；4:姐妹；5:朋友；
emergency_contact_personB_name|	紧急联系人B姓名|	string|	是|	
emergency_contact_personB_phone|	紧急联系人B电话|	string|	是|	
user_marriage|	婚姻状况|	string|	是|	1:未婚；2:已婚，无子女；3:已婚，有子女；4:离异；5:丧偶；6:复婚；7:其他（请备注）；
user_email|	email|	string|	是|	jiekuan@qq.com
time_graduation|	毕业时间|	string|	是|	2000-06-01
loan_purpose|	贷款用途|	string	|是	|
repayment_way|	还款方式|	string	|是|	

* 备注：补充信息字段基于商家需要返回，商家仅解析所需字段，第三方机构新增返回字段时应无影响。
**手机信息(contacts)**

参数|	名称|	值类型|	是否可空|	备注
------------ | ------------- | ------------
device_num|	手机设备号（iOS是IDFA，Andr是IMEI）|	string|	是|	有些手机刷机后或者一些模拟器会缺失
device_info|	手机型号|	string|	否|	
platform|	手机平台|	string|	否|	Android / iOS
app_location|	手机GPS信息|	object(JSON)|	是|	用户不开启的情况会缺失
app_location.lat|	纬度|	string|	是|	26.458051
app_location.lon|	经度	|string	|是|	106.521519
app_location.address|	定位地址|	string|	是|	中国 贵州省 贵阳市 花溪区 金马大道
phone_list|	手机通讯录|	array(JSON)|	是|	用户未开权限或者没有通讯录会缺失，若需要紧急联系人信息，则该字段一定会获取到。
phone_list[].name|	手机通讯录姓名|	string|	是|	张三
phone_list[].phone|	手机通讯录号码|	string|	是|	18000000000
phone_list[].createtime|	手机通讯录获取时间|	string|	是|	
call_log|	手机通话记录|	array(JSON)|	是|	用户未开权限或者没有通讯录会缺失
call_log[].type|	通话记录类型|	string|	是|	1.来电 2.去电 3.未接
call_log[].phone|	通话记录手机号码|	string|	是|	
call_log[].date|	通话记录时间|	string|	否|	2017-01-01 12:09
call_log[].duration|	通话记录时长|	string|	否|	88
apps|	应用安装列表|	array(JSON)|	是|	用户未开权限会缺失
apps.appName|	应用名称|	string|	否|	王者荣耀
apps.versionName|	应用版本|	string|	否|	1.34.1.11
device_info_all	|手机设备信息|	string|	是|	

**其他信息(extra_info)**

参数	|名称|	值类型|	是否可空|	备注
------------ | ------------- | ------------
fanli_data|	第三方机构数据|	object(JSON)|	是|	
fanli_data.user_order_submit_time|	购物时段分布|	string|	是|	18点-24点
fanli_data.user_order_history_count|	用户历史下单数|	string|	是|	10
fanli_data.user_history_amount|	用户历史消费总金额|	string|	是|	2179.76
fanli_data.user_order_count_6|	用户最近6个月下单数|	string|	是|	4
fanli_data.user_order_count_12|	用户最近12个月下单数|	string|	是|	9
fanli_data.user_amount_6|	近六个月消费金额|	string|	是|	491.66
fanli_data.user_amount_12|	近一年下单数|	string|	是|	2130.76
fanli_data.user_platform|	用户产生过交易行为的平台	|string|	是|	京东商城,同程旅游,淘宝网
fanli_data.user_register_time|	用户注册时长|	string|	是|	1458974132
fanli_data.level|	会员等级|	string|	是|	1


####4.1.1 请求示例

###4.2 响应说明

参数|	名称|	值类型|	是否可空|	备注
------------ | ------------- | ------------
verify_info|	缺失的抓取数据var_name的数组|	array|	是|	返回空数组[] 例如：["jingdong","alipay"]
other_info|	缺失的其他信息政策模板的数组|	array|	是|	返回空数组[] 例如：["addr_detail"]

* 正常示例：


```
{
  "code": 200,
  "msg": ""
}
```
* 异常示例：

code为非200时，表示还呗认为订单数据不符合要求(例如用户信息与还呗库中信息不符)，后续也无法进行审批，因此融360端会对返回非200的订单自动置为审批拒绝。 因此请慎重返回code为非200，并在msg返回可读的失败原因。
```
{
  "code": 400,
  "msg": "存在待审批进件，拒绝重复提交",
  “bizContent”: {
    "verify_info": [
      "jingdong",
      "alipay"
    ],
    "other_info": [
      "addr_detail"
    ]
  }
}
```
##附录
**face++人脸对比返回结果(face_response)**

字段|	类型|	说明
------------ | ------------- | ------------
request_id|	string|	用于区分每一次请求的唯一的字符串。此字符串可以用于后续数据反查。除非发生404（API_NOT_FOUND )错误，此字段必定返回。
result_faceid|	Object|	有源比对时，数据源人脸照片与待验证人脸照的比对结果。此字段只在接口被成功调用时返回。result_faceid对象包含如下字段：<br>"confidence"：比对结果的置信度，Float类型，取值［0，100］，数字越大表示两张照片越可能是同一个人。<br>“thresholds”：一组用于参考的置信度阈值，Object类型，包含四个字段，均为Float类型、取值［0，100］：<br>“1e-3”：误识率为千分之一的置信度阈值；<br>“1e-4”：误识率为万分之一的置信度阈值；<br>“1e-5”：误识率为十万分之一的置信度阈值;<br>“1e-6”：误识率为百万分之一的置信度阈值。<br>请注意：阈值不是静态的，每次比对返回的阈值不保证相同，所以没有持久化保存阈值的必要，更不要将当前调用返回的confidence与之前调用返回的阈值比较。<br>关于阈值选择，以下建议仅供参考：<br>阈值选择主要参考两个因素：业务对安全的要求和对用户体验的要求。严格的阈值对应更高的安全度，但是比对通过率会下降，因此更容易出现用户比对多次才通过的情况，用户体验会有影响；较松的阈值带来一次通过率会提升，用户体验更好，但是出现非同一个人的概率会增大，安全性会有影响。请按业务需求偏好慎重选择。<br>“1e-3”阈值是较松的阈值。如果confidence低于“1e-3”阈值，我们不建议认为是同一个人；如果仅高于“1e-3”阈值，勉强可以认为是同一个人。这个阈值主要针对对安全性要求较低的场景（比如在分项业务有独立密码保护的情况下刷脸登陆app），或者原则上安全性要求高、但在一个具体流程里如果发生安全事故后果不严重的场景（比如“转账”场景安全性要求高、但是当前转账的金额很小）<br>“1e-5”、“1e－6”阈值都是较严格的阈值，一般来说置信度大于“1e-5”阈值就可以相当明确是同一个人。我们建议使用“1e-5”到关键的、高安全级别业务场景中，比如大额度的借款或者转账。<br>“1e-6”则更加严格，适用于比较极端的场景。<br>“1e-4”阈值的严格程度介于上述两项之间。<br>如果对阈值选择存疑，请咨询FaceID商务或技术支持团队。
result_ref[y]|	Object|	用户上传的参考人脸照片（对应参数image_ref[y]）与待验证人脸照的比对结果。至多有三组，分别为result_ref1、result_ref2、result_ref3，这取决于image_ref[y]的个数。此对象包含的字段与result_faceid的一致，请参考对应的描述。此字段只在接口被成功调用时返回。
id_exceptions|	object(JSON)|	本对象返回身份相关的异常情况，如身份证号码是否曾被冒用来攻击FaceID活体检测、数据源人像照片是否存在质量不佳等问题。调用者可通过此对象增进对比对结果的解读。本对象仅在有源比对时（comparison_type == 1)返回，返回包含如下字段：<br>"id_attacked"：Int类型，判别身份证号码是否曾被冒用来攻击FaceID活体检测，取值1表示曾被攻击、取值0表示未被攻击。攻击形态包括但不限于戴面具、换人攻击、软件3D合成人脸等手段。若被判别为“是”，则有可能这个身份证号码目前存在被利用的风险。注意：判别为“是”不意味身份持有者本人参与攻击，有可能其身份被他人盗用而本人无感知。<br>"id_photo_monochrome"：Int类型，数据源人像照片的色彩判断，取值1表示是黑白照片、取值0表示是彩色照片。数据源存在一部分人像照片是黑白的现象，黑白的照片会影响比对质量，一般来说将降低比对分数。本字段表达这样的异常。
time_used|	Int|	整个请求所花费的时间，单位为毫秒。除非发生404（API_NOT_FOUND )错误，此字段必定返回。
faces|	Object|	当验证照为未经detect方法检测过的照片、且return_faces参数为1时，返回本字段。本字段包含了一张或多张脸的人脸检测信息，包括人脸位置和人脸图像质量。此字段只在接口被成功调用时返回，请见参数return_faces的说明了解相关的出错情况。faces对象包含一系列人脸检测结果，每个结果包含如下字段：<br>"quality"：Float类型，表示检测出的一张人脸图像的质量，取值［0，100］，越高越好。<br>“quality_threshold”：Float类型，取值［0，100］，表示FaceId建议的人脸图像质量阈值，大于此阈值可以认定图像质量足够完成比对。<br>"rect"：这是一个Object，表示检测出的一张人脸在原始图像中的包围盒。它包含left、top、width、height四个Float类型字段来表示坐标，均取值［0，1］，小数点后数字不限3位。left、top分别表示此包围盒的左上角相对于原始图像左上角的位置的比例，width、height分别表示此包围盒相对原始图像的宽、高的比例，比如 left =0.5、top=0.5、width =0.5、height＝0.5 表示该包围盒的左上角是图像正中心，右下角是图像的右下角。orientation：Int类型，本字段仅在参数 multi_oriented_detection为“1”时才返回，表示人脸框的朝向，目前仅取0、90、180或270，单位为度，按顺时针方向。以人脸来说，0度表示通常意义上的正向、90度为头顶朝右下巴朝左、180度表示上下颠掉、270度表示头顶朝左下巴朝右。按此角度逆时针旋转图片即可获得正向的人像照。
face_genuineness|	Object|	该字段表示待比对的脸的真实性。“真实的人脸”是指待比对的人脸图像是真实人脸的拍摄，而不是戴面具的脸、通过软件人工合成的脸、或是屏幕翻拍回放的脸。本字段返回真实性检查结果以及用作参考的相关阈值。仅当待比对照为来自MegLive模块时（face_image_type == "meglive" OR "meglive_flash"）才返回此对象。本对象包括如下字段：<br>synthetic_face_confidence：Float类型，取值［0，1］，表示人脸照片为软件合成脸的置信度。<br>synthetic_face_threshold：Float类型，取值［0，1］，表示人脸照片为软件合成脸的置信度阈值。 如果synthetic_face_confidence < synthetic_face_threshold，可以认为人脸不是软件合成脸。<br>mask_confidence：Float类型，取值［0，1］，表示人脸照片为面具的置信度。<br>mask_threshold：Float类型，取值［0，1］，表示人脸照片为面具的置信度阈值。 如果mask_confidence < mask_threshold，可以认为人脸不是面具。screen_replay_confidence：Float类型，取值［0，1］，表示人脸照片为屏幕翻拍的置信度。注意：此字段只有在调用时传入了image_env参数才返回。<br>screen_replay_threshold：Float类型，取值［0，1］，表示人脸照片为屏幕翻拍的置信度阈值。 如果screen_replay_confidence < screen_replay_threshold，可以认为人脸不是屏幕翻拍。注意：此字段只有在调用时传入了image_env参数才返回。（face_image_type == "meglive_flash" 不返回此字段）<br>face_replaced：Int类型，只取值0或1。0表示未检测出换脸攻击，1表示检测出了换脸攻击。注意：换脸攻击坚持检测需要MegLive SDK 2.4.3以上版本配合，仅当在移动端上使用的活体SDK版本不低于2.4.3或为炫彩活体时，才返回该字段，否则不返回该字段。
error_msg|	String|	当请求失败时才会返回此字符串，具体返回内容见后续错误信息章节。否则此字段不存在。




