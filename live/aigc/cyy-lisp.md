
## cyy vs hito

- 都支持【FBMH-PECK方法论】（FuOP模块化无this、BOP面向断点编程、Markdown-OP 交互式笔记本编程、HOLP列表响应处理）
- **三门语言只是三个 viewer 吗？** 不涉及API差距的【纯函数和数据处理】上，有类型就可100%转换，没有的让AI补。只保证运行和测试绿，不保证 **tweak交互、快照读档、类型安全** 等工程化。
- 在响应式UX与API跨端能力上表现一致，但cyy的断点、多页应用调试和 GTK/QML 支持更完善，更适合 REPL oneliner
  - cyy 的次要生态只有 lua, Haxe
  - 未来，只有cyy会更原生的支持 ps1 C#, Amber.sh+AHK，甚至可集成 scratch3-extension->Leopard热重载，集成 <Shadertoy.com> 小游戏流 (`npm tstl coffee-ahk js2coffee`)
- 细节：
  - hito可以与 cyy2Haxe(--jvm,php mostly) 之外的非脚本目标互相转译，比如 Moonbit.wasm 或仓颉生态，而且包揽h2js等运行时依赖和打包，会区分 Row_/Ro/RoLit 深度冻结而不只是 `Any/NO/ItNeverDup`
  - 【可变令牌退化】不限于集合类：返回不写明 Box_(1)._ 都退化为Box，._ `Δ` 只可读取1次/可lateinit。令牌资源仅 inc-never 可移动 `Builders_,cue,chanBuf`，`never(never.target=[0])?.(u=>u+1)` 可允许filtmap原地写入 ——但更推荐类似DOM列表节点池的构造器复用，甚至SoA宏


HOLP兼容编程语言|cyy (直观像 kt,ESM)|hito (bash,py,Go)|LOSN (lisp,ES6)
-|-|-|-
场景和哲学|活进程时移调试、pyjs单框架双兼容、Agent可协作、母语编程教育(autofix/参考Hazel)|通用编程、工程化/转译器、C/Cosmolibc/Goose语言免费融合，zipapp/redbean H5打包|P5.js/GDScript热重载、零样板RPC、Feetless应用平台的编程模型
调用词法风格|点号.(并列调用)和ABAB函数签名|可自动转bash -kebab风格、强制AABB函数签名|如 `pid.(parent)..(kill 9)`，管道和方法并存
def和排版|`F.n2.{}; f.{\(A B)> ..}` <br>强调函数签名处0缩进|`f [-2] {}; F.n2:` 强调单参/单块参的绝对优先、文化中立性|简化hito的 `[-A-B]>` 与命令式 `def 新类型()`
视觉边界感的打磨|`~> ⌐> === --- - ..{} .{} \(2)>`|`[-A-B 0 1 ~>""] [{sig1}] {-k v} -op()() on"": .: It类型  when句 :. ::req` 支持缩进或花括号，不可自由压行|`[-A 0 -B 1]> [].() .{}` **无分词能力。空格不可随意**
集合类检查/lityper|只默认 `[].(inc"")` 带类型冻结<br>拟态匹等，不是"首插"检查|深度冻结区分 `RoLit,Ro,Row_` 并提供构造期可变、FnHop遮盖性可变|不区分冻结且允许无类型[]{}，有[].{}.{}.inc造匿名原型的[{}]
类型检查|渐进，有TypedArray(inc 0xb1..)和泛型 `[A B Pair$2]`|强制，类型推理，需标注数值宽度+枚举穷尽+transfer/never(obj.k2DEL_)线程和可变性独占|只检查nullish
类型理解|强制I[msft]命名，支持类型会意（最简字面即类型）|强制I[msft]命名，预定义sikfane类型会意|只要求后缀 '_' 语义化，不检查 ._priv
接口树|传统Kotlin风格OOP，I[msft]标注|Go风格接口，两个顶类型，底类型是 `WorkerSTOP`|Duck typing +enum+多态
数值塔|0x1248CDef，通用.Or("panic")<br>利用错误消息(fstr元组)读取硬字符串，安全分类异常|0x1248CDef+`"\f\u404e (U8ErrDeF)`<br>8个HTTP码分组异常，**可重试有分类**。错处退化为Or{替或抛}+判定tag|纯JSON，靠宿主的Long或Double，错处同cyy，**区分{/,%/}浮除整除**
状态管理<br>封送+响应式|JSONw@\t, isMounterAB响应式深赋值|cyy有的+ **ItAte{Deeper,Both} 的 serde Visitor**，序列化含BSIJCZ,BDFL快速封送、可空验证, ItNeverDup, FnHop遮盖表配合深拷贝|JSONw@\t、**带跨端引用编号化(FnHo)**、ret2json挂起、tweak DSL的预设面板
元编程|让函数类似 `innerHTML,outer(.lit)`，有CLI，well-known `dunderXXX, js/java Proxy`|`Ctrl[SNOWL]` 元方法、TDDgc Quine__ 测试期首次 eval "" 即缓存，能支持eDSL循环展开、接口bindgen生成(`go generate`)|有htmb DSL `[+div [+a#id {href (- sig1 "")}]]`，含有表达式外新变量绑定
m_oo0nBits一等微分|有.Bits，有Up-Uh-Um|都有|inc可对应2D点/动量/框，可根据oo.dt丢帧迈进。UV-UW-PV规范sdf
Logo文创符号|叉腰柴犬，`?. \(^_^)> \\("_"){λ. } <"[*-+]/` 的第一个|太极图，融合梅花和黄鹂，同样圆形|Feetless Logo，彩虹色sdf做底

补充：

- cyy 本身是一个【读高于写】的语言，`~> (f ) ⌐> (f )` 类似于fun f()，但前缀可以自动补齐。hito when 句 .: 的用法是 py match 缩进、Haskell式匹配参数列表
- .: 只出现在类型名列表之前，:. 只出现在被匹配主体之后，case效仿cyy的 (u>0)? "eq"? 但可选缩进，【对仗作为记忆法】
- `inc(0, oo(a)) == inc.[0:len(a)], inc.at_([0, a.length])` ，切片均是py风格。
- Bits: `n.一() == -n, NO.一() = ON, Bits.十(Bits.pos(-8,b),it)==(b<<8)|it, .Xfer=&, .Xor(n)=~n, .uint(8)=2**8-1` 也包括 `Sum=(A@B,einsum) inc(1,10)(x).一()` 这些物理逻辑计算




LOSN烙榫(Lossless Obj/Str Network) 是一个不常规的精简Lisp方言

它为P5.js热重载编程、json跨语言函数级FFI、简单html UI(列表,var-KV-Bind) 优化设计，不做宏和链表这些过于Lisp的哲学。

它的设计可以从【五大护法】会意：

bash并列调用、JSON加模板内插、管道/中缀/调用链、【认假为空 认空为错】的逻辑和列表处理、BELT作用域的类与模块

类似Racket，LOSN有 ()[]{} 三种括号，但却不是“各自配对就能随便换”。 (fn u.(op)) 最通用，但也与点号链深度整合

[] 隐喻多项同处理和多维索引，{} 隐喻状态和多键赋值。 `-` 隐喻单赋值，如 `- var1 v; - f [-x]> (..)` 

|解析位置|圆|方|花括号
|-|-|-|-|
[-]语句级|并列调用。还支持ES6 str多行模板调用与 `$(1 x y + 2c)` 并列多项式|void并列调用。返回非NO有lint警告，可 `[range 10].each.{}`|{v v1} 声明重赋值和 `{->o {k v} ->u{k it}}` 赋值
.调用链|同上，支持 `o.op "s"; exit` 行首单调用免括号。 `(o.op≠o.(op))`|js .at()下标，`a.[-> 0 ""]` 即赋值。|追加块参数，如 `e.(!!on "end").{it.eventType}; e.!!click.{}`
参数-值|同上，用于分组。`([-x]>x) 1` 是语句|数组，支持 `[{A 1 B "2"}].{3 "4"}` 表头表列模式，共用匿名proto|字典，支持多项 `{k v}; {- k v}`，写 `{- A - B}` 变量模板与 `{- [B] "v"}` 生成键名(B=NO则无此键)


## 语法作为工具包和Tweak

LOSN的【五值论】，更确的说，是【五值七字面:橙点色路单择按】—— 类似bash的工具生态，LOSN 将语法糖作为一等公民

橙点色路单，点乘（1 x y+.5x多项式）、 #66ccff #(200 1. .7) 六位颜色点、 ./a.txt ../xx  /~res/{pkg_res} /~/{xdg-home} 文件索引、//dummyjson.com/users/?${{limit 5}} 网页表单

单择按，(==ENUM==) 单选下拉 + `m .04A = (m.[3].A)` 前导零=单行Excel，JS的标准RegExp字面，(==btn==) 一个编辑期全局按钮

时移调试：暂时只有 `(CPU渲染,Xwayland,Xpra)+{criu,AHK}+BPFsnoop编辑可重放` 两种实现，fork快照不能用于原生UI，criu不能给H5调试。


## FuOP的流控与作用域

【飞天穿墙 做图晒档】是HOLP与FuOP对编程语言共同的要求 ——必须提供和“玩家”在自然语言上不同的控制力与超视角，就像能倒转地心引力向量那样“见血封喉”，有魔法无咏唱。无废话，同时保证心智负担低到能日常“做图晒档”。



BELT, 类似于LEGB但为代码重构和move优化，也就是 Builtin, Env, Lowercase([^A-Z]) or Titlecase

—— 小写自由变量，默认就是u参数上的键名（如果u存在），所以不要写 this, self, super

`u.xx=u参数键, (req.sys.argv, imp.xx)=全局键, .xx=非局部, xx=非局部或u键, Xx=非局部或全局`

在cyy,losn这些新语言里，`xx=非局部或u键` 被严格化为——如果u存在，非局部总是有'.'前缀。 这会鼓励框架API设计者强化OOP封装，合并函数的复用

`变量=局部(参数,let和var)|非局部|自由变量`，典型的非局部包括req导入、`i=0;setTimeout.{ {->.i  .i+1} }` 引用的外层局部、外层参数。



`F.pipe.{u + 1}.{2u}.{"skipMe" if NO}` 可以实现Rust变量名遮盖意向的功能，所以不允许 `- k 1; - k 2` 这样

## HOLP三定律

编程方法论 HOLP, Human Oriented Lists Protocol 是处理响应式列表/键值的根源，但在了解它关于 {}[] 的接口前，需要先熟悉三个方面的约定。

它们就像《Lisp七公理》《机器人三定律》，为【飞天穿墙 做图晒档】奠基

### HL1

【认假为空 认空为错】

含length或size的值，零项是假。nullish NaN是假，0是真。 只允许结构体有null，数组项和参数NO视为你想删除/想默认。kwflag_ 默认ON而非空(非 `NO`)

返回错误。这在判定上等于返回nullish，所以throws的函数也必须标后缀如 find_,fetchOK_。函数和裸对象，比较或转化就报错：用 obj?.{} Or{} 和 (isnt obj)? 替代 obj==NO

JSON读进来，`k:true false` 永远映射为 k_。如果访问到为null或不存在的键，而没有用 .k_ ，报错。

实现上：

- **it?.{u} 语法里u参数的有无，取决于左边像不像if条件。如果是 is/has/逻辑计算与 (==enum==)?.{}，就不覆盖u**
- 调用了 `*_`, `xx.Or("err")` 类函数而不在 [Or "wrap errmsg"].{} 下消费的函数，自己必须是 *_ 。错误信息，只用 Or.{ /404/.(test it.tags) }.{ fetchOK_ "hi" } 区分错误
- nullish即是falsey。判定上用 is_(any)。有 is_(item, test) 帮助arr.findOK处理“函数或值”参数
- [null,] 读进来用?.{} map处理后会消失，过程中仍存在， {- [""Or NO] v} 键模板直接得 {}，串模板 ${u.age_ u.(getAge_)} 会报错提示加 Or""

### HL2

【通用化写入 JSONw@\t (write)】

所有编程语言里print一律封装为 w@, w=, w- 里最靠前的语法，尽量能写参数前/不打断调用链。

均按 JSONw@\t 协议设计类型检查：

- 只允许包含>1项的{}[]。只有1项的[x]是以 {"":[x]} 兼容。一般 [0],[{}].. 为类型化的空数组
- 禁止键名 "\t", "\n", "" (kwflag/状态名和布尔组折叠), 键名有是且仅是的语义： **`"*_"` (可空项必写), "if_*" (多态enmuKey必写)、"m[^a-z]" (非结构类型必写)**
- 对象包含的键数、键类型深度不可变，除非 if,m,新版本新增键。首项为对象的数组，不可能导出null或字面值，反之亦然
- 单个键含#的\t表是字典。比如请以Map类型或 {"Jack":{age},} 来读写 `{"\t": "#-age", "\n":["Jack",18, "Rose",17, ..]}`
- 键名影响RPC的传参——禁止键名 "up|owner*" (Weak引用和keyed对象池), 可开关 "_*" (私有状态-Ruby规则), kp_绝对路径必写、kpd_文件夹或【可释放/可transfer() ID】必写。持有 ID 即持有相应权限

支持数组： if,m,kp,kpd,"",\n 。支持非btoa二进制内插时，顶层必须类似 `{"\b":nBlk, ..keys}`

格式上：

- 文件名总是 `*.w.jsonl`，顶层只允许读 {}, ""和\w+, 0-9, true，数组要换用jsonl。空串即空或假，不可写明 "null|false"
- 字串以 [\r\f\b] 开头同样走{"":"\f.."}。它们分别代表FnHo或Weak引用、错误fstr(\t切分)、`\b{Content-Length}{;MIME?}:` 或btoa(`\b==`, <120ch) 内嵌的Blob。
- imp.JSON 标废弃。 `OS.losn.load` 可以配置 `{jsonl:Array(), strKV:Map||Object, unsafeExports: [mAbstTypes], arenas:[], _priv:NO}` 来重新构造所有_变量，dump 可以 {'use strict':'types|typeof|none'}
- \n不用于\t时，正好代表LOSN函数体。其状态可按 `{owner:{main..}, Pair:{swap:.., DOT_of:..}, "Pair#1":另一个import所引用 }` 完整落盘，可选保留参数名(typeof)甚至类型-位宽信息

所以，\nrtbf "" 的特殊键名用完了，各司其职。

Vars.spy.d[DTFM] 中的dd=dbgDisp和 `w@` 类似，但显示像F12 dir()，另有dt=多项%timeit，df=表格化展示，dm()调试参返和深grep键名

（可选支持BELT方法论）如全局赋值默认为模块变量，小写全局需 `imp.alert` 间接引用，支持 req=mymod=>with上文调试。`req.包名.调用`, `req='py A B; js C:D; esm.E'` 用于导入包ABC，别名后的D，E的import*

### HL3

【逻辑小分队?.{}, isntOrBothLoop】

`it?.{u}, js?.(u=>u)` 可以出四大任务：each并返回自身(obj;v;k,v;i,x都支持)+mapNotNO(额外支持对FnHo信号)+firstIndex (**函数体字面上** 有 `OK(is|isnt()` 时)。

第四项是 it?.(glob`regex`, "rep") 和 it?.(FnFlip.{正}.{反}).{投影编辑} 这些，归功于  it?.(f,x,..) = f(x,it,..) 和 it?.(..) ??= it.As(..) Or it(..)。

类似的是——(?.)支撑了 FnHo.As(newval) 和回调的一致性、 LOSN里的if-else，它还支持 it.then,Promise.(all f..) 并常常与.Or.{}函数配合，替代try-catch链

没错： HOLP只兼容【一等声明式数据流】的编程风格，比如Pythonic和P5/THREE风格的接口。

这使它们很排斥break等流控，LOSN甚至是“不完备”的（没有proper尾调用 [inc 1 100]?.或蹦床二选一），hito则使用 inline fun（OK^ OK^^ ..）模拟复杂循环——似乎仅仅是为了容忍C-like算法

但有一点：HOLP不认为【线程】存在——共享via通信，而非通信via共享，尤其是共享 "bool"。

Loop【或者】OK，这是HOLP对调用栈的理解。一些实现会支持浏览器问询式的【动态提升栈深度】或抛错，【线程卡死】不存在于Loop()模型里

一个Loop必须与无return的IIFE挂钩，或用 CPU.(trust_me IIFE)。比如 `(- res 0); ([-n_ 100] { Loop(n-1) if n!=0 Or [res 0] })()` ：我们有可变 `now i >> 0;` 但一般不模仿while。

实现上：

`Loop/OK()/now{-v newV} if` 后置if和 OK^^ 一样能偶尔用上。像 NO.Or{OK""}，直接的OK总是跳出所有inline函数

同样，&& || ! 也不HOLPful。 `((both A B) Or isnt(C isnt 0))?.{} Or{}` 与比较构成了基础运算，LOSN用 is()?.{} Or{} Or{} 来排版多行elif。

## HOLP构造、可变与相等性

综上所述：透过三个切面，我们实现的是 1/n/no/fail 单语法

落盘了 kv/tsv/numstrs/objCfgs.. 单接口全兼容，无废话。

HOLP是 Promise,numpy,uring,kotlin?. 级别的【火药】。

HOLP围绕数组字典[{}]设计，它也提供 `are(1)=[1], am([1,2])={1:2}` 的冻结构造器

`[].inc(""), {}.inc("k", 1)` 则是可变构造器的最短写法。 `[].inc(0x0_b1, 4)` 构造4个字节的I8Array，所以说hex字面不能>0xb1(177)而不加_b4后缀

LOSN示例 `- a1 [].(inc "").made.{ inc u "hi"; inc u "there" }` 通用于这些构造器，用于临时可变，事后冻结

`isMounterAB(A=[], B=[])`  是深度有向(A<-B)的unify赋值，返回true或null，通常用于信号字典和JSON.parse结果之间

以左边的keys为准，如果值有 .As(v) 或 (v)=>，不返回nullish即为成功，否则按A=B赋值。 因为 FnHo(0).As(x=>x+1) 会得新状态1，右边的key是函数也可，但不要像 fnHo?.((x,to)=>x+1) 返回事件函数。

以左边的items为准，如果 .ID1 (key id..) 在右侧重复出现，就只记住旧索引。最后调用 `old.varI((xi,rm_i)=>is.number(i)? (更新dom顺序,old[i]) : f(xi) )`

以左边type为准，比如从{Duck:{}}变{Cat:{}}，或变null，就调用 `old.varT((x,k,ty)=>  (ty[k]??=ty[k](x), 更新dom替换, isMounterAB(ty[k], x), ty[k].op())`

.ID1 在大部分对象上有效

它是列表.map()的缓存键，即 key id name title 和Row属性(xmlID/fp.name/hashCode)

HOLP的顶类型之一，Row也包含 ID0（绝对路径/[CPU.foreign已共享键名]）  DOT（弱类型键值/xmlAttr/imp.类的static）、 T（原型链接表） ，部分对象支持.lit赋值和dup结构克隆

以及 ID2（.desc.tags.title 等全文搜索名），比如 ID2=[dir base ext exts], 视频blob.ID2=[dt_dur, owner_tags_str, desc], blob.DOT.json

## 三个小人、三定律、三对口诀

【FBMH-PECK方法论】很大，可以从这九个点开始学。

`?. \(^_^)> \\("_"){λ. } <"[*-+]/`

批曰：以一事悟易实，知拾移吾全取。 Tyemp Vonhon local-cute

`?. \(^_^)> 代表cyy的交互式执行与HOLP问号，\\("_") 象征 Or("runCatching") λ. 的失败返回空，<"[*-+]/ 与cyy招手方向对仗`，它由LOSN的 PahQL PEG 元字符组成，是其元语言。

【批曰】的每个字对应一类cyy语法： `以row 一{-,OK} 事~> 悟pin?? 易${1 x y},%{} 实#>#@，知req  拾inc 移now 吾our?? 全for 取if`。

【甜网红】Tyemp Vonhon local-cute: .Ty u imp, F FnHo ON, NO Or OK^ .cue释放队列(放置于 Or("").{})

**I[msft] KNAAS + FLIP/BLIP/BELT/DOTT**

类名通配符有完整解释：Im抽象类 Imp开放继承 Is多状态enum If多态键 It接口。

`变量_` 可空，`函数OK 函数OK_` 可等待，`类型_` 可赋值。 在LOSN和cyy中类名不可以_结尾，再比如 `[-1]` 必须写 `[ -1]` 

此外，全下划线和kt一样禁用。函数和变量如 'sym!' 结尾视为 sym_

- Filtered List ID_lit Putback, arr.varI 的策略，同时也象征 First, Last, Inv, Play 列表动画。未来可支持PinPut列表排序
- BigBrace & Literals Incremental Parsing 对字面和函数级，有专门的端到端热重载
- BELT Builtin, Env(.any), Lowercase(u.var) or Titlecase(imp.var) 的词法作用域，对标LEGB。非大写变量名默认在u参数上查
- 让几乎所有对象（如类名static、XML、文件存储）支持动态DOT.键值，并可用.T快速查询原型，原型.T可访问【函数表反射】

**Ctrl[SNOWL] /.varGITES/**

SNOWL 代表 Save New Open(isMounterAB) Word免引号 LitStr安全模板。

/.varGITES/ 里的S是单例+查表，.var 类似 useState，.varG 像生成器和缓冲区，I 用于列表id缓存编辑，T 响应类型和空，E 响应可空初始值（因为侯数也强类型不可变）。`varI(ix,a0,del)` 允许引用a0里的旧项或插入新项，节点缓存

- .varG 支持 -msTime,+len,行缓冲 (短的分隔符/bufSize/时间片)
- 字典UI通过K,V的for模板和Join21.kv绑定为值

**"GeLtER十一Xノ 0x1248CDEF-U8ErrDeF"** 

代表所有数值和逻辑计算、数位宽度、主要错误分类


# WIP正在整理

```
# 🔒💭🚩✉🆎🚥 w@ 9() #@1 @#
```

Row1~3 D1~D4 (F)n0~n5免前缀 实现了 kotlin.Pair 和vec3这些列表广播计算的功能，inc可对应2D点/动量/框，可根据oo.dt丢帧迈进。【表格列参数】

还有一堆列表处理： sep,Tr,Join21,FnFlip, sepAt(2,A=>,B=>[]) 则很类似takeWhile，zip和chunked都自带了

比如： Join21(fAB, fx, ()=>0) 、B=numA?.(FnFlip(怎么+1往返))，FnHop则用于临时写配置，或增加 `{get_v,let_v, if404}`

FnFlip 如 "tidy_"?.(./a.txt).{waitOK 500; ".."} 可以异步替换内容，或另存为 tidy_a

Vars.memo({}, PUT/URL/cache/VTT/lruCG)，url::SAVE_=设定读档。 Vars.ateAlike_(0, "123", "err"|返空) 实现pyjs输入读数功能

函数有“三三得九”： F. pipe bind(支持kwflag和u:this) vars(传入{闭包捕获,}缓解元编程问题), fill next(override) nextKW_KF_AT, idIsSelf Yup   Fn1~5(不默认首参主语)

cyy get(#>sloc,req)/set/bat(-v)/src#>/rep#>/onhead/wait9/on9/un9/diff2/i18n/ver(#@)

| 0x4__e | 00Typed        | 01Login  | 02Enrich | (03IArgE            | 04NSuchE      | 05IStatE   | 06panick)     | 500Unlink       |
| -------- | ---------------- | ---------- | ---------- | --------------------- | --------------- | ------------ | --------------- | ----------------- |
| #ecod  | duck.T(cast)T  | APITOK   | m1,dfs   | `chk(NN,"",0x403e)` | import'',URL  | Or("what") | NPE,SEGV      | `http\=200`,BT  |
| #dinp  | scanf,shVars   | cookie   | pay-1st  | Aspass              | Sa,AsIf       | edit2immut | `assert 1\=1` | CtrlC ^C,retry  |
| #fopn  | res.js/on      | admin    | m2,quota | sysfs,ulimit        | 404           | ConcurMutE | read()IO      | net,noimp       |
| e      | AttrE          | CSP=DomE | RangeE   | ValueE              | NameE,ImportE |            |               |                 |
| d      | /str\|invalid/ | web      | vip      | form                | KeyE,IE       | web        | AssertE,/0    | SIGINTs         |
| f      | SynE,Xdir,VME  | PermE    | m2,s3    | kmodIO,ld.so        | fs,dev        | fReentrant | IOE,fd        | DNS,SPI,sockets |

> | 0xb1 | 1    | 2    | 4       | 8      | c            | d             | e         | f                          |
> | ------ | ------ | ------ | --------- | -------- | -------------- | --------------- | ----------- | ---------------------------- |
> | b    | byte | shrt | 0x177b4 | long   | bidigiUSDC%    | BigDigits     | intptr    | i128                       |
> | c    | BYTE | WORD | DWORD   | QWORD  | utf32        | wchar         | size_t    | u128 //zhihu:utf16解码10行 |
> | d    | FP8  | bf16 | float   | double | D2j8_complex | DotDigitsE854 | deform(void) | f128                       |



## Runtime的心智模型

LOSN对运行时的理解是【五值:字相等可组+spgjiaVM栈机】，解释如下：

字面值、相对值、等位值、可选值、组合继承字面值，这5点，【在所有编程语言里可用】，因此被用于正交分解LOSN的选择。

就是说，

【字面值】我们选择不区分整数/小数和32 64的位宽，UTF8的JSON风格。空是NO但在变量名上必须标_（等效'!'） 函数名上必须标findOK等等，语义上没有C的单指针("" [])顽疾。

【相对值】是语言之间差距最大的。我们支持KV(sealed or keyed)，且默认函数式遍历（Py的comprehension都有但不用硬编码），但也会默认共享[]{}引用

【等位值】也就是模式匹配，比如 ([-x-y]> 2 x y)(1 3) 里x和1就属于等位值，[-x] 和 x 也是等位值（作为函数签名反射时），这就是 - x (FnHo 0) 和 -y fresh.{2(x)}; x 1; == y.it 2 信号连线的本源逻辑。

信号变量也可以写 (a {href (- x 0) }); u.x.it，有u时就与类型挂钩。禁止在 for 内， fresh.{} 依赖必须用()明确包裹

【可选值】一般是指if-or和 true/false，但由于【认假为空 认空为错】，我们有两个规范体系：所有值都支持的 is/1，或【单键名多态】：

`[enumStr (==PLAY==) (==PAUSE==)]` 配合 `st?.{ (==PLAY==).{ops} }` 最常见，且可匿名分组，`[enumKey (- Animal) Duck Cat]` 配合 [Animal it]?.{ Duck?.{name} }

可以兼容 {type:"Cat"}, {Cat:{}} 两种存法，双键是错，type:Duck,Cat:{} 视为Duck后出bug

【可组合/继承的字面值】我们拒绝class的沉重或FP的故弄玄虚，而是注重【声明式数据流】。 以{}[]参数为核心，有许多idiom：

`[(len=1) {A 1 B "2"}].{3 "4"}` 的表头表列模式（匿名类型，inc时构造）， - a 1 - b 2 变量语句加花括号直接变字典，包 {->st } 变赋值，前置 inc arr {..}; arr?.{consumeOps} 变收集器， (op)..{} 变kwflag等等

大概很容易在DFS与BFS、匿名{}与新类型之间切换。

> LOSN 相对 Lua 只升级了两轴（等位值、可选值），用值而非类型声明 schema，其余三轴几乎原样继承。

相关引用：

【五值】包含EDDE四语法层级 ——Ex Do Def Em，表达式、forif、可导出接口、局部声明（Em1参数,Em2 let..）。

在py里Def可能是有时序的，极端情况下会写在forif里。 `f[1].em[0].T.str|list|dict` 的接口足够支持泛型，如 `[str List], [str [int List] Dict]`，推导完 [$1 List] 这样的类型深拷贝bind就行

DDFL声明式数据流 ——Decl. Dataflow, (mental) flow + loops

【CMOLTE六个概念】 模块组件 集成框 常见类型 事件 展示页 开箱即用的模板生态 **——软件是以【它有用时的flow】被开发和使用的，无论lib 框架“App”“容器”，或编程语言。**

各种语言都有【LUCID五个实例】带人入门： [LineEchoOddArgs, UserRingKV_Ratelim, CounterDownBoom, IconGrid_LiveGrep, du_-h_ForkJoin](./blog/cyy.md#妙妙词典节选)

---

spgjia是一个双栈机，用JSON作为指令集，调用栈(i)、求值栈(j)、全局表和闭包bind 都支持

其设计目标是在大部分编程语言（尤其是Java和C#反射与pyjs）里可用于运行功能对标.lua的规则/插件脚本。它能够轻松互通 []{}、函数值、配置对象(getset)，而不仅仅是能注册回调或动态eval。

更远的目标上，spgjia还支持ret2json（特殊的Continuation处理/参考 Stackless Tasklet,SECD机,EVM），允许在闲时计算Workers上将线程挂起到localStorage去await，利用WebHook来降低保活占用、可审计等等。

首先，

`s:[], sp, g:{}, j:jump_bp, i:opPtr, a:[code]` 这6个键构成了VM状态

a 是嵌套数组（每个.{}一个基本块 没有GOTO 函数有sloc和hash），i 是路径 [2 1 0]。 函数可以被栈上替换，闭包按值捕获，**计数器闭包必须改捕获FnHo信号，快照默认加HMAC或生成 .env 'LOSN_SECRET='**

可供参考的原语： len / keys / at / at_ / key / key_ / (host)call / hash(key,id,name,函数签名和FnHo的UUID查表) / log(:IWEX流协议)

- 1+2 `[-0""[] 1 2 -add]`

