##1. 接口简述
当还呗需要在用户确认审批结果即提现之后进行活体认证校验是否是本人时，通过该接口给还呗推送二次活体认证的信息。


##2. 接口要求
* https接口
* 接口采用post方式
* 数据组织采用json传递
* 性能要求：1s，超过1s后判断为超时，超时后报错，不会重新调用。


##3. 接口定义
###3.1 请求说明
####3.1.1 参数定义
参数|  名称 | 值类型| 是否必填|  备注
----------- | ------------- | ------------
order_no|  订单编号|  string|  Y| 
user_id| 51信用卡借款用户ID|  string|  N| /
idcard_info |身份信息|  object(JSON)|  N| 身份证、活体识别

**身份信息(idcard_info)**

参数|  名称|  值类型| 是否必填 | 备注
---------- | ------------- | ------------
assay_type|  活体来源 | string|  Y |目前仅支持face++
delta |face++| 上传数据的校验字符串的base64值的下载地址|  string | N| 在配合MegLive SDK使用时，用于校验上传数据的校验字符串，此字符串会由MegLive SDK直接返回
id_data_zip| 影像件打包地址| string | N| 
id_attacked| 判别身份证号码是否曾被冒用来攻击FaceID活体检测|  string | N| 取值1表示曾被攻击、取值0表示未被攻击
id_positive| ID正面|  array(String)| Y| "http://l0.51fanli.net/fun/2018/01/8d3f8020eb2855b5e5aa.jpg"
id_negative| ID反面|  array(String)| Y |"http://l0.51fanli.net/fun/2018/01/8d3f8020eb2855b5e5aa.jpg"
photo_assay| 活体识别|  array(String)| Y| 活体识别照片，一般为6张，默认第一张为最佳照片，第二张为半身照，后面是动作照<br>"http://l0.51fanli.net/fun/2018/01/8d3f8020eb2855b5e5aa.jpg"<br>"http://l0.51fanli.net/fun/2018/01/8d3f8020eb2855b5e5aa.jpg"......
name_ocr|  姓名（OCR）| string|  Y| 张三
id_number_ocr| 身份证号（OCR）| string|  Y |14120219802061530
id_due_time_ocr| 身份证有效期（OCR）| string|  Y| 1980-02-06
id_address_ocr | 身份证地址（OCR）|  string | Y |
id_sex_ocr|  性别（OCR）| string | Y| 男
id_ethnic_ocr| 民族（OCR）| string | Y |汉
id_issue_org_ocr|  签发机关（OCR）| string | Y |
scores_assay_face| 活体检测分face++| string|  Y |从face++获取的活体比对分数，取值[0,100]，数字越大越可能是同一人。
scores_range_assay_face| 活体检测分数线face++ |string|  Y| 从face++获取的不同误识率下活体比对分数阈值，还呗可自行选择，51信用卡默认按照1e-5进行比对。<br>1e-3：误识率为千分之一的置信度阈值；<br>1e-4：误识率为万分之一的置信度阈值；<br>1e-5：误识率为十万分之一的置信度阈值;<br>1e-6：误识率为百万分之一的置信度阈值。<br>阈值不是静态的，每次比对返回的阈值不保证相同，需要用同一次返回的scores_assay_face与scores_range_assay_face对比来进行判断是够通过

####3.1.2 请求示例
```
{
  "order_no": "245132241561415",
  "user_id": "111123222222",
  "idcard_info": {
    "assay_type": "1",
    "delta": "https://fanlidai.oss-cn-beijing.aliyuncs.com/face/201805/2645895151853050000",
    "id_negative": [
      "http://www.baidu.com"
    ],
    "id_positive": [
      "http://www.baidu.com"
    ],
    "photo_assay": [
      "http://l0.51fanli.net/fun/2018/01/8d3f8020eb2855b5e5aa.jpg",
      "http://l0.51fanli.net/fun/2018/01/8d3f8020eb2855b5e5aa.jpg",
      "http://l0.51fanli.net/fun/2018/01/8d3f8020eb2855b5e5aa.jpg",
      "http://l0.51fanli.net/fun/2018/01/8d3f8020eb2855b5e5aa.jpg",
      "http://l0.51fanli.net/fun/2018/01/8d3f8020eb2855b5e5aa.jpg"
    ],
    "name_ocr": "张三",
    "id_number_ocr": "14120219802061530",
    "id_due_time_ocr": "1980-02-06",
    "id_address_ocr": "中国",
    "id_sex_ocr": "男",
    "id_ethnic_ocr": "汉",
    "id_issue_org_ocr": "北京市海淀区公安局",
    "scores_assay_face": "80.11",
    "scores_range_assay_face": "1e-3"
  }
}
```
###3.2 响应说明
####3.2.1 响应示例
```
{
  "code": 200,
  "msg": ""
}
```
####3.2.2 异常说明
code为非200时，均为异常情况，请在msg中返回具体的可读的中文失败原因。
```
{
  "code": 400,
  "msg": "您请求的参数有误"
}
```








