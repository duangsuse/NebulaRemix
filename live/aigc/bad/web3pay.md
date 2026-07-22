# DApp联动-OpenAPI

下面要介绍的是实现 **Telegram单点登录、USD稳定币支付、Web3买币地址验证+加仓数量推送** 的http接口

🎄 快捷登录平台可以提升用户留存率、简化开发难度、增加安全口碑，从而结合Web3代币经济学运营

🎉 稳定币支付是避免审查和抽成的最终兵器，可以形成较为一致的DApp消费体验，比如代开tg会员、yt频道会员等等

✨ 目前，这些API能直接与页游“大杂烩”相辅相成，让我们的DAO和 USDC.LI 更具有完成度，具备扩张的潜力

✨ 这些开发方向是所有DAO都需要的。 我们可以通过“户圣开源”为社区积攒声望，甚至因为被知名DAO使用，而获得资本市场的认可

## ThisUser登录

这是一个三端互调的认证协议，能实现OAuth所不包含的：免刷新(全js可控)、零配置、可扫码

李社区将利用它无需配置、跨设备登录的能力。

😋 现在，A是任意站点，B是启用ThisUser的网站(比如tg)，C是客户端(如js脚本)

- 如果A发现用户已登录
  - 调用 [//web3.B.site/this.user?onreturn=更新小A的主页&hole=DZ66ccff]() => "//t.me/auth_bot?start=DZ66ccff"，空返回则换个“洞口”重试
  - C打开此tg窗口，在3分钟内授权登录
  - A.site 被POST回调，绑定 `user.web3={tg:$.my_tg}` ，设置头像：
    - `{my_tg:{id: 1145141919810, avatar:"可以curl", first_name:"Pony",last_name:"Ma"}}`
    - `{plus:{points:正整数_积分充值}}` ，根据"DZ"确认站方信息
  - 绑定记录不提供 .id_real ，但虚拟ID也可以调用 [//web3.B.site/Msg.do?to=id&txt](), 尤其是 [this.user?iam=虚拟id]() 可直接认证 .my_tg 资料
- 否则
  - 让 this.user() 返回给 `hole.set&ho=DZ66ccff&secret=..` ，再返回ho给C端轮询: [//web3.A.site/_DZ66ccff]() ，其他同上
  - hole.set() 监听到对DZ的赋值，匹配 `web3 LIKE "tg:{id:(.*)"` ，生成Set-Cookie页面。😋查不到即时注册
  - C读到 `/login` 开头的行，跳转 `//web3.A.site/_DZ66ccff?onreturn=${location.href}`

onreturn= 使用的验证方式可以是 `函数地址?secret=id;time;函数名;sha256(id +time+fn+salt)` ，若salt相同则可加载id数据

## bill.fn零头支付

Telegram Stars 提供了很好的Web3.0消费体验，而我们较容易参考的则是轻量级的 @OKPay 与 AnonPay 服务。

各大冷钱包都提供了SDK来允许网页拉起支付，但有鉴于 ERC-QRCode(不可在收款码里自定义币种CA的设计!)，我们不能只去依赖各大DApp的生态，而要利用交易TxID的透明性来收款。

一种在交易中隐写信息的方法是小数位高精度（比如5u收 `5.01984 USDC` ，1984在订单服务器上唯一），也就是小费即回调。我们这样表达：

[//web3.B.site/bill.fn?cost=pay-5.00u@0x收钱地址.net&onreturn=让tgbot向你发送收货地址]()

它返回一个链接，(如 [bill.fn?id=1984&tx=待用户转账并粘贴]())

🎉 用户付款 `4.91984 USDT/USDC` 后，粘贴其Tx链接即可验证。 当然分享到Phantom里打开此页也可以弹框直付

隐藏参数是 pay-5.00u 提及的 `&ca_u=USDT:1,USDC:1的币种地址` ，可以用 `&ca_li=Lpump:.0` 来打0折 (小数点不可省略)

> ps. CA(Contract Address) 是特定币种数据库对象的地址，可以调用 `transfer()` 等协议方法完成转账、活动页结算，甚至铸币销毁等事务，这是 EVM 等“世界计算机”的功能。 若要调用Web2数据，则需用到复杂的“预言机”，因此我们只需处理稳定币的进出即可

## bill.log(verify=零头验证)和查询余额

如果不支付，只是“导入”已有的持仓，也可以小费转账验证股东身份。

对每个钱包地址，有2次机会请求一个四位数，只要请求后3分钟内转账给任意地址，即可绑定此钱包，买币获赏。

[//web3.B.site/bill.log?id=0x持仓者&verify=Tx链接&onchange=tx监听]()

盘中买币(或手工审核) 生成的余额为SBT，不能转给其他人。 比如持仓1个月没动的，通过 @SafeGuard 自动盯盘看到的买币岛友的。 初始仓位需审核

[//web3.B.site/bill.log?id=0x持仓者&dt=>时间戳&per_page=1]() => `{tx:[一页1ms之前的转账]}`

看CA余额的方法和查询log是一样的。 默认dt为now()， dt=>now 则会追加 `{nCoin:余额,A:0x0,B:受益方,CA:币种,t:时间戳, DeFi:'若为买币'}` 的系列虚拟转账

# 小鱿鱼页游模块-OpenAPI

未来，包括反贼江湖在内的社区老页游/小游戏，都应被修改为符合五大原则：

- 悬 - 悬停时做好视觉提示，看板娘提示游戏界面玩法，视效/语气多样化
- 弹 - 弹框在移动端易关闭，带有等待特效
- 滑 - 触屏上可以滑动视图，没滑到的外链/嵌入，不加载
- 新 - 高频流程不需要刷新页面，返回地图自动更新
- 鼠 - 鼠标指针特效（可自定义），点击可在地图内交互/跑动

## hextalk/v1.js 即时聊天

这个单脚本组件以 `WebSocket('usdc.li/hextalk/').{post,on}Message` 非串行-双向通信

可以粘贴发图，使用 `<p ack=tieba>😏</p>` 包含表情，`<a t=uid>时间</a>回复消息` ，首字母'#'可设话题

它使用收到的s[0] 判断调用， 'p':发送消息, 't':拉取环形队列里从t开始(,#开头)?的消息, 'P':显示消息(时间t, id,安全html), i:通过 `this.user?iam=i` 登录

客户端被js自动创建，保存'P消息记录'。 群聊默认保存3天，IP用户由 TG_ADMIN 通过 "ip 89.64.19.84 (on/off)" 管理

## baicup.php 全站checkpoint恢复

小鱿鱼网站已经受过四次攻击（恐吓+.lnk文件钓鱼、反贼江湖XSS注入、小数负数刷积分，以及那次最严重的域名劫持），而兼容PHP5时代的程序，很难完全封锁这些漏洞

因此，需要一个将网安后果最小化的备份工具：git ，针对全DZ文件树+MySQL全表导入导出。

```
//A.site/baicup_66ccff.php?
  m=* {sqldump; status;add;commit -m带日时; diff DB/*} or{status;log}

  m=push{pull;push main}  m=pitr=* {checkout; sql-checkout DB/*}
```

你可以用VSCode左下角的 remote window 编辑站点。

关于dump:  Discuz X3.5 里 DB=./config/UGC/

DB/truncate -s0 的表不导出。第一次可通过 `git clone $GH .` 来共同编辑站点，DB/可以被 `mount --bind /tmp/dir DB/` 一下

WarmWallet机器人： 免外卡免佣打赏(/vta 1usdc,物价按钮+LI, +1跟赞,周榜,岛群)  听单(收币,放币 #质押放币#完工放币,打赏小票定向)   投币井(可定向,可记录报销 归还率可记录)

`curl -X POST` => 命令/编辑/回应报错、回调钮/代写钮/tme链接钮、(PWA应用)导航和状态栏、/pjoPager两栏两词两层表格协议、/Regex关键词/gis 全局过滤(admin特权)、(Author,Bla,Cafe)=>fstr`回复${模板}和可存储ID`
[PUT状态.toml] [轮询并编辑更新] [C大群] 变量集模型参见 @MissRose_bot, @JsonDumpBot 。安装油猴脚本以查阅WebUI/强类型补齐
[Cid标签:0xcafe,babe,deaf,..私有] [C转发回复自]   `A .rg[1~n解析][0=C.cue.text] [await A.get]  B bold带格式 bigpic预览 blob图片等 C cue回复 copied转发源 tme链接 topic帖子 emojis回应`
demo: 模板 onfetch = t.me_Rpc_onCallUI({SK_TG:'1;2', TG_ME=-100n});  {let {Rpc,Bla, id:{ME}}=t.me; Rpc.autovars({A:'This.bot', C:'t.me/c/'+ME}); Bla.do({text:'hi'}); t['/filters'].push([/start/, t.HI]) }

## 余额bot

> js写一个 Telegram bot webhook ，与EventSource('/sse')联动
> 自动退出 env.TG_KING 不在群里的群。
> 接受消息时，如果包含 solscan.io 或 bscscan 的链接，收款者是KING注册好的，提取其中USDT的数量
> 
> 将其加入uid的余额(以 int nCents 结算)，若>10u 则提示返还
> /v 0 即显示余额。
> /v 10u (#话题) 时，将余额划入所回复的uid(或话题)， 并发出弹幕 "+$10 #话题 @name"
> /v 普通文字 可发弹幕
> /v 回复的文字含 "pay-5.00u-商品@x.com" 可指定金额
> 特别的，KING 可以用 /v 10u #otc 来扣减自己的余额， 用 /v paired-话题 回复用户，来绑定话题。

@bot 10u...消息  可以证明自己的持仓>10u，或者贴出全仓扫链器

低配bot 贴码入金还有个“扫链盗刷”问题，因为公链普遍不支持带memo请求。 通过 /punish_FUD 回复一个Poll "t.me/+id SCAM, -100u"，可以解决。

运维方法:
在 Cloudflare.com 创建worker脚本，设置为 TG_KING=7583080483 (@UserInfoToBot)
SK_TG=您创建的bot
SK_BST="https://go.getblock.io/1af22a50b0d1443080894533bd7c4f43 api.solana.fm/v0/transfers 080785430fde1abe07095b63da7c868abb76a16f30a307df3da52a59fa0387cc"

添加bot后，send三次 "<10u, {0x地址/SOL44字/TONspace地址}"，必须是空钱包，不能接受tg圈外的入金。

```
send三次 CA $USDT@SOL: /Es9vMFrzaCERmJfrF4H2FYD4KCoNkY11McCe8BenwNYB
CA $USDT@TON: /0:b113a994b5024a16719f69139328eb759596c38a25f59028b146fecdc3621dfe
CA $USDT@BSC: /0x55d398326f99059ff775485246999027b3197955
也可以有汇率💴
CA $USDC@SOL: https://solscan.io/token/EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v
CA $USDC 1u
特别地，
CA $LIN 0.01u 用于指定 /v 发弹幕的开销，支付到 #LIN 话题
```

bot自动回复金额币种的示例：

/0x5d27044d8e64c3a595677351e312971c925c939b6d3d7dba3bb5966b82139fa8$/
https://solscan.io/tx/5paXomiVMnGPiZkRSZVHxzkbDs4yiwfHVU9C9426GCYZcHgTief5P84cJ1dCQ1Pcae44QVHbatuZopsevJAkKz5x 
https://tonviewer.com/transaction/080785430fde1abe07095b63da7c868abb76a16f30a307df3da52a59fa0387cc

我再加个微商模块吧，允许卖令牌的，比如加群，OpenAI，VMess 作为公测

KING 私聊 `/v #话题 1 ...多行链接` 表示支付话题的每1人消耗一行文字，由bot私发。 需要提前 /start bot

`/qf q (KING可设置cid)` 用于快速转发消息，比如 q趣事 j建议。

`/qtime (20250604)` 在群(的主话题)里，这条转发的日期发生了什么？

`#w 总结` 用于AI提示(回复在前)，按字数支付余额。 /ws ..是什么， /wq 请..:  为啥.. 咋.. 同理

`#wlong ` 用于AI扩写，添加了“继续”按钮。 如果没有换行，则将内容作为 /w 的 prompt 前缀，方便群友接龙。

`#qf BV..` 用于生成缓存B站、加速GitHub、无追踪的链接，帮助匿名分享

`/q 中文` 用于全文搜索，提供 tg_bot/?q= 接口，和 /sse 一样。

这些看起来很复杂，但实现出来还是很有活的，有 AI copilot 帮忙并不麻烦。

- https://blog.cloudflare.com/durable-objects-easy-fast-correct-choose-three/#part-3-automatic-in-memory-caching

CENTS = 10e5 // BSC/=10e12
