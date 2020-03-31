##1. 接口简述
授信接口，只传递用户基本信息和身份信息。
##2. 调用场景
当用户确认申请后调用还呗接口，调用用户信息推送接口。

##3. 接口要求
* https接口
* 接口采用post方式
* 数据组织采用json传递
* 数据采用gzip压缩传输
* 性能要求：
   * 5s，超过5s后判断为超时
   * 超时后会重新进行调用，前三次间隔为0s调用。
* 幂等性要求：重复请求，还呗需返回相同结果。（由于可能出现网络超时，我们没有收到还呗返回的结果，我们会重试，若您返回非200，会导致用户无法完成后续操作）<br>

##4. 接口定义
http://openapi.dev.lattebank.com/xxxchannel/applyAudit?r_c=XXX(渠道代码大写)
###4.1 请求说明
####4.1.1 参数定义

参数|	名称|	值类型|	是否可空|	备注
------------ | ------------- | ------------
user_id| 第三方机构借款用户ID|  String|  否|
apply_no|	申请请求流水号|	String|	否|	
user_base_info | 用户基本信息| Objet|否|
identification_info|身份证信息|Object|否|
face_info|人脸信息|Object|否|
contact_info|通话记录信息|Object|否|
credict_card_info|信用卡信息|Object|是|
location_info|定位信息|Object|否|
device_info|设备信息|Object|是|

**用户基本信息**

参数|	名称|	值类型|	是否可空|	备注
------------ | ------------- | ------------
name|	客户姓名|String|否|
identification_no|	身份证号|String|否|
mobile|	手机号码|String|否|
audit_mobile|	运营商手机号|String|否|认证手机号
education_enum|	用户学历|String|否|初中及以下,中专,高中，大专，本科，硕士，博士
email|	邮箱|String|否|
marriage_enum|	婚姻状况|String|否|已婚，未婚，其他
company_name|	公司名称|String|否|
company_address|	公司地址|String|否|
residence_address|家庭住址|String|否|
graduation_years|	毕业年限|Integer|是|不需要传递
birth_day|	生日|String|否|YYYYMMDD



**身份证信息**

参数|	名称|	值类型|	是否可空|	备注
------------ | ------------- | ------------
identification_no |	身份证-身份证号|String| 否
name|	身份证-姓名|String|否|
identification_address|	身份证-户籍地址|String|否|
valid_date	|身份证-有效期限|String |否| 格式 2018.10.10-2038.10.10  或者 2018.10.10-长期
nation|	民族|String|否|
issue_group|	签发机关|String|否|
idcard_front|	身份证-正面照片|String|否|图片的base64
idcard_back|	身份证-反面照片|String|否|图片的base64

**人脸信息**

参数|	名称|	值类型|	是否可空|	备注
------------ | ------------- | ------------
identify_score|	人脸识别-识别分数|Integer|是|
identify_result|	识别结果|String|是|
hack_score|	HACK评分|String|是|
face_vdeo|	人脸识别视频|String|是|
faceid_score|	人脸和身份证照片比对分数|String|是|
liveness_data|	人脸识别-原始数据|String|是|
face_photo|	人脸识别照片|String|否|图片的base64数据
supplier|	人脸识别运营商|String|是|

**通话记录信息**

参数|	名称|	值类型|	是否可空|	备注
------------ | ------------- | ------------
close_mobile|	亲密联系人电话|String|否|
close_name|	亲密联系人姓名|String|否|
common_mobile|一般联系人电话|String|否|
common_name|	一般联系人姓名|String|否|
other_contact_list|其他联系人信息列表|List|是|保存name,mobile
user_contact_list|用户通讯录信息|List|是|保存name,mobile
operator|运营商数据|Object|是|直接传递运营商原始数据由还呗解析
supplier|	通话报告运营商|String|是|具体的运营商名称

**信用卡信息**

参数|	名称|	值类型|	是否可空|	备注
------------ | ------------- | ------------
card_no	|卡号|String|是|
bank_name|	银行名称|String|是|
repayment_day|	还款日期|String|是|
limit|	额度|String|是|
card_pic|	卡照片|String|是|

**定位信息**

参数|	名称|	值类型|	是否可空|	备注
------------ | ------------- | ------------
meridian|	经度|String|否|
parallel|	纬度|String|否|
province|	省|String|是|
city|	市|String|是|
country_town|	区|String|是|
ip |ip地址|String|是|
detail_address|	详细地址|String|是|

**设备信息**

参数|	名称|	值类型|	是否可空|	备注
------------ | ------------- | ------------
brand|	手机品牌|String|是|例：oppo/vivo
model|	手机型号|String|是|例“SM——A5100
os_type|	系统类型|String|是|ANDROID/IOS
imei|	| String|是|
imsi|	| String|是|
client_mac|	手机mac地址|String|是|
client_ip|	IP地址|String|是|
device_id|	手机序列号|String|是|
idfa_uuid|	手机广告标识符IDFA|String|是|
android_id|	手机androidId|String|是|
os_version|	系统版本|String|是|
appVersion| 应用版本|String|	|
total_space|手机存储空间|Sting| |
useful_space|	手机可用存储空间|String| |
app_list|	手机装载APP列表|String| |


**运营商数据参数说明**

1、运营商数据(operator)

参数	|名称|	类型|	是否可空|	说明
------------ | ------------- | ------------
base|	用户基本信息|	JSON|	否|	
call|	通话详单|	MAP	|否|	
gprs|	月流量使用情况	|MAP|
bill|	账单详单|	MAP|	否|	
sms	|短信详单	|MAP|	否|	
familiarity_numbers	|亲情网|	List|	否|	
recharges|	充值信息|	MAP|	否|	

2、用户基本信息(operator.base)

参数|	名称|	类型|	是否可空|	说明
------------ | ------------- | ------------
mobile|	手机号|	String|	否|	
type|	运营商类型|	String|	否|	
province|	手机号归属省份|	String|	否|	
city|	手机号归属城市|	String|	否|	
open_time|	开通时间|	Date|	否|	
truename|	运营商登记姓名|	String|	否|	
id_card|	运营商登记身份证|	String|	否|	
address|	身份证地址|	String|	否	|
balance|	当前金额|	String|	否|	
certify	|是否实名	|String|	否|	实名认证、非实名认证、未确认
telPackage|	套餐名称|	String|	是|	
create_time	|认证时间|	Date|	否	|
channel|	认证渠道|	String|	是|

3、通话详单(operator.call)

参数|	名称	类型	是否可空	说明
------------ | ------------- | ------------
calltime|	通话时间|	Date|	否	|
callphone|	对方号码|	String|	否	|
thtypename|	呼叫类型|	String|	否|	语音电话（固定）
calllong|	通话时长|	String|	否|	单位秒
calltype|	主叫被叫|	String|	否|	主叫、被叫
landtype|	本地长途|	String|	否|	包括但不限于：地域类型、本地、长途、国内通话、国际通话
homearea|	通话时本号所在地|	String|	否|	
otherarea|	对方号码所在地	|String|	否|

4、月流量使用情况(operator.gprs)

参数|	名称|	类型|	是否可空|	说明
------------ | ------------- | ------------
total_flow|	本月使用的流量	|String|	否|

5、账单详单(operator.bill)

参数	|名称|	类型|	是否可空|	说明
------------ | ------------- | ------------
total_fee|	当月帐单总额|	String|	否|	
base_fee|	规定套餐费|	String|	否|	
gprs_fee|	流量费用|	String|	否	|
call_fee|	通话费用|	String|	否|	
sms_fee|	短信费用|	String|	否|

6、短信详单(operator.sms)

参数	|名称|	类型|	是否可空|	说明
------------ | ------------- | ------------
smstime|	发送/接收短信时间	|Date|	否|	
smsphone|	对方号码|	String|	否	|
homearea|	地区名 / 区号	|String|	否|	
smsfee|	短信费用|	string|	否|	
smstype|	发送 / 接收|	String|	否|	
businsstype	|短信／彩信|	String|	否|

7、亲情网信息字段（operator.familiarityNumbers）

参数	|名称|	类型|	是否可空|	说明
------------ | ------------- | ------------
memberNum|	亲情网成员长号|	String|	是|	
memberShortNum	|亲情网成员短号|	String|	是|	
memeberType|	亲情网成员类型|	String|	是|	
memeberAddDate|	亲情网成员添加时间|	String|	是|	
memeberExpireDate|	亲情网成员失效时间|	String|	是|


8、充值信息字段（operator.recharge）

参数|	名称|	类型	|是否可空|	说明
------------ | ------------- | ------------
rechargeTime|	充值时间	|Date|	是|	
rechargeAmount|	充值金额|	Double|	是|	单位：分
rechargeType|	充值方式|	String|	是|

###4.2 响应说明
####4.2.1 参数定义

参数|   值类型 |是否可空|  备注
----------- | ------------- | ------------
code | Integer |否| 200为推送成功，其他为失败，其他情况：301用户号重复 400系统错误
message| String|  否| 成功为success,失败为原因
bizContent |Object| 是| 响应数据

####4.2.1 响应示例
```
{
    "code": 200,
    "message": "success",
    "bizContent": {}
}
```






















