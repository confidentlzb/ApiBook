##1. 接口简述
第三方机构调用还呗提供的合同拉取接口，接口中告知还呗要获取的订单号和当前流程涉及的合同类型。最终还呗返回合同URL。 此外，若用户跳转到合同页面中进行操作，用户操作完成后，还呗需要跳转回第三方机构。

##2. 调用场景
 第三方机构的页面中默认勾选展示合同名称，用户点击后，可支持展示还呗的合同页面：

* 补充信息填写页：一般展示一些数据授权合同
* 绑卡页：一般展示绑卡相关合同
* 审批确认页：一般展示借款合同
* 账单页：一般展示借款合同 前三个页面支持配置最多3个合同，账单页仅支持1个合同。

合同要求：

* 法务要求：合同中需根据参数uid，在合同末尾拼接用户uid和手机号。若机构的合同中包含了用户的身份证信息，则可忽略该要求。 处理流程：机构需要按照以下合同接口中的格式解析，将uid和手机号拼接到合同末尾。若机构获取不到手机号时，则无需拼接手机号到合同中。 拼接的合同样式如下：
```
您的账号ID：${user_id}
您的登录手机号：13639343434
```
* 法务要求，为了记录用户签订的合同内容，我们需要获取到合同链接中的内容。当合同中包含多个合同链接时，我方无法记录。因此需要合同中不能包含其他合同的链接，需将链接内容写入合同中。

##3. 接口要求
http://openapi.dev.lattebank.com/xxxchannel/contractList?r_c=XXX(渠道代码大写)

* https接口
* 接口采用post方式
* 数据组织采用json传递。
* 性能要求：1s，超过1s后判断为超时。超时后不会重试。

##4. 接口定义
###4.1 请求说明
####4.1.1 具体参数定义

参数|  名称|  值类型| 是否必填|  备注
----------- | ------------- | ------------
user_id| 第三方机构借款用户ID|  String|  Y|第三方用户ID，需要将用户id添加到合同的最下方
return_url|  合同接口url| String|  N| 合同回调地址
contract_page| 合同所属的页面 |Integer| Y| 1:补充信息页 2:审批结果页 3:绑卡页面 4:账单页面
contract_pos|  合同在页面内的顺序展示| Integer| Y |示例值:1
amount|  用户选择的金额/分| Long|  N| 
term|  用户选择的期限| Integer| N| /
term_unit| 期限单位|  Integer| N| 1：天 2：月
rate | 利率|  String|  N |0.45

####4.1.3 请求示例
```
{
  "user_id": "32857782",
  "return_url": "xxxxxxx",
  "contract_page": 2,
  "contract_pos": 1
}
```
###4.2 响应说明
####4.2.1 响应参数定义
参数|  名称|  值类型| 是否可空 | 备注
---------- | ------------- | ------------
title| 合同标题|  String | Y| /
contract_url|  合同url| String|  Y| /
####4.2.2 响应示例
```
{
  "code": 200,
  "message": "",
  “bizContent”: {
      "title": "借款协议1",
      "contract_url": "xxxxxxxxxxx"
    }
}
```
####4.2.3 异常说明
code为非200时，均为异常情况，请在msg中返回具体的可读的中文失败原因。
```
{
    "code": 400,
    "message": "您请求的参数有误"
}
```


