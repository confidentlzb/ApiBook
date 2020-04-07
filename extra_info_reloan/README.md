##1. 授信用户附加信息接口

##2. 调用场景
第三方机构调用授信接口后，还呗会拉取用户的附加信息。




##3. 接口要求
* https接口
* 接口采用post方式
* 数据组织采用json传递



##4. 接口定义
###4.1 请求说明
####4.1.1 参数定义

参数|	名称|	值类型|	是否必填|	备注
------------ | ------------- | ------------
user_id| 第三方机构借款用户ID|  String|  Y|
apply_no|	申请请求流水号|	String|	Y|	

###4.2 响应说明
####4.2.1 参数定义

参数|	名称|	值类型|	是否必填|	备注
------------ | ------------- | ------------
user_id| 第三方机构借款用户ID|  String|  Y|
apply_no|	申请请求流水号|	String|	Y|	
face_info|人脸信息|Object|Y|
contact_info|通话记录信息|Object|Y|
credict_card_info|信用卡信息|Object|N|
location_info|定位信息|Object|N|
device_info|设备信息|Object|N|

**人脸信息**

参数|	名称|	值类型|	是否必填|	备注
------------ | ------------- | ------------
identify_score|	人脸识别-识别分数|Integer|N|
identify_result|	识别结果|String|N|
hack_score|	HACK评分|String|N|
face_vdeo|	人脸识别视频|String|N|
faceid_score|	人脸和身份证照片比对分数|String|N|
liveness_data|	人脸识别-原始数据|String|N|
face_photo|	人脸识别照片|String|Y|图片的base64数据
supplier|	人脸识别运营商|String|N|

**通话记录信息**

参数|	名称|	值类型|	是否必填|	备注
------------ | ------------- | ------------
close_mobile|	亲密联系人电话|String|Y|
close_name|	亲密联系人姓名|String|Y|
common_mobile|	一般联系人电话|String|Y|
common_name|	一般联系人姓名|String|Y|
user_contact_list|用户通讯录信息|List|N|保存name,mobile
operator|运营商数据|Object|Y|直接传递运营商原始数据由还呗解析
supplier|	通话报告运营商|String|Y|具体的运营商名称

**信用卡信息**

参数|	名称|	值类型|	是否必填|	备注
------------ | ------------- | ------------
card_no	|卡号|String|N|
bank_name|	银行名称|String|N|
repayment_day|	还款日期|String|N|
limit|	额度|String|N|
card_pic|	卡照片|String|N|

**定位信息**

参数|	名称|	值类型|	是否必填|	备注
------------ | ------------- | ------------
meridian|	经度|String|N|
parallel|	纬度|String|N|
province|	省|String|N|
city|	市|String|N|
country_town|	区|String|N|
ip |ip地址|String|N|
detail_address|	详细地址|String|N|

**设备信息**

参数|	名称|	值类型|	是否必填|	备注
------------ | ------------- | ------------
brand|	手机品牌|String|N|例：oppo/vivo
model|	手机型号|String|N|例“SM——A5100
os_type|	系统类型|String|N|ANDROID/IOS
imei|	| String|N|
imsi|	| String|N|
client_mac|	手机mac地址|String|N|
client_ip|	IP地址|String|N|
device_id|	手机序列号|String|N|
idfa_uuid|	手机广告标识符IDFA|String|N|
android_id|	手机androidId|String|N|
os_version|	系统版本|String|N|
appVersion| 应用版本|String|	|
total_space|手机存储空间|Sting| |
useful_space|	手机可用存储空间|String| |
app_list|	手机装载APP列表|String| |


**运营商数据参数说明**

1、运营商数据(operator)

参数	|名称|	类型|	是否必填|	说明
------------ | ------------- | ------------
base|	用户基本信息|	JSON|	Y|	
call|	通话详单|	MAP	|Y|	
gprs|	月流量使用情况	|MAP|
bill|	账单详单|	MAP|	Y|	
sms	|短信详单	|MAP|	Y|	
familiarity_numbers	|亲情网|	List|	Y|	
recharges|	充值信息|	MAP|	Y|	

2、用户基本信息(operator.base)

参数|	名称|	类型|	是否必填|	说明
------------ | ------------- | ------------
mobile|	手机号|	String|	Y|	
type|	运营商类型|	String|	Y|	
province|	手机号归属省份|	String|	Y|	
city|	手机号归属城市|	String|	Y|	
open_time|	开通时间|	Date|	Y|	
truename|	运营商登记姓名|	String|	Y|	
id_card|	运营商登记身份证|	String|	Y|	
address|	身份证地址|	String|	Y	|
balance|	当前金额|	String|	Y|	
certify	|是否实名	|String|	Y|	实名认证、非实名认证、未确认
telPackage|	套餐名称|	String|	N|	
create_time	|认证时间|	Date|	Y	|
channel|	认证渠道|	String|	N|

3、通话详单(operator.call)

参数|	名称	类型	是否可空	说明
------------ | ------------- | ------------
calltime|	通话时间|	Date|	Y	|
callphone|	对方号码|	String|	Y	|
thtypename|	呼叫类型|	String|	Y|	语音电话（固定）
calllong|	通话时长|	String|	Y|	单位秒
calltype|	主叫被叫|	String|	Y|	主叫、被叫
landtype|	本地长途|	String|	Y|	包括但不限于：地域类型、本地、长途、国内通话、国际通话
homearea|	通话时本号所在地|	String|	Y|	
otherarea|	对方号码所在地	|String|	Y|

4、月流量使用情况(operator.gprs)

参数|	名称|	类型|	是否必填|	说明
------------ | ------------- | ------------
total_flow|	本月使用的流量	|String|	Y|

5、账单详单(operator.bill)

参数	|名称|	类型|	是否必填|	说明
------------ | ------------- | ------------
total_fee|	当月帐单总额|	String|	Y|	
base_fee|	规定套餐费|	String|	Y|	
gprs_fee|	流量费用|	String|	Y	|
call_fee|	通话费用|	String|	Y|	
sms_fee|	短信费用|	String|	Y|

6、短信详单(operator.sms)

参数	|名称|	类型|	是否必填|	说明
------------ | ------------- | ------------
smstime|	发送/接收短信时间	|Date|	Y|	
smsphone|	对方号码|	String|	Y	|
homearea|	地区名 / 区号	|String|	Y|	
smsfee|	短信费用|	string|	Y|	
smstype|	发送 / 接收|	String|	Y|	
businsstype	|短信／彩信|	String|	Y|

7、亲情网信息字段（operator.familiarityNumbers）

参数	|名称|	类型|	是否必填|	说明
------------ | ------------- | ------------
memberNum|	亲情网成员长号|	String|	N|	
memberShortNum	|亲情网成员短号|	String|	N|	
memeberType|	亲情网成员类型|	String|	N|	
memeberAddDate|	亲情网成员添加时间|	String|	N|	
memeberExpireDate|	亲情网成员失效时间|	String|	N|


8、充值信息字段（operator.recharge）

参数|	名称|	类型	|是否必填|	说明
------------ | ------------- | ------------
rechargeTime|	充值时间	|Date|	N|	
rechargeAmount|	充值金额|	Double|	N|	单位：分
rechargeType|	充值方式|	String|	N|


