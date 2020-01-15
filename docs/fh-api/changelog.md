
#### APP-接口-调整 (2019-10-13)
* 异常修复：写日志没有权限引起的500错误
* 异常修复：部分站点ssc_cache保存异常修复
* 异常修复：部分站点blacklist保存了带http头的IP,导致PHP匹配报错的修复
* 适配配置：数据库配置文件适配老站点配置
* 接口调整：登录用户名参数：从最少6位换回最少5位

#### APP-接口-调整 (2019-09-19)
* APP版本检查接口：/system/version - 新增是否增量更新：isIncUpdate  1是   0否
* TOKEN机制完善：限定只能在登录的设备上使用，无法共享给他人

#### APP-接口-调整 (2019-09-15)
* 注册接口：/user/reg - 新增注册用户类型：regType: user: 普通用户，agent: 代理
* 游戏资讯详情：/bbs/gameDocDetail - 完善支付提示文字：hasPayTip、notPayTip
* 数据签名异常：提示数据签名异常已修复

#### APP-接口-调整 (2019-09-13)
* 获取申请活动彩金列表接口： /activity/winApplyList   取消分页查询，改为直接获取所有数据
* 用户信息接口：/user/info - 增加响应参数：hasActLottery 

#### APP-接口-调整 (2019-09-11)
* 长龙投注列表：/report/getUserRecentBet - 增加查询参数：tag： 0:普通投注，1:长龙助手，2:美女直播

#### APP-接口-调整 (2019-09-10)
* 用户信息接口：/user/info - 增加响应参数：isAgent


#### APP-接口-调整 (2019-09-09)
* 新增：网站公告接口新增弹窗开关，弹窗间隔时间字段/notice/latest `(@ugmichael)`


#### APP-接口-调整 (2019-09-06)
* 秒秒投注：/user/instantBet - 多余参数：betIssue、endTime 已删除
* 游戏资讯详情：/bbs/gameDocDetail - 查看文档不同场景及权限处理
    * canRead: true (是否能读取资讯, 如果为false, reason是原因说明)
    * reason: needLogin (需要登录：但没有登录)
    * reason: needReg (已登录：但非正式会员)
    * reason: needPay (已登录、且是正式会员，但需要支付)


#### APP-接口-调整 (2019-09-05)
* 新增接口：自定义游戏列表 /game/customGames `(@ugmichael)`
  

#### APP-接口-调整 (2019-09-04)
* 秒秒投注：/user/instantBet - 会员账号与试玩账号投注 - 整合到一个接口
* 长龙助手-我的投注：/report/getUserRecentBet - 会员账号与试玩账号接口整合
* 新增接口：签到相关 `(@ug_benjamin)`
    * 签到历史列表 - /task/checkinHistory
    * 连续签到奖励 - /task/checkinBonus
  

#### APP-接口-调整 (2019-09-03)
* 投注调整：参数：totalMoney, money, 金额必须为浮点型，且小数点为2位
* 支付通道：修复：充值通道配置图片 - 获取异常，字段是：qrcode
* 新增接口：游戏文档相关 `(@ug_benjamin)`
    * 游戏文档列表 - /bbs/gameDocList
    * 游戏文档详情 - /bbs/gameDocDetail
    * 游戏文档打赏 - /bbs/gameDocPay

#### APP-接口-调整 (2019-09-02)
* 新增接口：首页排行榜 - /system/rankingList  `(@ugmichael)`
* 会员信息接口新增余额宝开关字段：yuebaoSwitch


#### APP-接口-调整 (2019-09-01)
* 新增接口：用户余额查询 - /user/balance  `(@ug_benjamin)`
* 投注调整：新增投注来源参数 `tag`  0：自营彩、1：长龙助手、 2：美女直播

#### APP-接口-调整 (2019-08-31)
* 新增接口：秒秒彩投注 - /user/instantBet  `(@ug_benjamin)`
(说明：彩种配置里：isInstant = 1代表秒秒彩，使用次投注方法)
* 新增接口: 申请活动彩金活动列表 - /activity/winApplyList    `(@waly)`
* 新增接口: 申请彩金 - /activity/applyWin  `(@waly)`
* 新增接口: 获取申请活动彩金记录 - /activity/applyWinLog  `(@waly)`
* 新增接口: 获取申请活动彩金记录详情 - /activity /applyWinLogDetail `(@waly)`

#### APP-接口-调整 (2019-08-30)
* 红包活动：相关接口新增 - `(@waly)`
    * 红包活动：红包活动信息 - /activity / redBagActivityInfo
    * 红包活动：红包详情 - /activity/redBagDetail
    * 红包活动：领红包 - /activity/getRedBag
* 新增接口：APP在线人数 - /system/onlineCount - `(@ugmichael)`
* 首页推荐游戏新增字段：supportTrial-是否支持试玩 - //game/homeRecommend​ - `(@ugmichael)`

#### APP-接口-调整 (2019-08-29)
* 新增接口：真人额度转换记录 - /real/transferLogs - `(@ugmichael)`
* 在线支付接口更改为GET请求 - /recharge/onlinePay - `(@ugmichael)`
* 会员信息接口新增玩家玩过真人游戏ID - /user/info - `(@ugmichael)`


#### APP-接口-调整 (2019-08-28)
* 新增接口：手工充值存款 - /recharge/transfer  `(@ug_benjamin)`
* 调整接口：所有支付通道 - 响应银行列表Json字符串变Json
* 调整常用登录地接口：国家/省市/城市, 目前最低支持只设置到国家
* 后端调整
    * 请求频率限制：支持配置自定义错误信息：limitMsg


#### APP-接口-调整 (2019-08-27)
* 新增接口：获取所有支付通道 - /recharge/cashier  `(@ug_benjamin)`
* 接口响应数据格式：调整为统一使用APP响应格式
* 长龙助手-我的投注：注单ID参数：id 换为 betId


#### 原生APP-接口-调整 (2019-08-26)
* 重复登录：响应状态码调整为失败状态
* 长龙助手-我的投注： /report/getUserRecentBet - `(@ug_benjamin)`
* 优惠活动接口添加图片样式字段 - /system/promotions - `(@ugmichael)`
* 豪华版优惠活动接口添加图片样式字段 - /system/promotionsPlus - `(@ugmichael)`

#### 原生APP-接口-调整 (2019-08-26)
* 推荐收益：相关接口新增 - `(@parker)`
* 推荐收益：推荐信息 - /team/inviteInfo
* 推荐收益：代理域名 - /team/inviteDomain
* 推荐收益：下线列表 - /team/inviteList
* 推荐收益：下线投注报表 - /team/betStat
* 推荐收益：下线投注记录 - /team/betList
* 推荐收益：下线存款报表 - /team/depositStat
* 推荐收益：下线存款记录 - /team/depositList
* 推荐收益：下线提款报表 - /team/withdrawStat
* 推荐收益：下线提款记录 - /team/withdrawList
* 推荐收益：下线真人投注报表 - /team/realBetStat
* 推荐收益：下线真人投注记录 - /team/realBetList


#### 原生APP-接口-调整 (2019-08-25)
* 任务中心：相关接口新增 - `(@waly)`
    * 任务中心：任务中心任务列表 - /task/center
    * 任务中心：积分账变列表 - /task/creditsLog
    * 任务中心：积分兑换接口 - /task/creditsExchange
    * 任务中心：领任务接口 - /task/get
    * 任务中心：领取奖励接口 - /task/reward


#### 原生APP-接口-调整 (2019-08-25)
* 新增在线充值接口 - /recharge/onlinePay - `(@ugmichael)`



#### 原生APP-接口-调整 (2019-08-23)
* 余额宝：相关接口新增 - `(@ugmichael)`
    * 余额宝：统计数据 - /yuebao/stat
    * 余额宝：转入转出 - /yuebao/transfer
    * 余额宝：转入转出记录 - /yuebao/transferLogs
    * 余额宝：收益报表 - /yuebao/profitReport
* 系统配置接口：增加 yuebaoName 字段（余额宝名称）
* 开奖接口新增字段：zodiacNums（六合彩-生肖），fiveElements（六合彩-五行）
* 真人额度手动转入转出


#### 原生APP-接口-调整 (2019-08-22)
* 新增接口：长龙助手 - /game/changlong `(@ug_benjamin)`
* 新增接口：提款申请 - /withdraw/apply `(@ugmichael)`
* 接口变更：系统配置接口增加字段：
    * 最小提款金额：minWithdrawMoney
    * 最大提款金额：maxWithdrawMoney


#### 原生APP-接口-调整 (2019-08-21)
* 获取系统配置接口，增加字段mobile_logo(手机端logo)


#### 原生APP-接口-调整 (2019-08-15)

* 投注接口修复：投注连码玩法-后端接口报错问题修复
* 登录接口修复：同一IP登录用户限制不准确修复
* 用户信息接口：完善用户未读站内信条数字段：unreadMsg
* 图片验证码接口：WAP客户端设备也展示为: APP响应格式
* 响应格式调整：UserAgent（设备信息）识别有误差、现优先默认响应APP格式、之前为PC
* 后端文件日志：记录用户请求的UserAgent信息，登录、注册是否记日志 调整为可配置
* 后端变更日志：
    1. SQL变更日志记录：路径：Docs\mysql\change\，命名推荐：产品+版本+上线截止日期
    2. 接口变更日志记录：路径：Docs\docsify\docs\fh-api\changeLog.md，变更描述：尽量友好、清晰


#### 原生APP-接口-调整 (2019-08-14)

* 登录问题  ：APP登录后访问H5，会被提示异常登录问题，已被修复


#### 原生APP-接口-调整 (2019-08-11)

* MYSQL更新：UPDATE方法：更新数据和更新条件，有相同参数覆盖引起的问题，已用前缀区分
* 站内信接口：获取会员站内信修复，使用缓存的用户层级、导致无法获取更新层级后的信息
* 建议反馈接口：主反馈使用status判断是否已读，反馈详情获取已修复，前端格式待调整



#### 原生APP-接口-调整 (2019-08-10)

* 注册接口：邀请人参数传递问题, 修改为使用id
* 留言反馈接口：
    1. 修复初次留言提示已有留言正在处理问题
    2. 获取某条反馈记录的回复详情 异常修复 
* 投注接口：投注中奖 不派彩问题



#### 原生APP-接口-调整 (2019-08-08)

* 系统参数接口：返回参数已过滤
* 用户信息接口：补充：wx 字段
* 取消投注接口：相关报错与统计已修复
* 注册接口：IP限制描述根据后台配置展示


#### 豪华APP-接口-新增 (2019-08-05)

* 后台新增豪华APP上传真人游戏LOGO功能
* 后台新增上传APP启动图功能
* 新增获取APP启动图接口


#### 豪华APP-接口-修复 (2019-08-04)

* 后台新增豪华APP上传真人游戏LOGO功能
* 后台新增上传APP启动图功能
* 新增获取APP启动图接口

<!-- 

<a name="4.9.4"></a>
## [4.9.4](https://github.com/docsifyjs/docsify/compare/v4.9.2...v4.9.4) (2019-05-05)

<a name="4.9.4"></a>
## [4.9.4](https://github.com/docsifyjs/docsify/compare/v4.9.2...v4.9.4) (2019-05-04)



<a name="4.9.2"></a>
## [4.9.2](https://github.com/docsifyjs/docsify/compare/v4.9.1...v4.9.2) (2019-04-21)
-->  