# 版本记录
###还呗提供给第三方合作机构的接口文档
###修改记录
| 版本 | 修订时间 | 作者 | 备注 | 章节改动 | 备注 | 
| ---- | ---------- | ------ | ------ | ------ | ----- |
| 0.1   | 2019-05-21 | 李坤| 初始化文档 | 初始化文档 |初始化文档 | 
| 1.0   | 2019-06-05 | 李中宝| 更改授信方式 | 1.授信接口<br>2.授信用户附加信息接口| 1.去掉授信接口中的循环授信支持和标识<br>2.授信接口设备定位等信息拆分，通过授信用户附加信息接口拉取<br>3.新增授信用户附加信息接口| 
| 1.1   | 2019-06-10 | 李中宝| 更改加解密方式 | 接入说明 |修改请求和响应的加解密方式 | 
| 1.2   | 2019-06-26 | 李中宝| 所有章节 | 接入说明 |修正必填和非必填项，还款计划的费用明细，接口返回状态明确
| 1.3   | 2019-07-22 | 李中宝| 所有章节 | 准入接口<br>授信接口状态 |产品ID可以为空<br>授信状态枚举
| 1.4   | 2019-07-26 | 李中宝| 所有章节 | 所有接口 |根据自测重新调整接口和说明<br>所有接口加入测试地址
| 1.5  | 2019-08-08 | 李中宝| 新增其他联系人信息 | 授信接口 |新增other_contact_list用户信息
| 1.6  | 2019-08-19 | 李中宝| 新增公司地址和家庭住址 | 授信接口 |新增company_address公司地址，residence_address家庭住址
| 1.7  | 2019-08-20 | 李中宝| 修改绑卡响应结构和错误码 | 绑卡接口 |简化绑卡结果响应
| 1.8  | 2019-08-22 | 李中宝| 去除还款计划中的利率和展期 | 还款计划接口 |删除还款计划中的展期和利率
| 1.9  | 2019-08-26 | 李中宝| 修改合同列表数据结构和授信结果查询中的期数格式 | 合同列表接口，授信查询接口 |合同列表结构不再是数组，授信结果查询期数支持为支持期数的数组
| 2.0  | 2019-08-27 | 李中宝| 定位信息不能为空，定位经纬度不能为空 | 授信接口 |定位信息改为必传，定位经纬度不能为空
| 2.1  | 2019-09-03 | 李中宝|还款详情加入滞纳金和罚息 | 还款详情接口 |显示滞纳金和罚息字段
| 2.2  | 2019-09-09 | 李中宝|借款结果和还款结果，计划加入user_id | 借款结构，还款结果和还款计划 |借还款，还款计划查询和推送接口新增user_id