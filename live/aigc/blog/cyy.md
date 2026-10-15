# cyy编程语言

> `?. \(^_^)> \\("_"){λ. }` the fluent syntax with 90% Kotlin + practical ES6 interop, compiles to Py/JS/Lua, good 4 GTK/QML/TS AOP.

cyy 主攻面向字典(am/imp)、列表(are) 和箭头函数(`- f (x)> x+1`, FnHo) 的渐近强类型、类型推理、REPL-first 的编程环境，其特性相当适合从 Kotlinlang.org 迁移的开发者，同时，cyy是ES6的巧妙扩充（函数式集合、信号变量）。

cyy 希望在保留 H5/Bun.ts/libc 均有兼容的通用编程的同时，对JSON、GTK4和QML的对象子树有跨语言的eDSL语法支持，且以两框架Lua热补丁/热重载的效率，测试语言完成度、完成自举转译等等。cyy 受到 Nim (✨Nitter, Pixie), AHK Automator, Wren (Crafting Interpreters), Monkey (✨GoLang Book) 编程语言的启发。在发布页则上借鉴了 wy-lang, bend2, [lang_snippets](https://themaster1127.github.io/HTVM/web-ui/)

cyy 的 REPL-first 至少可以达到ChatUI的水准。比如，可以编辑旧段落并重算、固化测试输出(`%%init`, `%%test`)，可读档并回到旧段落，允许中断、耗时太长时显示耗时和资源，可清除输出、自动“重执行到此处”等等。

尽管可以接入标准的 Android/PDM/LuaRocks 包管理发布，它也更倾向于JBang式的依赖管理，并鼓励你“录制”一段REPL为独立文件的函数或动作(Squeaky fun)。你同样能复制 w@打印，在test{}块后看到其 %{长文本输出}。

## 简洁哲学

cyy 不是另一个 “C-Like 编程语言”，尽管它用起来也不会劣于 “工业级编译型语言”。它用于【读和修改已经实现的功能】【调试与拓宽脚本的运行平台】，而不是取代py/js的单文件快速原型。将以此为第一性原理设计。

尽管【运行期即编辑期】【像玩音游一样编程】，如 `#@1 AI重录选区, set{} %{prompt覆写}, #> #< 可点击源码中的按钮/快速字面配置, (TUI工具界面) git先提交后命名`，是它的显著特征，cyy 也和 Java, JS 一样有着很普通的语法。

以下六个关键字均有2个版本，“实”字是4个语言特性的助记词。

```
依一是，悟易实 req -/一/OK/Or fun set-our ${1 x y} ?.{}.().[]

以时移，全取吾 row inc new  for([]>>$u){u} .if{}Or{} I[msft]十一Xノ

语言中立版cyy： #> Lang emo   #> Lang zh-on-emo help_me  （即：CtrlX时只查看语法贴士）

🤏 依 🚫 一 ⛳️ 是 🔗🔒 悟吾 🤔😡 易或 🧲 全
✨ 以 💬 As 😅 引 🚫 五 🏳‍🌈 移 🧲 全  💭 Your Comment\n multiline..  💭\n doc blocks.. set fname{} %{doc} ⛳️
🚫 u ✉ imp 👉 F 👈 FnHo 🚫 ON,NO,OK^ 😡 Or ♻️ cue,[{$_}]   # Yemp Vonhon local-cute 😏“宜往宏”块和流控.  name_: [ON orNO] bool, or [ON 🤔✅], f(.., Xopt.n 开启尾部bool)
🆎 am 💌 isMounter 🚥 are 🍡 Args 🚩 test IN📦,OK📦 ItAteDeeper  It🔏 ItNeverDup  💭 value types a: are_(["", 0]) unchanged..
🔍 fetch 🏠 open 📻 exec 📠 sqli 🚧 Throwables ▶️ Contexts ⏩ Events 🔂 Chan 👑 F.next 🔁 Loop 🚮 Deprecated
```

1. 认假为空、认空为错(NO,NaN,[]{}.N=0)，空和async必须有_ 后缀，私有成员和可监听$侯数会有 _前缀，未知变量只有首字大写的查全局表。支持 `[{x}],x("可变引用即回调")` 局部值和字典，Xopt空参为默认推理为旗标的 [ON orNO]，支持 {-同名值 -免写}。 CDEF/ErrDef+Loadumper ，用于支持Java串读串写（序列化Visitor）和 import cffi 的一张基本类型的表格，[参考b4=i32, c4=u32,d4=f32等](#串读串写支持)
1. 【命名】最简字面即类型、通配命名即类型 `row req implied (- Str ""; - i* 0  - s* "str" - t* (0.) - a* [b4 are])`，字面比如 `[are((""), "init.[0]")  am(([0,""]), )]` 对应可变 `[Str are_]` 等
  -  F体系包括 F.next(super) F.next{KW,XO}，F.u3 首参命名空间化和 `f ((3)>[A B C], {u})` 的表格列参数也是好特性。As{for{}} 中心填OK^就相当于continue，而Or{cue}取代了SEH错误处理
1. 【侯数】惰性迭代器 `- obj { yielder_([{w_}],arg) { (w_(1)==arg) Or "received not {arg}"; OK Loop() Or end==1 OK } }` 为 [b4 b4 iter]，二参数表示next时可传入，?.f 是 await f
1. 【autofix】和非JSON字面  路径 `./x.path.open()  ./dir/x.txt = (U._txt.dir.x), //./r/es = (Uh.r.es)` 颜色 `[#000  #66ccff #(0,0,0) ]` 网络和$_UI回调 `//>http1/goog.le/{path}/?/{{k:v}} = 不加密http(反引号调用)` 免点号 `04A = (u.at_(3).A); a b -c d "" = (a.b.c(d,""))`，除了 十一Xノ infix_写函数名上，还有 isnt and or

【依一是悟易实，以时移五忆实】，记住这个会时移的五色神犬！这就行了，比Python还简单 ——只要你会编程的话 😏

如果main所用函数类型均完整，将可以编译到 Haxe fatjar (✨: FNF game)（唯一支持那个有 record,sealed,var 和 `List<>()` 的Java版本），甚至 Zig (WIP)。基于libc生态时均可选 [cosmocc](https://github.com/jart/cosmopolitan)/emcc 两种 zipapp

语法被设计为有可能编译到 GDScript、Amber、 Vlang .so (✨: Vinix OS, vsql, ved)、Zig nostd，它们被视为将来的支持目标。（native 暂时依赖 Glib or uthash/JSMN，未来可配合类似QObject/slot的 zero-GC 变量树管理）

它提供许多语法的自动修正（以降低摩擦），但运行时有较Py更严格、较Kt更宽松的编辑体验。其变量树具有 `可空性_, async动作_, if_内存宽度不定的row, row默认冻结+不含空` 的明确特质，并在JSON中严格执行。

不要将cyy理解为无聊的【解释型语言】或 “网络和调试的DSL”！通过 `pm -S --cyy many-libs` ，它可以开箱即用的辅助各类实践。

【可探索解释性】是一等公民，round-trip 和可二开是一等公民。类似Tab补齐（功能可探索）随从可见，TL;DR的文档、开箱配置（心路可解释）被视为成熟框架的标志。再比如你写“前端”，自然会考虑到【调用远端sql和本地bash】甚至【简易PWA打包】，向二开者展示 “mods.sortBy(着重性)” 的深刻理解力。

可配置，不折腾。这就是 round-trip 的框架。

对于cyy，编程语言更像一种【类型化的协议与CLI用户界面】，而不是语法+标准库+工具链。 cyy的类型检查通过将函数编译为JS实现缓存和infer，for跑一次if跑两边，那样简单。

**【主厨特调】 cyy 为你附带了一组SDK工具、运行环境，以及开箱即用的 demo src/asciicast。这些录屏使你可以免下载查到 `$ vfox available` 有哪些选项，NVim怎么用cyy，这样多少有点用户中心感。**

这包括 `pty实现 PyAutoGUI(uvx) luarocks chrome-remote-interface(pnpm) tailscale; mise vfox distrobox.it rr-www try dua-cli(项目耗时和大小)`

以及开箱即用的调试工具 `Appium+Inspector Bruno frida,gum,yad,gdb2win,gdb2jq   {l,s,bpf}trace,rr,asciinema,reptyr(Arch/DEB)`，作为SDK来说它们体积小巧。单个 > 10MB的包可以随时在vfox的同级菜单中安装！

> `#> env eval_in=exec_yes` 被用于展示vfox一键界面，支持()>显示简单进度条和不断按y。 pty 包括 ms_term+git_bash+(WinQ下拉终端,CtrlR搜历史,SDK补齐)，杂项兼容包括 `mise use -g fzf bat fx micro gh yazi uv mitmproxy aqua:ouch-org/ouch # pm -Sm fzf..`

> 几乎所有开发者都应该把 `apt;rpm;yay` 这些东西拉入黑名单，用mise和winget这些一键包。只有下的东西跑到 App Menu 或etc/var目录时，才解禁系统包，否则就等“开箱即用”时卸载重装吧。 [C开发可使用pkconf](#cyy分发)


> 注：row=结构体||结构的容器类型，因为有inline式链表(Linux)、AoS/SoA向量和列数据优化(Jonathan Blow)，**这个Excel式的概念并非 T[] 的泛型嵌套而已**。有一组 ID2,lit,dup,T,DOT 的万能属性，比如T可指代$0.节点名和$_.文件扩展名，DOT为节点KV和反射常量等。

## 限制与优势

不像绝大部分REPL，cyy以单文件内CtrlX+热重载+TUI只读面板，实现命令行交互。REPL总是attach到哪怕是空白C程序中运行，作为免安装 Web-pkgs-playground/eval.js 的补充。

暂时强制单语言兼容。比如 `%req { py first json pandas:pd; py os re; cyy ./wtf.cyy  //github/wtf/exportMe.cyy; vars json ./dates.json }`，py风格的导入导出！ imp.包名 自动安装导入，可插入 %py{} 块容错回编译。作者尽量使用 Babel/tstl 等既有工具拓展cyy可调用的软件生态数，并利用TDOP解析和SourceMap等技术【转译】复用目标的LSP支持。

REPL宿主称为 soPID，它提供了一组解释器状态+ldd依赖和同目录下res资源。TUI默认列出或创建 hookable_cat 🐱

您随时在其他窗口rePTYr连接这个能够访问frida+cffi的 PID (or GDB)，编辑和重放其 ldd/exec/open/mitmproxy，甚至屏上 listSort+texts 行为(Squeaky Entr, WIP)。

1. Squeaky Paw ：面向断点编程体系+网文链接渲染(BOP-MOP)。在报错、未定义、任意库函数开头时REPL试错、验收后自动替换函数体，Markdown笔记即文档支持
1. Squeaky Entr ：解析，存档重放进程和数据流环境依赖，如 open/exec/curl(F12 cdp) 类接口，实现外部服务交互转文本+热重载，支持onCurl/onResp脚本
1. Squeaky Choir ：基于最底线键鼠-窗口xywh的录制重放测试工具，也用于存档GTK4/Qt6/X窗口进程为时间旅行截屏（Winforks），快速触发按钮编辑框等
1. Squeaky Beg  ：像DarkReader那样用转译器查看ChatUI。单串多文件（面向剪贴板编程）、【测试即跑分】多平台转译对比工具。 BEgAST+数据流的块内符号迁移重构器、typer和Quine_使用的【Mock运行计算】流控框架


HiBLIP 允许在容易出错的函数开头加 `9()` 载入REPL（弹出调试时有 `\(^_^)> traceback_with_vars` 注释和各参数类型），这样，你就可以不断热编辑它，直到函数跑通并功能正确，再删掉 "9()" 记号，自动写入fun。其他位置写9()可打断点

`req u.myFun(1, "2")` 可（在9断点中）生成新的函数，或CtrlO定位到既有函数。直到输入 `OK(res)` 前你都实时的扮演myFun，你通过ABC..指代其参数，删掉弹出的增量栈 `\(^_^)>` 并CtrlX来时移读档，删掉9()CtrlX写入库函数。 "9" 正在提示你：F12中可以用F8和F10“播放”与步进程序！

尾部点号的行按CtrlX将显示值可见的补齐（外挂式补齐），选出 `- A .var1.evt.key` 再选B，然后执行 B.fn(A) 即autofix展开，不留AB。深变量可按CtrlX展开，`>> x` 保留上个值为x。 补齐UI同样可查重构/ctags/模块树


【单文件交互记录】如同 `"1+1 \n ()> 2\n"`，请在单元格的任何一行或()>输出处按CtrlX重复执行。在执行 `#> try do wtf` 后，你声明的 row/fun/req 均会写入 `~/Proj/cyy/try/$desc/{a.ho,a.f5}.cyy`，实现 HiBLIP 编辑。有类似 `chrome.devtools.panels.create` 的crx浏览器 PyOdide/cyy/Coffee/Fengari.lua 体验，还支持下方直输（无高亮，可即时求值，`SDK.RuntimeModel.ExecutionContext.evaluate` hook）

cyy引擎的F12面板，仅支持控制台、网络、本地存储、搜索、Perf 五个面板，初次打开将建议你调整顶栏/抽屉布局

HiBLIP 增量解析和逐函数翻译，支持字面常量池和单函数热编辑，极大的减少gdb目标重启。你甚至可以用 `FnHo("watch & print")?.{w@ u}; FnHo(()> liveJson1)?.{}` 监听即时编辑试错、硬编码调用 main argparse，等难题解决后再清理对BLIP系输入的依赖。

FnHo的【运行时回编辑】足以支持把 `[-button {tap()>alert("")}  p'designMode']` 热写回源码文件，就像F12的css编辑，甚至将支持 lilgui/tweakpane。 `nanoQuote(2)> p(A,B)` 【安全字串注入】可以强制单引号省略之前的p或print

cyy 单文件笔记本配合 F12 Workspace 时，可随CSS一同热编辑。REPL不支持FnHo，但仍可热编辑row函数，也包括箭头函数提升和this函数转发，如 `let i=FnHo(0); setInterval(()=> alert(i.v++), 1000)` 会被命名为 `()=>cyy.$xxx(i)` 对应 bind(null)。动作和数值均可替换。栈分配和闭包是啥？咱不知道！


【容器化交互记录】每个新的 OK 语句均可以删除来回档 gdb checkpoint，选中窗口时会问是否重启它以支持 [criu xpra bwrap] 的 Squeaky Loop（OK语句将产生 `cyy/png/截图`，可供拖入时回档，仅限本会话CtrlL之前用） ，还可设置退出时一键打开 speedscope.app


【声明处调试】`fun {f(x) |> {}}` 可log参数返回耗时，赋值 `- x 1+1 |> gl [x==2]` 可将符合条件的值保存为全局变量 S.x（输入 vars.spy.gl 防混淆打开），又如 `f(x,y) |> [A==B] {}`。函数参数 |> 后包含它的局部返回前log出表格。 '\>' 临时禁用断点。“数据流” 和TODO一样可以留作全局grep标记。

仅限 `imp.$DBG=d` 时执行（exit时可能会拉开REPL），通过 hosh x.cyy 则无效。`f(A,1+2 x.f1(n,o)|>, C)` 测试输出 "|f(B=1+2 (x.f1(1, S.o)))|> xVal"，你同样能复制 |>的打印，一键跳转到源码(zws.im) 和 vars.spy._1 来检查(S.o)。 birdseye 的类Scratch循环调试在编译到py时可用 f(x) |>> 触发，此外 are.movIns{ u(i, hint) } 可用于渲染指针动画

【动态模式匹配】类似于 `if { for(xs >> $x){} }` 即 if-let，fun内使用 `clear(0, $_a, $_b); swap($_a, $_b)` 两个FnHo可交换其值（若未初始化读取/未限定类型则报错）， fn([{..}]) row{} 中 `div(p($_e1, "Hi"), button($b1))` 可多方法，共同绑定私有变量


Squeaky Entr 特性未来可配合 Frida + Qiling / QEMU-Nyx，其读档存档相比gdb更不可破坏，有内核恢复与交叉调试（采血重放）能力。

TUI面板可列举变量类型/自定义类型上静态计数_行数/IO API调用计数/DB.SK_变量管理，或为不同窗口标题下的CtrlX分配既存或新宿主（窗口选择或 Ctrl+C bash jobs 里一行），您随时可CtrlL清空状态。默认勾选【报错到剪贴板】

TUI能够按sizeof和pad浪费过滤lib.so，以及查看xref和dump整个ldd依赖的cffi头（依赖 DEBUGINFOD_URLS）

它还包括【可观测性】工具包

Perf 支持 pid调试重启，火焰图采样、任务等待链(not lua)。通过 sgrud; clinic; findstr/lua-perf; py filprofiler 实现 (speedscope.app)

Perf 也包含静态的 npx source-map-explorer [online viewer](https://sokra.github.io/source-map-visualization/) （ https://blog.phronemophobic.com/treemap/treemap-demo.html 风格的指示器+Sugar High 高亮）用于可视化它们的ctags行数L/调用计数C/耗时Perf（LPC加权查询）。 scc/tokei alike

### 覆盖到细节的设计

工具链上参考了 <https://wy-lang.org/> 和 <https://www.emojicode.org/> ，以及Colab+文档页文学编程

`$ yay -S kate sublime-text-4 neovim micro` —— IPython，但是支持所有你喜欢的编辑器！

注：CtrlX基于(选区和章节光标->textarea)的绑定，完美支持 wl/xclip primary + win10，而你的编辑器能按Shift,End+上下右即可。gdb回档基于()>序号，这些甚至能无缝兼容 `eval_in=ruby`（单行无'='即包裹print, [自定义so_site](#cyy-repls)）

选区以 ()> 或 ``` 切分，其头部可能是 %%write 等魔法段落或 #line 0 "xx.py" 代码段落用于支持【文学编程】。#line 0 "xx.py#:~:Title" 可以将代码注入到模板 #> Title 或 //> Title 的位置，具体行号在执行时更新（每次启动清空#line文件）

【快速反查】如果在fun/row任意形参名尾加'!'并执行，错误面板将显示函数的xref并提取出caller的实参。

> 综上所述，cyy 被视为一种与运行中的 JVM/JS/Lua.. 等调试接口人机交互的文本界面，以及：声明式DSL与框架编程的【最小工具】。这就像Python是面向numpy或表格等内容那样。

代码的优化和替代品的开发，是一个【人机接口问题】和【进程接口问题】。思想试错和既有生态不可被跳过，这就是cyy的 Roadmap —— 我们将通过设立[参考软件](#Hall-of-fame)作为自身特性集的地标。

对了！你可以在 <https://justine.lol/cosmopolitan/functions.html> 看到各种PL的运行时所依赖的系统调用。这是一个polyfill多平台系统调用号的精干的静态链接libc，且libcosmo_plugin可通过干净的mmap加载系统链接库。

它基于 ./polygot.com 来下载，有可编辑的 ZipOS(/zip over /usr/share)，有网络+py命令行的移植，自带BSD风格list/kv。 apelink(syscall emu), blink(x64linux=universal) 以比图片还小的体积完成了更紧凑的fatjar式体验，技术力直追Bellard。

【Squeaky 是什么？】  Squeak.org 和Emacs很像，都是一种独立于操作系统的编程概念。它是真正“面向除错编程”的JVM，相比于各色“脚本语言”和 H5 Vite/Docker 更有特色。

其实，Alan Kay 的OOP更接近函数式世界，而不是 C++。 Blockly.com 作为Scratch3的组件库，能够让你重新理解源码和AST文档树，而LISP不一定叫 ((L(is)p))，它们总是有JSON和NET那样的名字。Tk和Plan9的可观测性，也启发了H5和Docker，甚至发生过“RMS和Tcl的战争”！

Squeaky fun 也有过3版设计：

- 比如 `req.. req clear($0=e, "lit"=s)` 语法，把这之间几行REPL替换、写入为 fun clear(e,s)
- 空函数加 9() 就能在断点中试错
- 前置req就能传参调用任何404的函数，然后交互式的学习它。为什么不在事件中用9()暂停，再调用进它的req函数呢？像扮演gcc那样！
- 对了， 9() 9() 的嵌套调试之上，每次有至多3行 `\(^_^)> traceback_with_vars`，是可CtrlX展开的。函数热重构就像Haskell的>>=，Bash的$_一样，是本项目的Logo来源。

cyy vs hito：

- 都支持HOLP和FuOP范式，在响应式与API跨端上表现一致，但roadmap不同。
  - hito可以与 Haxe(--jvm,php mostly) 之外的非脚本目标互相转译，而且有h2js等运行时依赖，会区分 Row_/Ro/RoLit 深度冻结而不只是 Any/NO/ItNeverDup
  - 所以，cyy更原生的支持 ps1,lua, GDScript, Amber.sh+AHK，甚至可测试 scratch3-extension->Leopard热重载 <Shadertoy.com> 小游戏流 (npx tstl ts2gd coffee-ahk js2coffee)
- cyy仍然保留了点号调用和ABAB函数签名，hito则完全转向bash-kebab风格与AABB函数签名
- hito语法文化中立、不可以自由压行，纪律性更适合大型仓库维护、系统编程，但在快速调试和编程教育上，与cyy差距较大
  - 可变退化不限于集合类：返回不写明 Box_(1)._ 都退化为Box，._只可读取1次/Δ lateinit。令牌资源仅 inc-never 可移动 `Builders_,cue,bufChan`，never.target可让filtmap原地（或先dup），FnHop?.{}可全局共享


## FuOP的三个组件

- if-Or惯用法和maath DSL

Or可用于字典替换和类型when-is，对于 \\("_"){tryOrNul} \\{OK("ret")} \\"普通异常Panic" 三条可以斜杠简写，支持【假空空错】和 isnt and Or ==, (isnt: A\=B,!A)

inc(赋值) OK `optOrNul() if 1==1` 和 `(1==1)?. { op() }` 也非常有效，.if{} 建议只用于多路判定（字面量用.Or({})），for建议只用于包含解构命名/if/OK^，if中有 (1==1)? "tag"; =ENUM=; for(matchCase) 和 `boy.if u.mother >> her, if {(her.isMale)? "False."}` 五种惯用法

And并列兼乘法 1==1 and '${1 x y}' 为单位和 P.X(Q) 优化，同理有 #(1,2,3).matmul #66ccff; 2.pow@[1 0 1] (0b101 comptime)，数值只支持0,0x,#前缀,.前缀,下划线和e[+-]n

- Java类型和 BELT(3.XNAA) 作用域： row req implied, (litDOTT)?.!!, [JSON]: row new{ate}, [bytes]: are_((0xb1),4)

- ItAteDeeper读写器和Quine_生成

nanoQAFT: Quote/Arg/File/T$2, vars.T.可补齐局部静态表，imp.Typ.DOT是元类

## BOP交互编程模块

## MOP文学编程模块

- 任何位置都可以 req뮤 (obj.)写入函数签名()，其中以uABC..E可引用当前参数，执行到即弹出REEPL
- Sketch-n-Sketch：列表状语法或多行折叠/展开/删注释，添加/清空Slider和参数化
- 补齐出带示例的调用，并且根据光标下参数的类型补齐变量和属性；甚至查询lib提供的test段/demo应用

## HOLP面向人类列表协议

## HOLP的三个组件


Choir 录制：

录制手势 ctrlC文本 Shift单击 Ctrl单击设置文色断言 快捷键 ，同标题归一个彩圈。按CSC时若鼠标按下，则每次重新聚焦均会重放。已读档的窗口按CSC会重放。

可以全局搜索UI文本，可查看perf排序的已调用nm+cpp符号并生成js(lastRet,9())替换到.f5文件 （frida""正常模式可用）

Choir.cyy 需要热重载 test 函数充当REPL，并使用 kbd "📸" 开头的函数从录屏截取步骤片段。（ tap #(x,y)  kbd "CAS-Key" kbd "📝 ctrlc" "📸 Win Title" ）

右单击彩圈，将从截图的占位窗口读档恢复进程与窗口xywh的位置，hook代码即地注入。中键可快速关闭窗口清楚rm存档


## 中文读码支持

只提示正确语法的保留字：`val class mod impl as await try do when while inline enum not you 不 break`，比如 OK^ crossinline 函数式流控搭配，就需要科普文档

为未来框架的保留字：`一の你的是且或而去了`，介词：`就要以外对于 有为如咋啥`。命名中包含介词，视为有点号分词，如 `b1以red外绿 == b1.以(red).外绿; am(1就2); (1==1)了`。

啥,去,了 用例 `user啥Str (弹出补齐); 海龟?.{要0度;30度;1步} ; $("div")有p("hi")了 # void断言(has p ok)`

前缀翻译规范: astogetset省略 on对 forBy以 fromOf要 with有 to到 is为 hasNext如有右 beforeAfter以左以右 If如 (async f, f!)  ` MaxAt0 大在先 /re/.exec 有(/re/) fetch! 得! [Str are] [词]`

那些部分场景下可执行，但应当换掉的虚词（从,在,把,被,对,比），给出一些用例：

```
prefer 于(>> a.[0:N]).复制到(b) but 从a 移到 b；你 于 #(0,0) 要30度 but 海龟在(0,0)设置顺时针30度

有(1~3 >> $x){} but 对(x 在 [1 2 3])；b1就{}, b1对"点击"{} but btn1被点击{}；a.比(b).if{=大在先= ..} but (a比b)大； (s1就"b")结尾?.{u} but ("ab"以"b"结尾)?.{"ab"}

以 物件() 悟 It可闭 :{一 i 移 0}
是 {
  four() ~> 0 { 引({i: 1}); inc({i()> you+3}); OK(i) }
}

依 `It点` 悟 It可阅
是 {
  走() {
    全(pts >> $p) p.Or({ As(u It点)> x.if {=0= "零"; =1= "百分百"; "见某词" "替换"} }) Or ""
    As { for(inc.[0:n(a)/2] >> $i) { a.[i] >> b.[i];  if { (a.[i]==0)? OK } } }
    - ret trust_me([3], (i) { Loop(i-1) Or i==0 OK "done"; w@ `--> ${i}` })
  }
  为完整()> 取 { (1==1)? ON; (并非 pts)? NO; (n(pts) 并非 1)? NO  } Or NO
}

見 もも() ● It閉 :{一 i 移 0}
思 { }

知 `It点` ● It見詞 # input ●, ".." 或写set运行即autofix。使用 zws.im +文尾字符串池保留翻译前的名字
思 {
  出す() { 全(pts >> $p) ... }
  確か()> 取 { (1==1)? ON } Or NO
}

웃 Wtf() ● ItCloses :{- i 새 0} 
뜻 { }

뮤 `ItPoint` ● ItInStr #> Claude Cutie!
뜻 {
  감() { 윗(pts >> $p) ... }
  온전()> 윳 { (1==1)? ON } Or NO
}

суть Wtf() есть ItCloses :{- i шаг 0}
я { }

для `ItPoint` есть ItInStr
я {
  goes() { все(pts >> $p) ... }
  chk()> если { (1==1)? ON } Or NO
}

#> 以下是英文版、颜文字。中日德韩俄5个语言互相对应

# Yemp Vonhon local-cute, u imp F.u,FnHo ON NO Or OK^ cue
# am_ inc are_ made_ at_?.[] Loop isMounter As: I[msft]XO_十一Xノ
# 🔒💭🚩✉🆎🚥 w@ 9() #@1 @# isnt并非 inc引 u你
# [fun Xopt row DOT_] [.if u>> for] [As,trust_me:Loop-Or-OK] [req newF.n, >> newval, (>>expArg)]

row Wtf() set ItCloses :{- i new 0}
fun{}

req `ItPoint` set ItInStr
fun {
  go(){ for(pts>>$p)...}
  safe()> if{(1==1)? ON} Or NO 
}

✨ Wtf() 🔗 It🚪 :{ - i 🏳‍🌈 0 }  # useful ✨ on doc-site!
⛳️ { }

🤏 `It📍` 🔗 It🔤
⛳️ {
  🏃() { 🧲(pts >> $p) ... }
  ✅()> 🤔 { (1==1)? ON } 😡 NO
}

werk Wtf hat ItCloses :{- i new 0}
kern {}

fuer `ItPoint` hat ItInStr
kern {
  go(){ je(pts>>$p)...}
  safe()> if{(1==1)? ON} Or NO 
}
```


以上，

实虚词是和动名词一样强大的语法概括，我们可以从中看出中文编程（形式语言设计）的弊病。 许多虚词可互相替换(且/和, 把/了/过) 并顺手破坏掉语序的一致性，而 会/过/再/.. 这些虚词，无法与几乎所有硬件和场景相容。《十八个文言虚词》里“冯诺曼世界”能用的不到一半

在错误的方向上停止，就是前进！

- “简单是可靠代码的先决条件，而非可靠的牺牲品——Dijkstra, 发明递归的人”
- “业界有两个设计软件的方法，一种简单大方，明确没有堆砌缺陷，一种繁琐抽象，连缺陷都难以写明确——Hoare, 快速排序之父”

有些人认为NLP的弊病仅仅在分词、代词模糊，比如 “管理[和服]务” “猫坐在毛毯上，它很舒服” “石头碰上鸡蛋，它碎了” 里语序不能明确出什么

“那位黑的卡车司机闯了红灯”，仅仅是【那】脱离语境就能翻译出黑人、黑车、司机没开车、红灯只是红灯四种不同语意。再想想AppleScript的恐怖之处，那些该死的 to, of the, end if.. 把命名写长，就能让任何人看懂代码吗？

哪怕仅仅兼容代词 `你我他它ta..` 也会让语法错误变得无法掌控，而如果你只取it,“它”，又会让下游可以滥用“你我他”等UB一般的slot命名

哪怕我们只用LLM，消解掉同义的、无语意字符的问题，很快也会撞上工程规范和可复现性（Dijkstra预言/EWD666+1）的叹息之墙。语境大于语法，选型大于实现。

**我已经厌倦了【保留字军备竞赛】，这是在为尚未出现的语境假扮上帝，我也不想随便添加mixfix等乱改语言规范的宏魔法，并以为自己会的hack比别人多。**

cyy的块语法和UFCS已经够“乱”了，变成Perl或 Ruby do 那样？只会成就Smalltalk的第二次失败。

我选择允许 \`keyword\` 成为slot的名字，并且只保留人类可记的十几字顺口溜。在我认为的【所有应用场景】下，它们像CSS一样有效。但我不是仓颉…… 幸运的是，文字本来就是像 “以为” “不如” “非但”，无意义便是意义。

请分级的使用它们，就可以取得权衡：比如 e0.append(e) 是 `e0有([e,]) 或 e0以下e`，前者为声明式，后者为过程式的时序（区分append/prepend之类）

请聪明的使用十几个【保留介词】和operator，你完全可以让cyy实现中文母语感，哪怕字符集非常狭窄…… 下一个语法项目，我不会嵌入任何“词”了。

请注意：如果在autofix里加入jieba分词，而不是源码级、破坏点号调用，会好很多。【母语编程】不能让垃圾API变成声明式的数据流，AIGC也不能让天真的临时需求变成有竞争力的App。

作为一次首创性的实验，中文是种信息熵极高的素材，但好孬全凭API的作者。 你认为没有“首创性”？那就是你对。

相比于清晰、一致、初等如同ZXCV剪贴板但功能强大的操作（窄接口），无论是太“自由”或太“智能”的界面，反而让用户困惑，不知道如何可靠地解决需求。

法律语言不像“喜怒哀惧”那样严谨，因为语言只是【对事实和经验的侧写】，是一种了解实情的介质。通过交互、比喻、灵感或图形掌握了事情本身，才是软件或司法的【第一性原理】。

## 以实例学语法

语法范例：

```
row req implied ( #> 以 依 类型会意()
  - lang_features "repl blip ckpt.png"
)
row FilenameCyy :{
  - aVar [1 2]
  - bVar :{-k "v"}
  - CONST_1 "all-caps"
}
# 允许文件级语句。row等类型值只能先定义，后调用；允许字面和闭包热更新，OK语句即检查点

- N 1_000 # 大写默认const
- n 250 # FileCyy.class 里的 final int。非交互文件视为包在 fn() new row{} 里必须写成 n()- 250

- s1 (N==1)?. {"1st"} 或 {"nth"} # ?. 的函数名为As() 或 走()

- s2 (N).if {
  (u<10)? "💥" # 读作 u<10 if so “的话”
  (u>10)?
    w@ "💢"
    "💥"
  # null表达式即 else -> （一般写双路if）
} Or (u==10)?. { "✅" } Or("panic")

- s3 n.if u >> { u+1; u*2 if ON; u.Str } # "502", when(val u=)

# - n 250 # 禁止变量名遮盖
new acc are_((0)) # .([int are_])!!，搭配某种动作。可变集合均含有后缀_ （但类名不可以_结尾）

- cnt FnHo(0)
vars.memo({cnt}) # 侯数+json状态管理，不怕 unload
w@ cnt; inc({cnt(n)> n+1})

cnt?.((n,to)> n+1)()  # 更通用的GSetJQ写法。to可能为Event
cnt.Or({1: 'item', '': 'items'})  # Or 可隐式变量派生 when+if，其他许多情况需要套一层 cnt?.(u=>u.Or())

OK test {
  acc.isEmpty
  inc(acc, 42) if ON
  OK() if NO #> 两种if短写
  inc(acc, 2 * acc.[-1])
  # 结构变更，配合for时容易报错
  inc(acc, 0, [233])
  inc(acc, 0, NO)==233
  acc.[0:-1].Str == "[42 84]"
  42 >> acc.[0]

  # inc({n: n+1}) TypeErr

  new ki am_(["", 0])
  ki("")==NO
  ki("hi", 233)
  ki("hi") { u*2 }
  ki("hi", (u, to) { u+1 }) (NO) # event handler, GSetJQ style
  ki("hi")==467
}
test {
  - a [1 2 3]
  - a1 are_((0)).made_ {
    inc(u, 1)
    if {
      (a.1 == 2)? inc(u, 2)
    }
    inc(u, 3)
  }

  - numbers  [1, 6, 3, 8, 2]
  - doubled_evens numbers?.{2 u if u > 5}
  (w@ doubled_evens) # mapNotNull 输出: [12, 16]
  - went (isnt tired)?. { go_to_party }
  went?. {
    take_photo
  } \\ "Not going!!"
  - rem  16px
  - s    1000ms
  w@ .3s 
}

test {
  w@ "".join {
    say(`Hi`, `there`).{-end '' -sep ', '}
    say`StringBuilder`
  }
  inc(0, oo(a))?. { say`{a.[idx]}` } ## ∞ == a.at_(), for(let i=0,N=∞; i!=N; i++){}

  for(inc.[0:a.at_()] >> $i) a.[i] #  oo(a) = len(a)-1

  "Hi 123"?.(re`(\d+)`) { - n readAlike(0, A) ; 2n } #> 支持coffeescript风格正则块，autofix re'wtf' 为 re`wtf`
}


row Wtf() set ItCloses {
  set close # final override funs
  close()> w@ "Good 4 u"
}
var row Wtf {} # same-name object
- or0 "".(["" orNO])
\\("_") {
  [Wtf() Wtf()]?.{ u.cue } # res dispose (As {} scope). \\("_")> SEH try-throw now offically deprecated
  or0 Or{ OK } # retearly

  for ([Args(1~100)]) {
    say`--> {u}`  # list(*range) forEach
    OK^() # for:continue
    OK # for:break, uses IIFE return or do{}while(0);
  }
}

inc(1, 100)?.{ (remLesser(2)==1)? {u} Or{NO} }?.((n) {
  say`--> {n}`  # lazy flow
})
```

基本上，你可以相信cyy和手写性能一样，不用操心开没开 -O3 优化。 Boring~ ¯\_(ツ)_/¯¯

`never(never.target=[0])?.(u=>u+1)` 可原地映射过滤

类型与复用和kt略有不同。cyy更接近原型链 + LEGB作用域。`G=函数内req + u参数上的属性展开`，文件是单函数

函数值如 `(i 0, s "")>ON 类型是 [b4 Str [ON orNO] F.n2]`，F.u2 将参数0视为u，它们在Kt的表现不同，但cyy是调用侧决定展开第几个参数。


```
row XY (
  - x new 0
  - y_ new (x)
)
# y_ 代表有默认值。类型的特例：0可以指代b4整数，"",[],{} 分别指代Str，Any数组，["" any am] 的Dict

new ki am_([0,""]); new az are_((""), "init.[0]")

row Pair$2(- first A - second B) # 表格列参数，TypeVar()时同理
fun {
  set Str #, .. override funs, set? for open fun in Im/Imp class
  our swap(nth 2) # private funs, our? for internal, ?!protected
  #^ 已有_前缀的不得用our。 private<3 个lint会建议用下划线


  swap()> Pair(second, first)
  swap(_bad ON)> Pair.$[B A](...) # TODO() 的缩写
  Str() { OK(" <3 ") }
}
# autofix 连续单独 fun -> fun {} + 允许kt风格参数

row Rect(- u new XY; - len XY)
fun {
  our? u(set) # internal setter
}

row Rect set ItPoint { DeriveTo_(u, ItPoint) } # 自动实现宏

req `ItPoint` set ItInStr, ItCanEq 💭
  Go式结构化类型，编译为 LOOKUP.findVirtual; 有 hashCode()
fun { x()> 0  y()> 0 }

# 如果你不实现就 new p_ ItPoint; inc({p_: Rect(0,0) })
# 会提示 T.404: 以 Rect 悟 ItPoint {}

OK test { # OK (语句)，在文件尾收集到main
  - p XY(0,0).Rect(XY(1,1))
  w@ Pair(p.x, w@ p.len.y).swap
  inc({p: {x: 233}}) # autofix 允许不输入:,符号

  #> w@ 是可测试打印的单目运算符，dd= 的别名。cyy赋值不用等号。 （w@ 之间不可空格。autofmt可修正）
  (p.area==1) \\ "fail msg"
  sum(1)==1
  sum(123, 123).{prec: 10} == 240
}

fun {
  area(u Rect)> u.len?. { 1 x y } # u.len.As(u => (u.x*u.y)) # .len?.((u) {1x y})， ?.{} 是唯二的箭头函数语法
  sum(A 0, B (0)) Xopt.:{-prec 100 -dbg_} {
    dbg_?. { w@ "Flag enabled" }
    OK(1 prec Math.floor((A+B)/prec))
  }
  # F.nextKW={prec:1}; sum(0,0) 或 sum?.(Args([0], {prec:1})) 以调用
}
```

有些更加高级的抽象，比如强类型union

```
# AbstractBox 自动映射，类似 ItBox 但允许存在默认构造模板，但它，除了序列化和验证啥都访问不了

row ImpBox(- tag "") new { Ate_(vars.T); /^_/.test(tag).Or() }  # 生成 ItAteDeeper 构造器，HOLP里只有带了串读串写接口，才称为【构造器】
fun {
  open()
  size()> 0
}
# 如果想加构造逻辑，new{} 换成扩展 fun { made_(pargs)> F.next(args); ... } 就会隐藏默认构造模板，但不能多加，且只能修改args，允许内嵌As{}语句。

row ImKitty set ImpBox("_cat") # 继续 non-final class，必须加Im前缀
fun {
  set open, size # autofix defs.
  set open {ABC arg-order test, test(u)} %{ My prompt and quickdoc. }
  born(u, age 0) new row { w@ age }
}

Tom() row set ImKitty() { # ImKitty类型键名必须为 .im_XX，但Tom不需要。
  our [JvmStatic] myVal myGet
  #^ object{} with static myVal=Proxy, annotation @Jvm("static")
  # 泛型的声明处 in/out, T? 可空性问题：需要 our $0(in/out/where限定类名)
  # 顶/底/void/untyped 类型为 [Any orNO],ThreadDie,NO,!!

  - myVal 1+1
  - myBox FnHop((v)> v) { say`setter: = ${u}` }
  myGet()> "get"
  
  say`Hello` # 零参数 fun T=object{} 不需要额外val=T()，同时允许尾部语句块(SAM impl, ABCD args)
}
test {
  - tc Tom.born(18)
  inc({tc: {age: 23}})
  tc()
  - C [1 2].if { for([1 B]) B; for([A B]) A }

  - fmt ((2)> "#${A} item ${B}").([0 "" "" F.n2])!!
  fmt(233, "666")
  imp.([b4 Str [Any] F.n2] are)(233, "666")
}

req `IsBytecode`
fun {
  # static assert = /name|list/
  # ::IsBytecode.assert(""), imp.IsBytecode.DOT.assert
  # 详见惯用法 (litDOTT)?.!!
  DOT_assert(u "")> /ADD|MUL/.test(u)
}
req `IfExpr`
fun {
  N(n 0)
  Op(k IsBytecode, A+B IfExpr)
  #^ Rust enum，但不支持If含泛型。如果只是想简洁的生成SQL/JSON表的话，库可能提供 Quine_ 宏和 ItXXApp{} 接口完成

  show()> u.Or({
    As(u N)> "{n}",
    As(u Op)> k.id.if { =MUL= "{A.show} {B.show}" } Or("")
    #^ =MUL 可补齐， 可自动生成 when-is .(Any)!!
  })
}

fun IsBytecode {
  id()> u  # inline class
}
fun `IfExpr.N` {}

```

I[msft] 类名通配符的意义在此例中，有完整解释。此外，类名不可以_结尾，全下划线和kt一样禁用。函数和变量如 'sym!' 结尾视为 sym_

【表格列参数】 Row1~3 D1~D4  (F)n0~n5免前缀 实现了 kotlin.Pair 和vec3这些列表广播计算的功能。

## 妙妙词典节选

LUCID-MeowDict 是用于跨语言/跨平台开发的心智模型写法测试，兼“helloworld程序集”，类似TodoMVC但更重视全局性。

各种语言都有【LUCID五个实例】带人入门： LineEchoOddArgs, UserRingKV_Ratelim, CounterDownBoom, IconGrid_LiveGrep, du_-h_ForkJoin

- L: 考察argv数组（parse后或序号是odd即 " ".join 显示）、stdio流的读写（同条件显示），一般的forif列表处理和复用。
- U: 标准输入用户ID，若5s之内用过则输出提示，并输出最近3位用户，否则执行 `fastfetch -l $randLogo|lolcat` 并更新。考核computeIf{}等声明式的支持，考核 [].push shift 的队列与 `fastfetch --list-logos` 胶水便利性
- C: 考核基本Mainloop，比如 `id=timer(1s, num?.(n, to)=>n-1); .. timer(-3s, onBOOM);` rAF(dt=>{}) 插值
- I: 生产力模板。H5的声明式forif，考核响应式集合和FLIP动画、防抖
- D: 考核文件/文件夹递归处理的便利性，以及多线程AtomicInt规约的难度

- lambda tuple recursion query combinator json_python_Regex function_FnHo
- event_closure handler parser sockets hashToken
- deque async coro-utine Cookie Context echo Channel
- macro Interpreter compiler-VM union_insect


1. 函数式：栏目答 它剖 蕊括深 馈睿 控伴同 择深_徘深_正择 仿行_候数(Holes Observer)
1. 面向事件：一问特_括需饵 喊夺饵 爬撕饵-串读串写 授齐易-管道配对 海稀通证
1. IO密集型：待客 偶醒刻 叩如探-回调链表 窟启 控态事 挨口-逐行挨端口 迁篓
1. 通用执行层：码可揉 硬特匹蕊特 勘木排而-微迈细硬  有联_应塞类型
1. 多页面Web： 画架嵌入-iframe 疲架可撕-PJAX 矮架可撕-AJAX 落靠-locale(LC_ALL)
1. 特殊的App：串塞-Transaction 瀑息-Push 位可投-Vector 纯函数-可复现 分页 转圈重试 输入防抖 Sync-新客（新客户端）

确实如此，

- 算法的类型可以区分四个维度：`IO密集 交互密集 协处理器密集 文件管道密集` （IO密集本质上是pid/上文频繁yield后才能跑下去）
- 人机交互同样是四个维度：`触屏 横屏 无屏 无盘`，无屏往往看并发，无盘看成本。 这些软件都可理解为【截图录屏文件夹】
- 硬件配置以五个维度衡量：`单线 超线 缓存 存储 散热`，服务器最需要可持续散热

“儿童编程环境” 目标并不是简化，而是让人体验MC一样的创作欲。设计本就是人类的天性！

没错，就是像MC、F12、Excel那样，【图形】不是编程的简化，而是即时连接材料。 是创造模式飞行（birdseye TTD）、穿墙（inspect REPL, tweakpane）、指令字典（lib with demo, playpen/nb/nodes）、联机和灵感推荐！

是在编程语言中数一数二的【超导特性】，比如强类型，它既可以把dev当傻瓜来惩罚，也可以实现探索补齐和存储优化。 不是看 user. 前缀，是补齐出带示例的调用，并且根据光标下参数的类型补齐变量和属性！

git和区块链像【文件夹历史记录】，socket则像【文件夹监听-创建回应文件流】。

🐧 pthreads： 你们终于重新发明了pid和 cat|less 的背压暂停，还把kill和Promise.all一挂全挂当成独家特性来做了。

如果stdout支持对象，并且ForkJoin支持call()结果数组，这大概就是async的样子了。

一切皆文件？一切皆对象？ UNIX其实是说 “一切皆管道” + “一切可以Tab补齐和tree查看”，确实如此。但 seek/socks/ioctl/gpio 接口与文件夹思维不兼容。

一切皆文件，带来了一些隐形弊病，而UNIX编程环境的【四大恶贼】是：空指针+单双指针自动转换、安全str内插无语法、不支持=>函数、运行时不带包管理器（“软件失忆”问题）

y2k/c10k/nulls/safe_strs/ paired_ptrs (eg. OOB segfault, funarg problem, io_uring+closures..) / newdll_and_signal (ldso TLS,DBus,regedit..) / zipapp distro.

这些技术之“神”早被自己造的石头砸死了。

软件世界从最下层，90%的应用层都需求的语义上，就在犯错。bash 软件受欢迎是因为它除了缺乏 safe_strs，你能做就能封装好。 “两三行解决问题的能力” 现代在脚本的世界都逐渐稀缺。

三位一体的荒谬：消息边界or帧差反馈？状态管理？串读串写/对象绑定？从头无意识堆砌一直到应用层。 写个App都能感觉你的上游是梦游。

js世界一开始就拎清了 XPU、IO、UIUX、音画解码、“API优于IPC”（文件管道密集） 的场景，Arc Mutex 不用写样板，只需要 transfer:[me] 默认零共享

再比如yield是按bufsize算，还是 stdbuf -oL 还是 timeout $(curl)？ Channel，还是能硬编码时间节点的SHM，甚至丢几帧（debounce=latest）？ 一旦打开“流”、socket和画框，我们会考虑面向消息？面向帧差？

消息边界？ 丢帧如何反馈？ 而标准库不明白。 你的游戏锁帧60fps，因为能省3行定时器函数头。

它往往有几种【事件类型】都拎不清。犯了React的“事后diff”的epoll（甚至select），和js早有了的uring（1 rw 不需要挂起 1 pid）；拘泥于ImGui的网卡卡绘制，“流”的键鼠事件队列也只是变成Elm的funarg队列

网络各自重造轮子：WebSocket、QUIC、gRPC length-prefi；UI：deadline 密集型应用（16.6ms）+ callee优先级继承。Media时间戳： 直接把Wayland的【被遮挡时0Hz渲染】模型干的心态崩了

而时钟域之间必须放一个有损 channel，就像“UI按键检测”轮询与UX，AudioContext.currentTime（音画clk_id）。绝大多数语言：按量、按元素类型，单时钟、无优先级、不可drop

onBufferOverflow = DROP_OLDEST | DROP_LATEST | SUSPEND 的大一统遥不可及，甚至连curl调限时重试都要魔法。Erlang 封送转二进制后超过 64 字节却懂要走引用计数+SHM

bashrc和apprc都是把“点按钮”的命令队列当设置KV来用，目前是JSON世界之外唯一能说明白的状态管理方法——【启动状态只是命令重放的退出】。其他的，改个配置要重启。

1rw:1pid 的本质更要追溯到函数本身 —— 串读串写（序列化）如何0拷贝？递归head()一个流->返回对象本就是把栈copy到堆，再把堆sort回栈write(x)。Proxy掉 o.k,o.k= 语法很容易做到即用即seek，push分配时记在小本本。

序列化是一个内存碎片整理(file Arena)问题，不是4K分页malloc问题。内核抽象机很复杂，但CRIU和Boehm/AutoFDO（免源码GC/PGO）的功能都做不到。

Linux 5.x，长达数十年后node风格的带闭包IOCP才被融入libc生态圈。如果🐧是在刷yt主页，那它大概需要先拿前30个推荐视频的id，再逐个去GET标题作者封面呗？消息边界由gcc规定，你以为就没坑可造了？

当考虑【消息本身】最少包含的信息量时，你必须按T{}的【最小有意义使用】来构造T，而不是按无功无过的OOP。 对寄存器化类型（轻str）、重文本编辑器、侵入式双向链表、零开销inline、new引用和SHM，甚至是og:meta卡片和SoA/AoS多态坐标的Arena，都可以去对标。情况比你想象的要轻松，只要你比🐧会刷yt主页！

再比如，MC里描述一张截图的Swizzle表示是 E.xyzb'mat (blk,meta,anim,turn)，分为(整数对齐|动物浮点)两图层。高性能的本质是理解了问题的限度。Veloren的体素有最高的山、最精细的捏脸。性能不是在浪费设计精力，而是制造一种新玩法；【理解事情】不是为了接近最优瓶颈，而是在改变品类。

真是三位一体的荒谬。和状态管理与流同步的群魔乱舞，一样“本质优雅”，却有异曲同工之妙。 OOP该死的 (a,b..)> this.a=a 不就是这样无意识的序列化和设计API，把神兵雕刻成了人人厌弃的败笔吗？

Rust的SSA变量管理也一样繁琐。

但它的故事讲的通。rs借用和 QObject+slot free 模型是一个可用级

单定义单引用和滤镜图一样直观连接。它有字面类型拷贝和只读借用，显得不像Blender滤镜图拖放连线。

而标准线性类型系统，没那么美丽，连 mgr.f().f1() 都从JQ风格变成地狱。 “仿射类型”是什么鬼。 ☢️ 放射物？

filt.map调用链和“变量自带mutex/defer”是真能用，但尽管利用了“rc==1的连线”，它也不得不还了词法{}lifetime的NLL债，而且对子树分别共享至今Rc RefCell，很丑，最后人均Arena.u32

<details>

【控伴同】 就像 (A,B)=>(runtime_vars=>A+B)，一类栏目、一类回答。规则集中，每个“伙伴”都有相同接口(_=>...)

【候数】的精妙在于数据流，可以绑定json表单 user.age?.(n=>n+1) 派生新的候数，或只是添加.onchange

【括需饵/喊夺饵】 两个词强相关，缺一不可。Qt不使用(ev=>{}) 括需饵，一切都是 class SLOT 导致开发繁琐。数据多是 (用户生成并通证|授齐易|爬撕饵|ev=>{脚本})

【叩如探/待客/偶醒刻】是一组破解c10k问题的 “用户态Linux”，通过coro库轮询待客，检测出 await fetch() 等听取器的 “偶醒刻”，耳听多路，融动于静，重启【括需饵】的下一行响应结果。

【勘木排而】中，信达雅的点破了：compilers 会勘测查找type/slot的具体类型和重载，然后将forif与嵌套访问铺平为goto和指针加法，即排序

【微迈细硬】指出：VMs提供了GC对象树，字节码，以及模块名-值替换的链接、线程间postMessage的实现。 “细硬” 则强调了代码API - 运行ABI的隔绝（硬）与反汇编后的晦涩（细）。 Linux 的 vm.c 即mmap拥有VM的本质，而且有houdini这样arm/x86的兼容技术。

【硬特匹蕊特】并非音译。解释器的本质，就是【支持变量和导入的控伴同】，拿【硬】编码的lookup (LEGB/invokevirtual)，匹配AST【蕊】里的特殊情况，哪怕是线性AST（for.. switch { OP_ADD; OP_RET }）

【有联_应塞类型】在OOP里，class T0{}; class A,B : T0() 时，T0就是A|B的有联类型，通过 enum tag(instanceof) 可执行不同的toString行为，这是C不支持的。应塞类型就像Go的鸭子接口 type any interface{}，invokedynamic T0.toStr时塞入A|B|..的实现，避免了 java Adapter 的尴尬。

【窟启】是很类似PIN和2fa软件-OTP密码的 “临时编号”，就像洞口或端口。这个字符串是登录状态的所有凭据，用于在匿名的http之中识别客户端

【海稀_通证】被OTP密码和json签名验证使用。虽然token今天有词元(Compute Unit) 的意味，它也被用于表示跨端却不可伪造/枚举/重放的凭据。

【授齐易】非常类似一组 stdio s=input();print(s) 的闭环，比如curl的发送-阻塞接受，它处理 Content-Length 和utf8，这是和WebSocket的本质区别

【正择】的复杂性常被我们忽视（grep;cat 编程时立马见效）。PCRE的回溯常被用于 ReDOS，而 (?:num=)(\d+) 的不择取/择取parse许多人却不会。echo足以使用 yes|tail -f 这样的协程、pty进程间信道、ANSI色彩和位置协议，像浏览器般复杂

</details>

## 妙妙词典后记

【扩展1】L 新建pid调用1次自身接口；U 支持cbreak输入和ANSI清屏、保存并恢复使用记录；D 使用nullish或函数式增加错处和defer close

【扩展2】I 从 http-server 读取清单和图标，支持“划走”删除，并支持RegExp匹配和内容高亮。

【扩展3】从链接和 '命令' 获取输出到单数组。支持 -j N -timeout 1 -enc auto 、背压传导、整体.abort不exit。考核并发基础与REPL库的可玩性。可选断点续传（HTTP Range缓存）支持

【扩展J】7GUIs 和 component-party.dev 的所有测试，并与 https://madewithvuejs.com 的主流Apps进行质量对照。 【非常】厉害的作者可以试试 https://github.com/janpaul123/jsdares

> 【程序设计】应该重视【截图和可视化, Q&A, Before&After, Awesome Lists, Build in Public 】五个要素，这样才能称为 Learnable Programming

我发现我掌握的技术，有30%都是类似“把CLI封装回API”，却不能像 import fire, plumbum 那样理解自己在干什么。而且制造分裂。

所以我自称为【软件工艺师】啊。【知道】高深的技巧和细节，不能让你设计出 fire, plumbum 这样的【理解】。 如果你在看书时觉得无聊，那肯定是作者糊涂，而不是你真的不如最新的GPT聪明。

所以使得能够替真人输命令的dsh和bash显得如此强大。这类“软件”不需求服务端开个 /api/v1 才能帮主人拉点【内容】下来。

【同理心驱动开发】，而不是面向薛定谔编程，或面向F5编程，或拿【屏幕之内】的“事件”和“测试”驱动开发。

你用自建站时，登录邮箱这些functional一个不缺，但你开发时，开源世界像【软件失忆】了一样不吱声，提都不提，思路和数据流没有

🤥 It magically works ，这叫干工程，这叫【资深大佬】做的事情。 最后全靠AI。

当我提到flow时，主要是Workflow，但也有 Mental flow, Dataflow 在里边，其实它们是【同像性】的。H5和MC之所以成功，是因为用户体验、开发体验不分家。 Scratch和Flash的技术力，也比圈外人想象的高的多。

许多人还停留在 `wtf.pkg.Flow<T>` 比如 `[[Int Str Pair] Flow]` 上，但我说的绝不是【屏幕之内】的那些，你无需【理解】只用【知道】的技术。它们会失忆，因为在屏幕内。

“支付宝换二维码存相册”这个自分发付费流程，就是 Cookie 怎么变成JWT的，因为服务器群也像用户群一样。如果你先谈Session KV和密码学，我就知道——在代码里你并不理解事情，只是习惯它。

知道一件事物的名字，和知道它，有如上文所示的极大区别。这只是一个场景，但这种【区别】却会决定产品的命运。

## 串读串写支持

ErrDeF 和二进制位宽的0x和\uXXXX定义。

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

## Hall-of-fame

软件是以【它有用时的flow】被开发和使用的，无论lib 框架“App”“容器”，或编程语言。

简单是可靠性的常识，不是可靠性的祭品。都在创新，不明白为什么厌旧。

Bellard和iq是他们编程领域可称为“神”的作者。

LuaJIT，SQLite和stb，llamafile, smallpt, OpenDAW, SerenityOS+Ladybird; ciechanow.ski; handmade.network/m/acegikmo

这些作者，确实可以和iq/Bellard相比，从信噪比和理论准确的角度看——工程界也有不少人，比如 r2 pancake, C# 之父 这样既有灵感也有技术力的作者，简直数以百计

🔍 Rob Pike、Bret Victor、J. Blow、 tonsky.me(Fira Code) 和 Ncase.me …… 这样“非常冷门”但其实启发了一种【全新体验】的人更是有【不拘一格降人才】的感觉

> （如果C#之父和Pike严格提到最前，Linus和NT生态的缔造者们也可以上榜了，那就没完没了了。Sysinternals和toys只是Win极客圈里保底的一部分，如果这样算 YaST libyui 和 AUR hooks 都能分为有品味）

还有一些负面 “语言特性红榜”，如果缺乏它们，就是黑榜：

y2k/c10k/nulls/safe_strs/ paired_ptrs (eg. segfault, funarg problem, io_uring+closures) / newdll_and_signal / zipapp distro.

这些问题的解法都不是高深的算法或厂商的护城河，最后往往要么成为魔法，要么变成大厂商的硬广(Snap, Silverblue)

每个语言都充满了特色精神污染，亦如 java public 与分号，或如Perl傲慢又脑残的单字符，几乎无一例外。

就像PHP和JQ不知道SQLi和XSS会变成那样，或Java认为NPE不是自己的错，bash觉得 "$wtf" 优于 $wtf，做错的设计、没有「品味」的特殊语法和API太多了！

Python以为靠[forif]和《对reduce的批判》就能跳过多行lambda的【事件+闭包】，C世界的设计者用atexit止痛药理解SIGXXX，而失去了React和Qt的Signal

失去了Kay的消息+边界意识，现代才重新发明 io_uring；如果早就意识到.so是伪共享的 new Object，UNIX本来的模型可以和 Qt SIGNAL/SLOT 一样不可或缺。

SIG真的只是taskmgr上挂的 (UserScript) 吗？ .so 真的只是一堆 [Str CDef KV] 吗？ 一句话把问题描述好，少走歧路，这是cyy对Roadmap的【元编程】——特性素材并不稀缺，而关键在主厨

## cyy分发


Docker真的不是少一个systemd单元格的事，它太省心了，你可以一键改yml、看文件库而不用查什么man，这是理所当然的。CI不仅是AI试错的必需品，更是发布/上线的必须。

但每次往【部署】走，都把【开发】的活依赖变成死文件。每次往【容器】走，都会弄丢调试和F5即跑的便捷性。 这简直像userspace的定律。

即便你在【部署时】，也不可避免要到/etc或/{tmp,var}里填写好 first run 或查房，但好像【软件失忆】了一般，它不是按截图录屏里的那个样子，不是以那样的方式被开发的。你不是以【运行频率】为参考敲的每一行代码，而是去补一个个该死的分号。

至少，我们可以先修复一部分。

可以用 `$ rr-www go get` 等命令透明缓存网络资源，之后用 `rr-www bash ./rw_history` 就能离线重复运行。 (proxychains-ng or exe.env)

文件缓存hash支持Range头，--H-no 'X-Amz-* timestamp' 等标准兼容

rr-www 用于单文件分发SDK（录制子进程下载的资源，内容寻址/MIME分组），还支持GFW特色专线。特色专线均在 `~/Proj/cyy/zh_api{.toml,.dl/}`，TUI首次启动后编辑即热更新。默认提供 ghproxy/docker/brew/pip/npm/go/rs/rb/mvn/hf 不再多加。

单文件环境配置：

- `rr-www -g xship  ./file` 基于yadm，激活临时模块，并将主目录之中的file添加到它 `~/bin/xship.zipos/HOME/...`。./file 若包含 `#> ZIP xship blabla\n..\n#>` 段会自动截取、重排入文件尾。F2重命名 `~/bin/xship.zipos` 去掉os即变为可分享的格式。cyy首次启动后，随时可增删zip模块，其文件修改通过 `yadm add; yadm commit` 将打包回zip，但git提交跳过ZIP模块内容。
- 为了方便模块开发，将添加别名 `ln -sfn ~/.local/share ~/.res; ln -sfn ~/.config ~/.app`， .local/{state,bin} 将移动和软链接到.app和~/bin下，>200K .app/文件自动移入
- 移入时自动可执行，并且安装 `$0 complete -s $SHELL` 到 `tree .cache/oh-my-zsh/completions .app/fish/completions .res/bash-completion/completions`

module.prop 自述格式与Magisk相同。`install.sh` 中的 `eget --to ~/bin BurntSushi/ripgrep; git clone $webDir $dst; pm -S pacman-ptr org.flatpkgs` 均会加入cyy的更新和卸载信息面板。若无特别指定(eg. `git --depth=1`) 将应用 `--filter=blob:none --recurse-submodules --shallow-submodules`

关于Wayland桌面：

只需编写一个 ~/.res/myapp/a.desktop 对应到 ~/bin/myapp (myapp/index.{sh,py..})

或者实现 `myapp complete-desktop` svg输出（头两行的格式必须出现在.rodata，即 #embed 里。sh补齐同需包含 `"\ncomplete -F"`）

```ini
[Desktop Entry]
Comment="MyApp" Simple sync (1.0)
C.zh="小程序" 简单同步
Categories=Game
# autogen rest, strip-out "" (1.0)
MimeType=application/x-nsp;x-scheme-handler/nspsync;
#> Autoexec xdg-mime default; update-desktop-database
Actions= # autogen
[Desktop Action ReadOnly] # autogen:
Name=ReadOnly
Exec=myapp --ReadOnly %U

[x-nsp]
Nintendo Submission Package
*.nsp
"PFS0" bytes.0
"PFV0" bytes.0

[svg]
#> Maybe https://vectorizer.ai/ https://SVGco.de
```

.desktop同目录下的 ext_nsp.svg 就是文件徽标，以上可生成 yazi /usr/share/mime/ 所展示的格式。可用 `xdg-open ./fs.nsp or nspsync://` 测试此配置

module.prop 自述默认为首个desktop文件内容，这种情况下，cyy列表中🔧的图标变为🗓️。对于有bin而无桌面，🔧的图标变为💲

启动位置为 .res/myapp。单实例App可以用 ./PWD/argv[1] 读取输入， ./app/_.toml 保存状态。  ~/.app/state/$APP/ 保存自动生成数据和大文件

应用的 env PATH LD_LIBRARY_PATH 均默认为 .res/myapp/lib/ 且可执行，类似 AppImage （WIP: 自动转换为AppDir; jordansissel/fpm 跨发行版?）

zip不支持部分下载，所以采取升级自AppImageUpdate的 erofs zstd(-b 256KB)压缩，desync去重，然后CAS放在 zip -0 HTTP Range 里查尾部CD，去重下载后overlayfs挂载到~/

可以重命名.zipos自动装拆箱，重命名.zip.svc允许开机启动。 删除repack文件，可将dotfiles导出到zip中

`*_GH_bellard.zip` 采用 https://github.com/bellard.keys 的签名，要求 zip.cmt = `64byte(zip[0:eocd+22]|sha256|ECDSA)`

内部有 file.{exe,dmg,bin}.url 平台特定运行时下载支持， README.webp 产品图和注释支持

安卓kt2apk可见 https://buck2.build/docs/prelude/rules/android/android_binary/

包如果依赖一些x64或通用文件，可以从任何发行版(rpm,deb..)处下载。 搜索 pkgs.org

```sh
u="https://github.com/Limine-Bootloader/Limine/releases/download/v7.7.0/limine-7.7.0.tar.lz"; curl -fsSL $u | bsdtar -xOf - 'limine-7.7.0/PHILOSOPHY.md' > b
curl -fsSL $u | bsdtar --strip-components 1 -xf - '*PHILOSOPHY*'
egets $u 'limine-7.7.0/PHILOSOPHY.md' #./b
egets $u. '*PHILOSOPHY*' ./

u="https://github.com/BurntSushi/ripgrep/releases/download/14.1.1/ripgrep_14.1.1-1_amd64.deb"; curl -fsSL $u \
  | bsdtar -xOf - 'data.tar.*' | bsdtar -xOf - '*/bin/rg' >bin/rg
egets .deb not.exe BurntSushi/ripgrep # select dl-cmd

#dnf install "pkgconfig(yaml-cpp)" "libssl.so.3()(64bit)" /usr/bin/rustc; pacman -F
#> pm pk yaml-cpp; pm so ssl libssl; pm rustc --version # clipboard (CMake/ld/cnf) sel & $ pm supported

k=yaml-cpp; curl -s -A "cyy/1.0" \
  "https://repology.org/api/v1/project/$k" | \
  jq -r '.[] | "\(.repo): \(.binname // .srcname)"' | sort -u
```


## cyy-repls

尽管cyy的笔记本前端被用于优化 py/frida.js/lua dlsym 直接连接，它仍可通过stdio的一行一次exec实现。执行前 replace('\xBB', '\n')，之后也 printf \n\xBB，若执行返回时stderr有新数据，则视为失败

不支持 input()。ANSI色彩和进度条等光标控制，非cyy语言多行输出会自动注释。热重载等Squeaky功能均不可用，但默认开启OK检查点

pahole(from perf)+luajit 提供和读写C的类型信息与，可以处理 c=dup4ret({}) hook返回时深拷贝json、多线程、hook死递归、gcc -E。我的需求类似于直接对知道ABI的部分ldd符号做hook

```sh
#TODO \n quote, errs
#> pip install pipreqs

python3 -c "import sys; class A(dict): __missing__ = lambda s, k: s.setdefault(k, __import__(k)); g = A(__builtins__=__builtins__); [exec(l, g) for l in sys.stdin if l.strip()]"

#//()>
node -e "const vm = require('vm'), ctx = vm.createContext(new Proxy(globalThis, { has: () => true, get: (t, p) => p in t ? t[p] : (t[p] = require(String(p))) })); require('readline').createInterface({input: process.stdin}).on('line', l => { if (l.trim())  try { require('vm').runInThisContext(l) } catch(e){ console.error(e.message); } })"

#--()>
lua -e 'setmetatable(_G, {__index = function(t, k) local ok, m = pcall(require, k); if ok then rawset(t, k, m); return m end end}); for l in io.lines() do if l:match("%S") then local fn, err = load(l, "=stdin", "t", _G); if not fn then io.stderr:write("SyntaxError: " .. tostring(err) .. "\n") else local ok, rerr = pcall(fn); if not ok then io.stderr:write(tostring(rerr) .. "\n") end end end end'

ruby -e 'def Object.const_missing(c); require c.to_s.gsub(/([a-z])([A-Z])/, "\1_\2").downcase; const_get(c); end' -ne 'eval($_, TOPLEVEL_BINDING)'

php -R '$c=trim($argn); if($c!=="") { if(substr($c,-1)!==";") $c.=";"; try { eval($c); } catch (\Throwable $e) { fwrite(STDERR, "Error: " . $e->getMessage() . " in stdin\n"); } }'

#> mise use -g jbang kotlin@latest
#> iex "& { $(iwr https://ps.jbang.dev) } app setup"

#> dotnet tool install -g csharprepl
#> go install github.com/cosmos72/gomacro@latest
```

如果文件启用了 `#> hito .lua $it` 这样的行并被 Ctrl+X 执行，比如 `nvim ~/.config/nvim/init.cyy`,就会为它安装热重载，直到此行删去

笔记本，如 a.ho.py 均可使用 %%magic ，如 `%%write ~/.libc.h` 改写依赖文件。可以载入 `%%write .env`。 %%gish 使用bash方言，支持win11

最常见的是 %%test 要求输出自添加时保持一致，以及 %%init ，它让格子的编辑行为，触发从curCell到lastUsedCell的遍历执行

%%write ./file 将通过 C-Shift-S 找到当前路径，并在执行前读取其内容到剪贴板。这样 C-X C-V 就可以更新格子了。

cyy的 Ctrl+Shift+S 先提交后命名，snack弹框另存为或提交名。默认只列出带"(..)"的提交，也要求，第二号提交是 `git commit -m 'template(): Gtk4'`，也就是在全是项目样板文件的情况下，要提交1次！！ 再去改代码。

NVim用户需要特别设置以模拟micro  ¯\(°_o)/¯, 请参考 [vscode.nvim](https://www.lazyvim.org/extras/vscode)

```lua
-- kate ~/.config/nvim/init.lua
-- $ nvim -- try Press: key s, p, c, x (keyseq: / lang.ang  q)
require("config.lazy"); local Pek=Snacks.picker
vim.cmd.colorscheme("catppuccin" or"tokyonight"or "aether")


-- Feat: $(pkconf), f5 ./ref line + SinglePage App HotReaload with kwin_wayland
vim.opt.keymodel = "startsel,stopsel"
vim.opt.selectmode = "mouse,key"
vim.api.nvim_create_autocmd({ "BufEnter", "BufWinEnter", "CmdlineLeave" }, { callback = function()
    if vim.bo.buftype == "" and vim.api.nvim_buf_get_name(0)~="" then vim.cmd("startinsert") end
end})

vim.api.nvim_create_autocmd("User", { pattern = "VeryLazy", callback = function()

for lhs, rhs in pairs({
    ["<M-e>"] = vim.lsp.buf.hover, ["<C-d>"] = Pek.lsp_symbols, ["<M-d>"] = vim.lsp.buf.code_action, ["<F2>"] = vim.lsp.buf.rename,  ['<C-S-c>'] = omnibox_show, ['<C-p>'] = omnibox_show,
}) do vim.keymap.set({ "n", "i" }, lhs, rhs, { silent = true }) end
end})
for mode, maps in pairs({ -- ZXCV protocol
    i = { ["<C-z>"] = "<Cmd>undo<CR>", ["<C-S-z>"] = "<Cmd>redo<CR>", ["<C-x>"] = '<Esc>"+ddi', ["<C-v>"] = "<C-r><C-o>+", ["<M-x>"] = "<Esc>:" },
    s = { ["<C-c>"] = '<C-o>"+y<Esc>i', ['<Right>'] = '<Esc>`>a' },
    [{ "n", "i" }] = { ["<C-a>"] = "<Esc>ggVG<C-g>", ["<C-Left>"] = "<C-o>b", ["<C-Right>"] = "<C-o>w",
        ['<F1>'] = '<cmd>q<cr>', ["<C-s>"] = "<Cmd>w<CR>", ["<C-e>"] = "<cmd>Neotree toggle<cr>",
        ["<F5>"] = "<Cmd>ClangEntr<CR>", ["<M-LeftMouse>"] = function()
        local p=vim.fn.getmousepos(); vim.api.nvim_win_set_cursor(p.winid, { p.line, p.column - 1 })
        Pek.lsp_references()
    end },
    [{ "n", "v" }] = { ["<M-x>"] = ":", ['<CR>'] = 'i', ['<Backspace>'] = 'i', ["`"] = function() (#vim.diagnostic.get(0) > 0 and Pek.diagnostics_buffer or Pek.notifications)() end },
}) do
    for lhs, rhs in pairs(maps) do vim.keymap.set(mode, lhs, rhs, { silent = true }) end
end

function omnibox_show()
local key_CSC = {
    [">"] = "commands",
    ["/"] = "grep_buffers",
    ["#"] = "grep",
    ["@"] = "that, Ctrl+D list func",
    ["!"] = "that, SPC+fT open term",
    ["?"] = "pickers"
}
local u, s = Pek.smart(), ""

vim.api.nvim_create_autocmd("TextChangedI", { buffer = u.input.filter.buf, callback = function()
    s=key_CSC[vim.api.nvim_get_current_line():sub(1, 1)]; if s then Pek(s); pcall(u.input, 'set',"") end
    end })
end

function wl_iframe_xywh(term, c_win)
c_win = c_win or 'c.skipSwitcher = true'
local win = type(term) == "table" and term.win or (term or 0)
local pos = vim.api.nvim_win_get_position(win)
local rx, ry = pos[2] / vim.o.columns, pos[1] / vim.o.lines
local rw, rh = vim.api.nvim_win_get_width(win) / vim.o.columns, vim.api.nvim_win_get_height(win) / vim.o.lines
local js = string.format([[
    let p = workspace.activeWindow, c; //<v parent child upd(onmove, act z-index)
    let s = workspace.windowAdded;
    let a = workspace.windowActivated;
    let u = () => { if (p && c) { let g = p.frameGeometry; c.frameGeometry = { x: Math.round(g.x + g.width * %f), y: Math.round(g.y + g.height * %f), width: Math.round(g.width * %f), height: Math.round(g.height * %f) } } };
    let act = w => { if (c) c.minimized = (w != p && w != c); };
    if (p) p.frameGeometryChanged.connect(u);
    if (a) a.connect(act);
    let onAdd = w => { if (w != p) { c = w; s.disconnect(onAdd); c.keepAbove = true; %s; u(); c.closed.connect(() => { if (p) p.frameGeometryChanged.disconnect(u); if (a) a.disconnect(act) }); ; workspace.raiseWindow(c) } };
    s.connect(onAdd);]], rx-.02, ry, rw+.02, rh-.01, c_win)

vim.fn.writefile(vim.split(js, "\n"), "/tmp/wm_reparent.js")
term:show() -- area_rxy*wdiv (0.0 ~ 1.0)
end

--> yay -Qi jansson json-c json-glib curl gtk3 qt6-base
--> yay -S --needed base-devel bear entr mold ccache gdb perf labwc gtk3 gtk4 libadwaita glade cambalache
--> yay -S --needed base-devel meson ninja cmake qt6-base qt6-declarative qt6-tools qtcreator gnome-builder
function sh_ok(s) return os.execute(s) == 0 end
_G.CFLAGS=' -g3 -O0 -fPIC -fsanitize=address,undefined'..(sh_ok("which mold") and " -fuse-ld=mold" or"")
--> echo>~/.libc.h '\# include <signal.h>\n__attribute__((constructor)) static void f5(void) { signal(15, SIG_IGN); }'

;  vim.api.nvim_create_user_command("Ldd", function(opts)
local s = opts.args ~= "" and opts.args or "jansson json-c json-glib-1.0 libcurl gtk+-3.0 Qt6Widgets Qt6Quick"
if not s:find("%-%-") then s = "--cflags --libs " .. s end

local cmd = string.format("pkg-config %s | tr ' ' '\\n' > compile_flags.txt", s)
vim.notify(cmd, vim.log.levels.INFO)
if sh_ok(cmd) then
    vim.notify("✓ PKConf compile_flags.txt ", vim.log.levels.INFO); vim.cmd("edit!")
end
end, { nargs = "?",
    complete = function(lead)
return vim.fn.systemlist("pkg-config --list-all 2>/dev/null | awk '$1 ~ /^" .. lead .. "/ {print $1}'")
end })


; vim.api.nvim_create_user_command("ClangEntr", function()
local f5_ = vim.api.nvim_get_current_line():match("f5%s+(%.%/%S+)"); if f5_ then vim.cmd.edit(vim.fn.fnameescape(f5_)); return end
local pkg_ = vim.api.nvim_get_current_line():match("%$%(pkg%-config%s+([^%)]+)%)"); if pkg_ then vim.cmd.Ldd(pkg_); return end
local file = vim.fn.expand("%")
local cmd = vim.api.nvim_get_current_line():match("^%s*[#/%-]+%s*>%s*(.-)$")
if cmd then
    cmd = cmd:gsub("^%b[]%s*", "")
    if #cmd > 0 then vim.cmd("botright 10split | terminal it="..file..";" .. cmd); return end
end
local out = vim.fn.expand("%:p:r") .. ".com"
local msg = "\x1b[96m:lua _G.CFLAGS = ".._G.CFLAGS.."\x1b[0m".." && ID=$(qdbus org.kde.KWin /Scripting loadScript /tmp/wm_reparent.js) && qdbus org.kde.KWin /Scripting/Script$ID run"

cmd = string.format(
    [[ls %s window.ui window.qml | entr -rc sh -c '%s %s -include ~/.libc.h $(cat compile_flags.txt) -o %s && echo %s && ]]..
    [[exec env ASAN_OPTIONS=detect_leaks=0 %s & (sleep 1.5&&kill -KILL $(cat /tmp/wm.pid) 2>/dev/null ||:) && echo $! > /tmp/wm.pid']],
    file, (sh_ok("which ccache") and "ccache " or"")..(file:match("%.cpp$") and "c++ -std=c++20" or "cc").._G.CFLAGS, file, out, msg, out)

if sh_ok('grep -q -wE "gtk_init|QApplication" '..file) and _G.Snacks then
    wl_iframe_xywh(Snacks.terminal.get([[echo "
\x1b[1;36mGTK3\x1b[90m/\x1b[1;36m4\x1b[90m/\x1b[1;32mQt5\x1b[90m/\x1b[1;35mQML\x1b[0m
\x1b[1;33mCtrl+Shift+C + >\x1b[0m \x1b[1;32mLdd\x1b[0m \x1b[1;36mEnter\x1b[90m/\x1b[1;36mTab\x1b[0m for all libs

Esc: SPC+SPC Go Everywhere, SPC  quick-ui
Ctrl+{E,D} trees, Alt+{E,D} Doc, {Ctrl,Alt}+Click tags
Alt+X VimCmd, F1 quit, [~] errs_notify, [s] 2key-jump
";tee]], { id = "wdiv", auto_close = true, win = { position = "right", width = 0.45 } }))
else vim.fn.writefile({""}, "/tmp/wm_reparent.js") end

vim.cmd("botright 10split | terminal " .. cmd); vim.bo.bufhidden = "wipe"
end, {})

vim.lsp.enable("clangd")

```
