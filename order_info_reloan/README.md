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

参数|	名称|	值类型|	是否必填|	备注
------------ | ------------- | ------------
user_id| 第三方机构借款用户ID|  String|  Y|
apply_no|	申请请求流水号|	String|	Y|	
user_base_info | 用户基本信息| Objet|Y|
identification_info|身份证信息|Object|Y|
face_info|人脸信息|Object|Y|
contact_info|通话记录信息|Object|Y|
credict_card_info|信用卡信息|Object|Y|
location_info|定位信息|Object|Y|
device_info|设备信息|Object|Y|
third_info|第三方信息|String|N|芝麻信用数据（只给芝麻分）
other|其他|String|N|申请流程每步操作时长

**用户基本信息**

参数|	名称|	值类型|	是否必填|	备注
------------ | ------------- | ------------
name|	客户姓名|String|Y|
identification_no|	身份证号|String|Y|
mobile|	手机号码|String|Y|
audit_mobile|	运营商手机号|String|Y|认证手机号
education_enum|	用户学历|String|Y|初中及以下,中专,高中，大专，本科，硕士，博士
email|	邮箱|String|Y|
marriage_enum|	婚姻状况|String|Y|已婚，未婚，其他
company_name|	公司名称|String|Y|
company_telephone|	公司电话|String|N|
company_address|	公司地址|String|N|
education_enum|	学历|String|Y|
residence_address|家庭住址|String|Y|
graduation_years|	毕业年限|Integer|N|
email|邮箱|String|Y|
birth_day|	生日|String|Y|YYYYMMDD
income_information|收入信息|String|Y|


**身份证信息**

参数|	名称|	值类型|	是否必填|	备注
------------ | ------------- | ------------
identification_no |	身份证-身份证号|String| Y
name|	身份证-姓名|String|Y|
identification_address|	身份证-户籍地址|String|Y|
valid_date	|身份证-有效期限|String |Y| 格式 2018.10.10-2038.10.10  或者 2018.10.10-长期
nation|	民族|String|Y|
issue_group|	签发机关|String|Y|
idcard_front|	身份证-正面照片|String|Y|图片的base64
idcard_back|	身份证-反面照片|String|Y|图片的base64

**人脸信息**

参数|	名称|	值类型|	是否必填|	备注
------------ | ------------- | ------------ | ------------ | ------------ 
identify_score|	人脸识别-识别分数|Integer|Y|
identify_result|	识别结果|String|Y|
hack_score|	HACK评分|String|Y|
face_photo|	人脸识别照片|String|Y|图片的base64数据
**通话记录信息**

参数|	名称|	值类型|	是否必填|	备注
------------ | ------------- | ------------ | ------------ | ------------ 
close_mobile|	亲密联系人电话|String|Y|
close_name|	亲密联系人姓名|String|Y|
common_mobile|一般联系人电话|String|Y|
common_name|	一般联系人姓名|String|Y|
contact_cnt|	通讯录个数|Integer|Y|
other_contact_list|其他联系人信息列表|List|N|保存name,mobile
user_contact_list|用户通讯录信息|List|N|保存name,mobile
operator|运营商数据|Object|N|直接传递运营商原始数据由还呗解析
supplier|	通话报告运营商|String|N|具体的运营商名称
short_message| 短信内容 |String|N|短信记录明细

**信用卡信息**

参数|	名称|	值类型|	是否必填|	备注
------------ | ------------- | ------------ | ------------ | ------------ 
card_no	|卡号|String|Y|
repayment_day|	还款日期|String|Y|
limit|	额度|String|Y|

**定位信息**

参数|	名称|	值类型|	是否必填|	备注
------------ | ------------- | ------------ | ------------ | ------------ 
meridian|	经度|String|Y|
parallel|	纬度|String|Y|
ip |ip地址|String|Y|
detail_address|	详细地址|String|Y|

**设备信息**

参数|	名称|	值类型|	是否必填|	备注
------------ | ------------- | ------------ | ------------ | ------------ 
brand|	手机品牌|String|Y|例：oppo/vivo
model|	手机型号|String|Y|例“SM——A5100
os_type|	系统类型|String|Y|ANDROID/IOS
imei|	| String|Y|
imsi|	| String|Y|
client_mac|	手机mac地址|String|Y|
client_ip|	IP地址|String|Y|
device_id|	手机序列号|String|Y|
idfa_uuid|	手机广告标识符IDFA|String|Y|
wifi_name|	WIFI名称|String|Y|
android_id|	手机androidId|String|Y|
os_version|	系统版本|String|Y|
memory|	手机内存|String|Y|
total_space|手机存储空间|Sting| Y |
useful_space|	手机可用存储空间|String| Y |
screen_resolution|	屏幕分辨率|String| Y |
network_status|	网络状态|String| Y |
dpi|	屏幕密度|String| Y |
height|	屏幕高度|String| Y |
width|	屏幕宽度|String| Y |
app_list|	手机装载APP列表|String| N |


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

参数|	名称|	类型|	是否必填|	说明
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

###4.2 响应说明
####4.2.1 参数定义

参数|   值类型 |是否必填|  备注
----------- | ------------- | ------------
code | Integer |Y| 200为推送成功，其他为失败，其他情况：301用户号重复 400系统错误
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






















