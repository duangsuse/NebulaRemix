# Cyy“汗语”编程语言

Cyy😅，是大蓝象设计、棍母实现的 高 雅 版 "C语言"，初心是帮助电棍特训中考 冲刺大专。 

Cyy是一种😇 [恶俗编程语言 esoteric-language]。友链： [唐时说的道理](https://wy-lang.org/)、[俺们东北话](https://github.com/sunny352/Awesome-FangyanLang)、[阿里P9面试特供语言](https://github.com/flaneur2020/pua-lang#syntax-overview)

神人友链☝️🤓： [LLVM上伪人的.kt](https://github.com/Mivik/kamet/blob/master/src/main/kotlin/com/mivik/kamet/ast/ConstantNode.kt)/[伪SQL](https://duangsuse.github.io/tv/黑了/db.htm)/[Lua的改良方案？](https://github.com/duangsuse/tv/blob/main/流行/从Lua重构解释器/README.md#本书的语法改良)

解析算法，用于REPL和油猴

- 支持 ':\n(\s+宽度)' 缩排和 `(nums: u>1)` 单行函数{}块，不处理 `"{consts}\"(嵌套)"`
- 合法化 val n 1 ，而且将 `fun of(n 0, z_ 1) 视为 (n: typeof 0, z_=1)` 。 `var n,b _on(kv) 视为 var n by _on(kv); var b..`
- 横杠调用链 `obj -fn hi; out -fg RED -print (kv."Amy" tag.0)(arg two) `, (JSON, `//路径.txt`)转mapOf,URL字面等
- 将 `a: List<Int> =..` 换为 `a .. :: [Int Line]`；互换了 `Int::MAX, Int.toStr()` 的语意，让 T::类名//nameas 更像 `json {fnid: [funLiteral,]}`

翻译器将捕获 `:\n  fun(名)  Azz.az  (名 名 -名 名括号..)  ::Type` 关键词。

标准库 (WIP: v2)

> 总而言之， `json.loads/dumps` + http(d), 依赖注入, fatJar(抖树压缩版的exe) 写就行了。 不需要几千行“框架”和“学生价”的IDE，缝合几个嵌套键值或者脚本罢了。

- when{}模式匹配赋值 `[Hole(any)] == [1]; mind{A,B-> (A to 2)==(1 to B) }`
- XML DSL `modeHTML { div("ContentEditable" - YY,) { a("url"){"txt"}; h1.of("#Head") } }`
- 深字串化 `BENC: BlahSeer(s:Str.AZ.eat) { fun on(e: Int)="i${v}e",retE or s()=='i',assign }`
- 根据{i:Int}属性反射/函数键名/参数类型dict实现 `Proxy(interface{K:Fn2..})`；codegen 变量树遍历实现 (`BENC: ImplFor[Pair.class]`)

### 🤓🖕 有先辈曾说过大道理「失败是成功之母，但父亲一定要是男的✍️✍️✍️」。棍母的成功离不开<ruby>棍父的自愿赠予<rt>俺拾嘞🀄️</rt></ruby>。

Cyy 的父语言(成 分 父 杂)： [科特林](https://github.com/xitu/awesome-kotlin-cn) / [排深😇](https://gist.github.com/duangsuse/519411ab618ee57350ee2df93d33f58e) & [构栏](https://gobyexample.com/variables) / [百系](https://github.com/ruanyf/simple-bash-scripts) 风格的[这个](https://ziglang.cc/post/2025/01/23/bonkers-comptime/)

…… 厚礼蟹，还有[原！神！](https://rustwiki.org/zh-CN/rust-by-example/)

棍母语言：赢梦、电棍、抽象梗、[痩猫😇](https://lab.scratch.mit.edu/)。

本项目无关于任何用相关梗网暴、破坏团结的团体，纯乐子人行为，教学目的，24小时下后不用删。

## Cyy语法框架

难绷词典（语意为单个词的项无需特殊处理，`依:组合 悟:单次赋值 实:可写` 为元字符，词频为匠心设计，正常编程时注意不到"绷急乐典孝唐"一类抽象梗😅）

```
zh
依据 class          NO null, 默认 arg:dynamic
一 val              YY true 
是 fun              NN false 
悟 override         pass Unit 
一悟 return         数 Int, _0=0::数, 依据[T Row行] 盒(一 x T)
译 when             词 String, [词 数1 俩列Col2], [数4 俩点]
译一 sealed 接口     列 Map, addr(0 /"")::[B A Addr]
实 object           来 List, inc:are()::[T行 Line], [Str Ln]
悟实 interface      来自 in, 来.get(i:箭头Indx)              
绷 runCatching      走 for{it==u}       
急 await, async{}   一意 if  
乐 import "no.*"    孤行 else  
典 package          懂完了 final  
疑悟 as             译是 is        
热梗 abstract       说的 require  
孝 super (.same)    哼哼哼 try   (需要 #!cyy -X injectThrowCallbacks)
依 data ?{fun Box}  啊啊 Error (绷{}?.走{} 仍可用, Error/Exc直接!!即抛)
译悟 enum           哼哼 catch (finally需换为用 res.cue +走{}绑定域)
已 var =inc()       擦擦边 while{} {} 踩踩背后 do (#!cyy -X blockingEvpoll)
已悟 constructor    不输别玩 break (可用ret@,伪递归代替)
已实 this           你先别急 continue, yield
类名 companion      烂梗 inner (class Row.MoreCols..) + Env::JvmSDK.As(跨平台接口) v2
dark private        棍母 typealias(CONST)
梗 open             棍母 inline val
还真是 Deprecated   那咋了 dbg.d[dtfm]
depdark internal   dark♂ protected
读 get  写 set
卷王 vararg/operator/infix
润了 external/annotation/suspend/noinline/reified  (卷王/润了 #!cyy -X bindingOrDSL)
藏在 -X deepDive 后的禁语:  /@(?<!file):/, where in out, init, final protected


en
put class       
$ val         
eg fun         
on override    
enum sealed{}
if when, ifyes if       
OK return()    
vartree object      do As, for(u in a), nums.do: u+1
puts interface      hooked super
withOK runCatching  A this, u it, An arguments[1]
mod package        
use import          impl? abstract  
ok await/async{}    impl! final  
enumtag enum        impl open  
tree data？{fun Box} except catch  
CtrlN constructor   retearly continue  
nameas companion    loop do/tailrec
used private        use1 internal  
useN public         usebytype protected  
```

## 调包仓库

类似 [rust-nursey](https://users.rust-lang.org/t/what-does-being-in-rust-nursery-mean/55463) 需要安装 `$ cyy 献 中` (cyy -Su 中)

> 随时根据网梗更新调包别名，不喜勿喷

牢达 盯动击 钉真 大狗教.试玩函数 搞核算 假人们谁懂啊 金可拿 永乐大典 指片人faceQR

几里太美 月拌描 漫波 压压 绿色奶龙 哈基米德OCR/VTT 烫实.json 蕉迟但到

## 群贤毕至-Kt反编译对照榜

```kt
sealed interface Exp {
  data class N(val n:Int): Exp
  data class Op(val AB:Pair<Exp,Exp>, val k:Char): Exp
  fun go(): Int = when(this) {
    is N -> n; is Op -> if(k=='+') AB.run { first.go() + second.go() } else TODO()
  }
}
import Line_6.Exp.*
val e=Op(N(1) to N(2), '+')
```

## 有什么意义

<p align=center>Just 4 fun!  -- 快乐就是意义。 </p>

尽管Cyy只是一个 toy/demo/PoC 样板，或者是一个Meme，它的语法和选词，是经过精心设计的，在词✍️频、内🤓涵上，都追求完美。

虽然，这些关键字选得很好笑，但它给人的直观语感、心智模型，也并非随手找着笑点就编出来了。语法的梗是按年为单位的，也是我一个独立项目的前驱--将实现互译(类似lib2to3)。

Cyy 词法经过编辑调试，可以作为实际项目的预处理器(build step)，也可以fork出自己圈的“黑话”。 它并不是 wy-lang 那样主攻朋友圈的“中文编程”，真正可以日用。

我希望，中国CS届也能出现 [QEMU&FFmpeg作者](https://bellard.org/) 这样，既有头脑，也高兴的“怪人”，而不是让中科院国企 [木兰uLang](https://zhuanlan.zhihu.com/p/345851006)、TS 之辈去“各有千秋”。

**Cyy 也可以用于编程教学目的， 如果你真心为学生--而不是[「专家的教条主义」](https://t.me/dsuse/21286)着想。**

Cyy 的技术，或许在用惯了 (代数)关系式、纯函数式、LLVM IR "滤镜图"的大佬看来不入流， 但入流，从来不是它想「说的道理」。


> <mark>软件开发者在总人口中只占相当小比例。</mark> 任何一门编程语言？哪怕已被广泛使用，大多数地球人，顶多听过它的名字，完全没法体会编程对自己值几块钱。

> WA-lang 和 [月兔语言](https://www.moonbitlang.cn/blog/rabbit-tea) 可能想说：即使在项目组内部，意义也很可能就是很简单的一句 “没干什么，只是有趣”。
<br> 你不应该为「想要某种东西」的欲望找理由。只要需求存在，技术力和资本就会应运而生。 [Creative Coding!](https://wa-lang.org/qa/)

生而平凡，没精力去积累无趣的知识。

我的工作并非懂不懂，而是已知和未知。
