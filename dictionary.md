# 怪力牛项目数据库字典

### 目录

- 功能概述
- 数据对照表


---

**1\. 功能概述**

&emsp;&emsp;项目主要功能包括：
包含权限管理、用户管理、菜单管理、文章管理、商品管理、订单管理等基本模块；
CMS需要适配IPAD；
支持小程序模板消息；
支持微信支付、微信退款；

---
**2\. 数据对照表**

### 通用字段说明

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| id | int(11)| 主键：该数据ID |
| listorder | int(11) | 自定义排序 |
| img_array | varchar(100) | 图片组 |
| create_time | int(11) | 创建时间 |
| update_time | int(11) | 更新时间 |
| delete_time | int(11) | 删除时间 |
| thirdapp_id | int(11) | 关联thirdapp |
| user_no | varchar(255) | 关联user |
| user_type | tinyint(2) | 用户类型0.前端2.cms |
| status | tinyint(2) | 状态:1正常；-1删除 |



### user表

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| login_name | varchar(100) | 登录名 |
| password | varchar(255) | 密码 |
| nickname | varchar(255) | 微信昵称 |
| openid | varchar(50) | 微信openid |
| headImgUrl | varchar(9999) | 微信头像 |
| role | int(11) | 权限角色 |
| primary_scope | int(11) | 权限级别：60.项目管理员/门店;30.教练;10.用户 |
| user_type | tinyint(2) | 0,小程序用户;1.门店/教练;2,cms用户; |
| user_no | varchar(255) | 用户编号 |



### user_info表

| 字段 | 类型 | 说明 |
| ------ | ------  | ------ |
| behavior | tinyint(2) | 身份:0.无;1.会员; |
| deadline | int(11) | 会员截止日期 |
| id_free | tinyint(2) | 0.无1.免费体验会员 |
| is_new | tinyint(2) | 0.无1.已奖励上级 |



### shop表-new

| 字段 | 类型 | 说明 |
| ------ | ------  | ------ |
| name | varchar(255) | 门店名称 |
| address | varchar(255) | 地址 |
| longitude | varchar(255) | 经度 |
| latitude | varchar(255) | 纬度 |
| total_area | varchar(10) | 总面积 |
| private_area | varchar(10) | 私教面积 |
| mainImg | text | 主图 |
| bannerImg | text | 多图 |
| apparatus | tinyint(2) | 固定器械：0.无1.有 |
| dumbbell | tinyint(2) | 哑铃：0.无1.有 |
| squat | tinyint(2) | 深蹲架：0.无1.有 |
| bench | tinyint(2) | 卧推：0.无1.有 |
| bicycle | tinyint(2) | 动感单车：0.无1.有 |
| run | tinyint(2) | 跑步机：0.无1.有 |
| yoga | tinyint(2) | 瑜伽垫：0.无1.有 |
| wifi | tinyint(2) | 无线网络：0.无1.有 |
| cabinet | tinyint(2) | 储物柜：0.无1.有 |
| locker | tinyint(2) | 更衣室：0.无1.有 |
| water | tinyint(2) | 饮水机：0.无1.有 |
| face | tinyint(2) | 人脸识别：0.无1.有 |
| charge | tinyint(2) | 充电宝：0.无1.有 |
| bathroom | tinyint(2) | 沐浴房：0.无1.有 |



### coach表-new

| 字段 | 类型 | 说明 |
| ------ | ------  | ------ |
| name | varchar(255) | 姓名 |
| title | varchar(255) | 称号 |
| phone | varchar(20) | 手机号 |
| good | varchar(10) | 好评率 |
| class | int(11) | 累计上课 |
| mainImg | text | 主图 |
| bannerImg | text | 宽图 |
| albumImg | text | 相册 |
| certificate | text | 证书图 |
| introduce | varchar(255) | 自我介绍 |
| expertise | varchar(255) | 专长 |
| shop_no | varchar(255) | 教练绑定门店 |



### third_app表-update

| 字段 | 类型 | 说明 |
| ------ | ------  | ------ |
| free_member | int(11) | 奖励会员天数 |



### distribution表

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| parent_no | varchar(255) | 父级NO |
| child_no | varchar(255) | 子级NO |



### label表

| 字段 | 类型 | 说明 |
| ------ | ------ | ------ |
| title | varchar(40) | 菜单名称 |
| description | varchar(255) | 描述 |
| parentid | int(11) | 父级菜单ID |
| type | tinyint(2) | 1,menu;2,menu_item;3.category;4.coupon; |



### article表

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| title | varchar(100) | 文章标题 |
| menu_id | int(11) | 首页banner,活动,关于我们,加盟我们,预约课程说明/服务协议 |
| description | varchar(255) | 描述 |
| phone | varchar(255) | 联系电话 |
| keywords | varchar(255) | 客服微信 |
| content | text | 活动规则 |
| mainImg | text | 主图 |
| passage1 | text | 关联会员卡/课程 |
| passage2 | text | 关联活动id |
| limit_time | int(11) | 限制时间 |
| check | tinyint(2) | -1.拒绝0.未审核1.通过 |



### message表-加盟我们(type=1)

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| title | varchar(255) | 姓名 |
| phone | varchar(255) | 电话 |
| description | varchar(255) | 区域 |

### message表-评论(type=2)-update

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| title | varchar(255) | 昵称 |
| mainImg | text | 头像 |
| bannerImg | text | 评论图 |
| description | varchar(255) | 评论 |
| relation_table | varchar(255) | Product |
| relation_id | varchar(255) | 课程id |
| passage1 | varchar(255) | 门店NO |
| passage2 | varchar(255) | 教练NO |



### pay_log表

| 字段 | 类型 | 说明 |
| ------ |  :------:  | ------  | 
| title | varchar(255) | 标题 |
| result | varchar(255) | 结果描述 |
| content | text | 详情 |
| type | int(11) | 类别:1.微信支付 |
| order_no | varchar(100) | 关联order |
| pay_no | varchar(255) | 关联flowLog |
| transaction_id | varchar(255) | 微信流水 |
| behavior | int(11) | 预留 |
| pay_info | varchar(999) | 支付信息 |
| prepay_id | varchar(255) | 订单微信支付的预订单id(用于发送模板消息) |
| wx_prepay_info | varchar(999) | 储存微信预支付信息，再次调起支付使用 |
| parent_no | varchar(255) | 父级订单NO |



### product表-update（type==1）

| 字段 | 类型 | 说明 |
| ------ | ------  | ------ | 
| product_no | varchar(255) | NO编号 |
| title | varchar(255) | 课程名称 |
| fit | varchar(255) | 适合人群 |
| description | varchar(255) | 特色 |
| content | text | 课程内容 |
| rule | text | 课程规则 |
| mainImg | text | 主图 |
| bannerImg | text | banner图 |
| category_id | int(11) | 关联label表 |
| type | int(11) | 1.课程2.会员卡 |
| price | decimal(10,2) | 价格 |
| max | int(11) | 人数上限 |
| standard | int(11) | 开课人数 |
| score | int(11) | 节数 |
| start_time | bigint(13) | 开课时间 |
| end_time | bigint(13) | 结束时间 |
| duration | int(11) | 有效天数 |
| course_type | tinyint(2) | 1.免费团课2.付费团课3.私教 |
| book_start_time | bigint(13) | 预约开始时间 |
| book_end_time | bigint(13) | 预约结束时间 |
| shop_no | varchar(255) | 所属门店 |
| coach_no | varchar(255) | 教练名称 |

### product表（type==2）

| 字段 | 类型 | 说明 |
| ------ | ------  | ------ | 
| o_price | decimal(10,2) | 原价 |
| duration | int(11) | 有效天数 |



### product_date表

| 字段 | 类型 | 说明 |
| ------ | ------  | ------ |
| type | tinyint(2) | 1.标准库存2.日期库存 |
| product_no | varchar(255) | 商品NO |
| sku_no | varchar(255) | SKU NO |
| price | decimal(10,2) | 价格 |
| day_time | int(11) | 0点时间戳 |
| stock | int(11) | 库存 |



### order表-update（type==1）

| 字段 | 类型 | 说明 |
| ------ | ------  | ------ |
| order_no | varchar(255) | 订单NO |
| pay | varchar(255) | pay方式详情 |
| price | decimal(10,2) | 订单金额 |
| pay_status | tinyint(2) | -1.退款0.未支付1.已支付 |
| type | tinyint(2) | 1.课程,2.会员卡,6.虚拟订单 |
| order_step | tinyint(2) | 0.正常下单,1.申请撤单,2.完成撤单,3.完结 |
| transport_status | tinyint(2) | -1.取消,0.未使用,1.预约中,2.已完结 |
| level | tinyint(2) | 层级：1.父级订单 |
| parent_no | varchar(255) | 父级订单NO |
| product_id | int(11) | 商品id |
| sku_id | int(11) | SKU id |
| title | varchar(255) | 商品名称 |
| unit_price | decimal(10,2) | 商品单价 |
| count | int(11) | 商品数量 |
| isremark | tinyint(2) | 0.未评论；1.已评论 |
| index | int(11) | 序号 |
| invalid_time | int(11) | 过期时间 |
| standard | int(11) | 可使用次数 |
| course_type | tinyint(2) | 1.免费团课2.付费团课3.私教 |
| qrcode | varchar(255) | 核销码 |
| cancel_limit_time | int(11) | 取消订单限制时间 |

### order表(type==2)

| 字段 | 类型 | 说明 |
| ------ | ------ | ------ | 
| invalid_time | int(11) | 会员卡有效期 |
| passage1 | text | 关联奖励优惠券 |



### course表-new

| 字段 | 类型 | 说明 |
| ------ | ------  | ------ |
| product_id | int(11) | 关联课程 |
| course_type | tinyint(2) | 1.免费团课2.付费团课3.私教 |
| is_book | tinyint(2) | -1.失败0.预约中1.成团 |
| book_time | int(11) | 预约时间 |
| user_no | varchar(255) | 教练NO |
| shop_no | varchar(255) | 门店 |



### order_log表-new

| 字段 | 类型 | 说明 |
| ------ | ------ | ------ |
| order_no | varchar(255) | 关联订单 |
| product_id | int(11) | 关联课程 |
| course_type | tinyint(2) | 1.免费团课2.付费团课3.私教 |
| is_use | tinyint(2) | 0.未使用1.已使用 |
| book_time | int(11) | 预约时间 |
| sign_time | int(11) | 签到时间 |
| in_time | int(11) | 进场时间 |
| out_time | int(11) | 离场时间 |
| coach_no | varchar(255) | 扫描教练 |
| shop_no | varchar(255) | 门店 |



### order_item表

| 字段 | 类型 | 说明 |
| ------ | ------  | ------ |
| order_no | varchar(255) | 订单NO |
| product_id | int(11) | 商品id |
| snap_product | text | 商品信息快照 |



### flow_log表

| 字段 | 类型 | 说明 |
| ------ | ------  | ------ | 
| type | int(11) | 1.微信支付2.余额3.积分 |
| count | int(11) | 金额 |
| trade_info | varchar(255) | 说明 |
| order_no | varchar(255) | 订单NO |
| parent_no | varchar(255) | 父级订单NO |
| level | tinyint(2) | 层级 |
| account | tinyint(2) | 0.不计算1.计算 |
| withdraw | tinyint(2) | 0.非提现1.提现 |
| withdraw_status | tinyint(2) | -1.拒绝0.待审核1.同意 |



### coupon表

| 字段 | 类型 | 说明 |
| ------ | ------  | ------ | 
| coupon_no | varchar(255) | 优惠券编号 |
| title | varchar(255) | 标题 |
| description | varchar(255) | 描述 |
| content | text | 详情 |
| mainImg | text | 主图 |
| bannerImg | text | 轮播图 |
| price | decimal(10,2) | 价格 |
| value | int(11) | 价值，可抵扣金额 |
| condition | int(11) | 使用条件，满减要求 |
| stock | int(11) | 库存 |
| sale_count | int(11) | 销量 |
| type | int(11) | 1.抵扣券 |
| valid_time | int(11) | 有效期（天） |



### user_coupon表

| 字段 | 类型 | 说明 |
| ------ | ------  | ------ | 
| type | tinyint(2) | 1.抵扣券2.折扣券 |
| use_step | tinyint(2) | 1.未使用2.已使用-1.已过期 |
| invalid_time | bigint(13) | 过期时间戳，前端记录13位 |


---