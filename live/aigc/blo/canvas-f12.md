# 记一次pip离线包管理-以及调试Chrome彩蛋の里技！

假设你要在许多电脑上安装同一个 py SDK，想加快下载速度，可以把安装包在本地存好（`pip download; pip install *.{whl,tar.gz}`），通过 [file.pizza]() 等P2P网盘分发安装

但是：只要访问不了 PyPI.org ，离线安装就有可能出以下错误：

```
{Retry下载setuptools一直失败}
× pip subprocess to install build dependencies did not run successfully.
│ exit code: 1
╰─> See above for output.
```

为啥不用绿色版 `sys.path` 大包呢？

玩Linux的应该知道，Ubuntu现在用的安装方式和解压.GHO镜像很像，是 `squashfs` 直接平A到根，再运行 chroot setup。如果请APT处理那个依赖……那可是要慢三四倍（可见 `pacman` 同行的工作逻辑有多么正确）

但如果你是老师的话，总要教学生咋搭建环境，而不是解压+注册表。 自己配环境比用鸭嘴笔还痛苦，但为了工作效率必须学啊！

> 先定位 root cause，寻找改参数修复的方法

先找源码位置。虽然没有 java.exe 那样的 Ctrl+| =调用栈dump，也可以在 `python --help-env` 里找调试参数 ~~

`$env:PYTHONVERBOSE=1; python -m this` ！它能跟踪 `__import__('this')` 包依赖树的文件位置。

执行并搜索"pip"，就知道可在code里打开 `~/AppData/Local/anaconda3/Lib/site-packages/pip/_internal` ，并搜索 `pip subprocess` 等子串：

```py
        args.append("--")
        args.extend(requirements)
        # breakpoint() #< Pdb断点
        with open_spinner(f"Installing {kind}") as spinner:
            call_subprocess(
                args, #v AOP的切入点。 搜索甚至覆盖 strings *.so 也是静态分析里最简单有效的
                command_desc=f"pip subprocess to install {kind}",
                spinner=spinner,
            )
```

~~ 往上翻，先尝试让它用离线包

```py
    @staticmethod
    def _install_requirements(
        pip_runnable: str,
        finder: "PackageFinder",
        requirements: Iterable[str],
        prefix: _Prefix,
        *, #v  纯粹kwarg
        kind: str,
    ) -> None:

        args: List[str] = [
            sys.executable,
            pip_runnable,
            "install",
            # "--ignore-installed",
```

报错！启用Pdb那一行的断点。

pdb REPL 的有效命令大概是 `puawcl` (print up args where cont list) 几个。 `u; a; p prefix.path` 可以看见，py和pip都是基于另外的PATH

> 发现构建时基于虚拟环境：最基础的setuptools，都要在线装

哈哈，Pythonic 所谓的 batteries included 呢？世界构建系统都一样烂，java龟斗特别烂

既然不能用全局容器的tools安装.whl ，只好利用mirror了 ~~

`pipi='pip install -i http://localhost:8000/'` （其实把 `args` 里的构建依赖名replace到本地也行）

`python -m http.server` 能看到pip默认是会列出、下载 https://pypi.org/index/simple 下的 `/{name}/*name-ver-ABIs.whl`

~~ 我们把放whl/tgz的文件夹备齐，改成这种前缀模式即可做离线镜像。

```pwsh
```

有意思的是，pip不区分[-_ ]，比如 `pip show mysql_connector_python` 会显示my官方的py绑定

查看模块文件也可以用： `python -c "import os;os.system('start '+__import__(os.getenv('m')).__file__)"; $env:m='this'` 

- idle.exe IDE 里调用的 `py -m turtledemo` 包含的tree脚本挺好看的，他也可以看看py模块列表 (Path浏览器。包括选中import名，Alt+M)。 
- `python -m panel serve` 可以在Jupyter的"ipykernel"右边点开。依赖bokeh的py前端框架，包含了DOM的typedefs `wc ~/AppData/Local/anaconda3/Lib/site-packages//bokeh/server/static/lib/lib.dom.d.ts` (1M呢！文件夹里剩下的 js SDK defs 也才1M. ES6==ES2015)，而 mypy.typeshed 也是2.4M， 当然你写个ts再在[VSCode/Go菜单/T键]也能看到全部的Web接口
- `/site-packages/pygments/lexers/_mapping.py` 包含了所有流行语言的词法(搜索 `/\b[EA]BNF|^#.*parser/, /grammar (?:is|for)/`, 包含cpp!)。 对了， JMESPath Matcher 有点像 bash jq, prompt_toolkit 是REPL非AI, llvmlite.tests.test_ir 支撑了Numba(Xarray,LIEF kaitai)？  `python -m sympy.printing.tests.test_rust`
- pandas/pyProject.toml 有近1k行！ dill 支持解释器级序列化 dump_module, obj_diff, getsource, `parent(iter(['queryObjects']), list)`, import boltons as builtins..
- 想渲染(支持gfm等扩展的md)？ `ls|%{ python -m markdown -o html -f "out/$($_.basename).htm" $_.name }` ps. -o-f搞反了吧
- black|flake8|yapf|cffi|-m pydevd(可attach,关键词PyObjectHolder) 是查错工具, mypy|jedi|astro|pylint|sphinx文档生成。只需让 $b=http.server的目录 `sphinx-apidoc -F -o $b (pip文件夹); sphinx-build $b $b/doc` 即可读本地pip的文档！ (sphinx-quickstart $b 可生成utils/conf.py文件)

想体验所有主题，只需3行：

```js
//先 ls sphinx/themes|%{sphinx-build.exe -D html_theme=$_ $b/b $b/doc/$_}
document.body.insertAdjacentHTML('beforeEnd', `<iframe src=basic style="width:100%; resize: vertical">`)
onclick=(ev, e)=>{if(!(e=ev.target).matches(`a`))return; frames[0].location=e.href; ev.preventDefault()}
```

再来个好玩的，标准库里也有中文！

```python
import linkify_it.tlds as web
import encodings.punycode as asci
import pyuca as ucd
# also rich._emoji_codes
a=[asci.punycode_decode(s[3:], '') for s in web.TLDS[1341-10:1495-10]]
sorted(a, key=ucd.Collator().sort_key, reverse=True)
'/local/anaconda3/lib/site-packages/tldextract/' #这里也有

#词频
import charset_normalizer.constant as c;c.FREQUENCIES['Chinese']

#python -m nltk.test.unit.test_tokenize
```

不得不说计算机老人们写的工具，还是值得探索一番。 黄金之风啊JOJO。 

## chrome:dino金手指+连点器

先礼后兵！ 下面才是重头戏。

谷歌家的安卓有OS版本彩蛋，开启方式类似[开发者模式]，是暴力点击设置的相关列表项（我认识的还有点屏幕四角跳过首次开机屏、拨号4636，STFW）

[壳容,] 作为完成度比安卓高到不知哪里去了的平台，彩蛋更是非常显眼，所谓“灯下黑”，就在断网页面上（自己找）

因为是 about:// 的资源，[F12/Source/Page] 里不能直接看到js。 可以先访问 `about://about` +F12，再前进后退到Dinosaur彩蛋

> 定位源码，禁用函数

选中body，在[样式]栏右边，开启[祖先]，才能看到 `keyup` 监听器，跳过去。你会查到 `handleEvent;update;gameOver` 几个关键函数，以及其类型Runner

搜索 `new Runner`，发现值和游戏大陆(Horizon)似乎没被保存（只是在requestAnimationFrame事件里持有了），那就 `queryObjects(Runner)` ，[0]存储为全局变量 ~~>

`temp1.gameOver=()=>{}` 就可以刷分，但看起来很鬼畜，似乎恐龙完全不在图层以内……

> 其实模拟一下空格键就行了

关键词 `onKeyDown, if Runner.keycodes.JUMP[e.keyCode]` ，相对的， `e={keyCode:Object.keys(Runner.keycodes.JUMP)[0]}` 就能触发跳一跳

`temp1.gameOver=()=>{temp1.onKeyDown({keyCode:32, preventDefault (){} })}` ，能用了

自动高跳（不以keyup打断跳跃）！但，我还希望恐龙能“预判”前方的障碍，也就是把他的碰撞体调的比精灵贴图宽一点。

搜 `this.gameOver()` ，向上找到 `checkForCollision(this.horizon.obstacles[0], this.tRex)`

我们要调的是 `tRex.getCollisionBoxes()` ，宽一点 ~~>

方法是直接Ctrl+F它，打断点，在函数内改，F8（直接在悬停UI里双击数值即编辑）

……没效果。排查是 `createAdjustedCollisionBox(AB)=new CollisionBox` .width 里没有调用 `A` 的数值（=小恐龙体内的CollisionBoxes[i]），而只能暴改 B=`let tRexBox` 的“预筛选盒” ~~

在let下方一行行号，右键条件断点 `void(tRexBox.width+=60)` （日志断点会一直输出烦人的log。 又不支持直接加HTTP内容替换……）

`temp1.tRex.getCollisionBoxes()[0].width+=50` 配合下碰撞体预筛选盒的调整

一次成功。

~~ 从框架角度看，预筛选盒必须从小恐龙的内部盒子计算，而不是硬编码 (尤其是需要在不同位置/文件里耦合后！)；因此，爆改别人的代码时，也别忘了熟悉他们的风格，有点心理预期，省不少麻烦。

接下来，是使用 `import pyautogui as pa` 检测恐龙，躲避屏幕中心黑色项的方法。

```py
hw=next(u for u in g.getAllWindows() if '恐龙游戏' in u.title)
```

黑盒的好处就是100%兼容，与代码版本无关。不要低估基于A11y的油猴脚本，它们能实现的参数化和健壮性并不低端！

## 调试技巧

本文的 `代码段` 可以选中拖放到F12执行。 **这是“动苏日码”系列的样板文章，关键代码段一定出现在行首、`~~>` 行尾意味着自己探索，即刻得到下一行的答案。**

调试他人的代码基，是优秀程序员的根基，同时也最适合业余开发者。 在理解别人组合出的功能点or框架时，咱们首先要学会【做减法】。许多大佬引以为豪的“模式”，对我们而言只是过度工程和障眼法。存在\\=合理

不要使用 `Ctrl+Shift+{C,F,F8},F10, 右键 [继续到此行] [评估/监视表达式]` 以外的快捷键或界面栏。 

慎用F10单步执行！如果你在大脑里跑不通最基本的时序块和流控（比如对app生命周期没概念），调试器对你来说，就只有少写点临时 `print()` 和“游戏修改器”的功能。

多用 `Ctrl+{|暂停主循环,F8停用断点,ShiftO/P函数跳转/点开文件}`。预启用 `throw异常时，XHR/fetch地址时，(DOM变更)事件监听器` 的断点也非常有效，推荐一下！

`let a=1; dir(()=>a); getEventListeners(document)` 下拉可以看到闭包变量a的值，这个非常非常实用（打破 Lexical Scopes 的封“闭”）

同样霸道的事情py里也易如反掌：

```python
import inspect as I
??I.getclosurevars

def SAM(): #constructor
  a=1; f=lambda:(a, I.getclosurevars(f))
  return f

SAM()()
```

Workspace+紫色Override 也是F12里最有用的功能（没有之一）！ 不过，在调试 sw.js（PWA离线缓存）等技术时，千万要用前者，比如在F12直接编辑 `http-server -c-1` 收发的文件。 不要被莫名其妙的"bug"（其实是工作区没清扫好--被基本功）给卡住哦，要熟练于最小可复现的道理。

除了F12的“油猴”代码段，善用 Jupyter Notebook (Anaconda 自带) 所支持的 `%%js, %%file a.py` 和 ipywidget UI，也很会写实用性文件了。调包 `%autoawait, showast, traceback_with_vars` 这也都是无print调试秘技

最主要，还是多涉猎 `document.__defineGetter__('body',()=> ({}))` 这样的原型链/元编程、CSS AOP，才能写好油猴脚本啊。

## DevTools App

快捷键

```js
在控制台中评估所选文本 (重命名 Ctrl+D /F7。主要用于测试单个函数的修改)
Ctrl+B

将[内存] [变更] [Lighthouse]标签右键移至底栏，顶栏保留5项： 元素UI 源码UX 应用API 网络UGC 记录器UE 即可

底栏可保留： 控制台 AI 快速来源(元素面板,右键链接Enter几次即可打开) 动画 搜索 覆盖率(函数热点) 内存(性能应通过UGC面板做PGO)

显示/隐藏抽屉栏
Esc

创建实时表达式 (也用于避免 长文本/参数赋值 干扰历史记录)
Ctrl+Shift+F8

清除控制台
Ctrl+L

下一个面板
'Ctrl+]'

关闭源文件 (WERO导航)
Alt+W
上一个编辑器
Alt+E
下一个编辑器
Alt+R
在导航区域显示当前编辑器
Alt+O

开启/关闭导航器边栏
Ctrl+Shift+E

开启/关闭调试程序边栏
Ctrl+Shift+R

检测覆盖率
P
F2重命名，大家很习惯了。Ctrl+F F3,F4搜索列表 也有人知道。 元素面板上[AH]也利于截图使用！
```

建议启用的设置

```js
默认缩进：2 Space
显示空格字符：尾随

在开发者工具已打开时停用缓存 //注意！！以后你可能为了速度而关闭此项
启用 Ctrl + 1-9 快捷键切换面板 //开启应用状态面板只需 Ctrl+3

按 Enter 键时接受自动补全建议
记录 XMLHttpRequest
停用异步堆栈轨迹 //不要把rAF等回调识别为递归async

启用本地替换
火焰图导航：现代  //代码热点Profiler 移动缩放需使用Ctrl/Shift+滚轮 。注意，(console.timeEnd) 会显示在耗时火焰图上。

[忽略列表]
来自 eval 或控制台的匿名脚本

[CPU 节流]
校准一次

[快捷键]
记录重放器 Ctrl+E
```

这些除错利器可以辅助你掌握，并最终控制代码块背后的逻辑，还是值得一学啊~

现在，按一下 Ctrl+Shift+F8 ，我有个给F12上加按钮的小技巧。

## canvas调试

H5总体上来说是世界第一好调试内查的SDK了，没有C那样的无类型指针，也没有Java/py反射的繁琐，一切均可替换。

但碰到canvas绘制的界面交互，许多人依然会被卡住。 排除 WASM,Unity 这些伪js的情况，那就是因为DevTools不支持像查看事件监听、XML嵌套一样处理2D碰撞和精灵/地图了

然而这个支持很好做，只要你能从调试步骤里设计工具。

首先有一个UX更复杂的最初版本。 试着思考其中关键时序的差别？

```js
queryObjects(CanvasRenderingContext2D)
//将项0保存为temp1

sort=(a,f)=>a.sort((A,B)=>f(A)-f(B))
hook=(g,k,f, f0)=>{f0=g[k]; g[k]=function(){let r=f0.apply(this,arguments); f.apply(r,arguments);return r}}
absdiff=(a,b)=>a.reduce((A,B,i)=> A+Math.abs(B-b[i]) ,0)

{let x=0,y=0,g=temp1, cookie=r=>{g.fillStyle=fgDBG; g.fillRect(x,y, 3,3) },
  fgDbg,fgDBG,kf='绘制背景';
(g.canvas.onclick=u=>{
  u.shiftKey&&u.ctrlKey? (x=u.offsetX,y=u.offsetY,0) : 0;
  fgDbg=new Uint8Array(3).map(x=>Math.random()*255), fgDBG=`rgb(${fgDbg.join()})`
})({}); hook(g,'clearRect', cookie)

sort(Object.keys(g.__proto__), k=>/^(fill|stroke|draw)/.test(k)).slice(65)
// sort(Object.keys(g.__proto__), k=>/^(clear|getImageData)/.test(k)||!g[k].call).slice(0,43)
.map(k=>hook(g,k,function(r){
  if(absdiff(fgDbg,g.getImageData(x,y,1,1).data) <5)return
  let k=Error().stack.split('\n')[4].slice(7)
  if(k==kf){return}else{console.info(kf=k)}
  x=y=0; '这是为何？'||cookie(); debugger } ))
}
```

```js
// 每帧开始(或鼠标划过)时刷新一个亮点像素。 绘制单项后，若亮点已被遮盖，且栈顶段不在忽略列表中，断点并记录函数名。 (亮点位置=Ctrl+Shift+点击 or 悬停)
grabug=(c={ctx:1})=>{
const CANVAS_DIRTY=/^(fill|stroke|draw|putImageData)/,
  rgb2hex=a=>a.slice(0,3).reduce((A,B)=> A+B.toString(16).padStart(2,'0'),'#'),
  on=(e, dep=(k,f)=>(e.addEventListener(k,f), dep))=>dep,
  hook=(g,k,f)=>{let f0=g[k]; g[k]=function(){let r=f0.apply(this,arguments); return f.apply(f0.bind(this),arguments)?? r}}

if(!c.ctx.canvas) return hook(HTMLCanvasElement.prototype,'getContext', (i=>function(A,B, r){if(i++!=c.ctx){return}
  B??={};B.willReadFrequently=true; grabug({...c, ctx: globalThis.gra=r=this(A,B)}); return r
})(1))

let g=c.ctx, x=0,y=0, fgHit='',
  fgCookie=()=>{fgHit=rgb2hex(new Uint8Array(3).map(x=>Math.random()*255))},
  z=0, fnIgn=(c.sdkFuns=new Set((c.sdkFuns||'').split('\n'))),kFn='',

  stopTail=fnIgn.has('-stopTail'), cssHit=/filter:(.*)/.exec(c.cssHover)?.[1]?? 'brightness(0.8) sepia(1) hue-rotate(-134deg)',
  debounce=0, F5=g.fillRect, e=g.canvas, ec=e.style;

if(e) { on(e)
  ('contextmenu',u=>{z=2; u.preventDefault()})
  ('auxclick',u=>{z=3; fgCookie()})
  ('mousemove',u=>{x=u.offsetX,y=u.offsetY; if(e)ec.translate=`${x-e.offsetWidth/2}px ${Math.max(20, y-20)}px`})
  let hide
  if(c.cssHover=='') {e=null; hide=()=>{ec.cursor='initial'}} else {
    e.insertAdjacentHTML('afterEnd', '<tooltip  style="display: inline-block; border: 1px solid; transition: 70ms opacity ease-out; will-change:transform;pointer-events: none;">')
    e=e.nextSibling, ec=e.style, hide=()=>{ec.opacity=0;debounce=1}
  }
  setInterval(hide, 250)
}
fgCookie();
hook(g,'clearRect', ()=>{
  let v0=g.fillStyle; g.fillStyle=fgHit;
  F5.call(g, x,y, 1,1); g.fillStyle=v0
  z=stopTail&&x==0? 0 : z
})
Object.keys(g.__proto__).map(kf=>{
  if(!CANVAS_DIRTY.test(kf))return;
  hook(g,kf, function(){
    if(x==-1||fgHit==rgb2hex(g.getImageData(x,y,1,1).data))return;
    if(!e){ec.cursor='crosshair'}
    if(e||z!=0) { // or [no tooltip mode]
      let v0=g.filter; g.filter=cssHit;
      this.apply(0, arguments); g.filter=v0

      let a=Error().stack.split('\n'), i=3, N=a.length
      while(i<N) {if(!fnIgn.has(kFn=a[i].slice(7)))break; i++} if(i==N){return}

      let fun=kFn.split(' ',1)[0]
      if(!e) { console.info(fun) } else
      if(debounce||z) { debounce=0; e.innerText=fun;ec.opacity=1; if(z)console.info(fun) }
    }
    if(z==2) { z=stopTail? z : 0; x=y=(stopTail? 0 : -1);  debugger } else
    if(z==3) { z=0; fnIgn.add(kFn) }
  })
})
}// grabug()

grabug([{ctx:Runner.instance_.horizon.canvasCtx, cssHover:''},
{ctx:Runner.instance_.horizon.canvasCtx, },
{ctx:1,sdkFuns:'-stopTail', cssHover:'filter: invert(1) '}][1])

;(_1=Runner.instance_).gameOver=()=>{_1.onKeyDown({keyCode:32, preventDefault (){} })}

```

不瞒你们说，这代码写长了虽然功能齐全，但有时也会把全局表/某个函数分块的变量搞混（eg.生命周期尽量改短，但忘记别处引用了…… 尤其是用先堆砌后设计的 原型-重构工作流）

如果我用最“规范”的缩排，大概就写不出这些功能了吧。可以说有得必有失。 F12里列出泄露变量的最好方法是 Ctrl+| ，随便触发一个事件，然后看调用堆叠下方的[Scopes]，很容易区分开全局块里/globalThis.表上的新加变量

Workspace+Override 也是F12里最有用的功能（没有之一）

## 题外话-嘿客

> 为了 anti DevTools ，一些脚本会疯狂调用 `if(true) debugger` 来卡人，为此右键有显示忽略列表、禁止匿名脚本调用debugger的选项

ps. 有些人觉得自己会MITM，调用个 [WireShark GUI]() 抓包就是嘿客了，其实连 SSL Pin证书都破解不了，重放攻击都不会。 `chrome://net-export` 是浏览器级别的抓包，数据更好看，工具更趁手。不懂，网安届为啥不用F12的UX显示native的http(s)调用，做一个polyfill，非得去增加工作量。

~~

`monitorEvents(document,'click')` 等DOM小工具[在这里有文档]()，打开 `chrome://inspect/#pages` ，调试DevTools本身，在源面板文件树右键[搜索] "dirxml", "inspectRequested" 即可看到有 `debug(alert)` 等罕见的CDP协议命令，甚至姑狗大厂的实现（笑哭）。 

>对了，在搜索源码时，猜对关键词/核心或热点函数/必调API是很重要的。 “深入浅出”要找对点

`this.nextMessageId(); c.dumpProtocol ` 这两个关键词，在F12自身的源码里，就是和 **前后端分离** 相关的重要API实现细节，与所有业务逻辑耦合。

再通过搜索/inspectRequested/g，你可以定位并log重放重要逻辑，搞懂 `queryObjects(ResizeObserver)` 等命令的json形式，才能够进入开发F12这样的大型工具

再比如，有案例：

>在VSCode中禁用 `compositionstart` （输入法拦截）

- 全局搜索Event名字符串(到 `this._context.viewModel.onCompositionStart` 附近)
- 在u=> 上打断点： `let f=i.addEventListener; i.addEventListener=(k,...a)=>/^composition/.test(k)?0: f(...a)`
- 或在u=>内打断点： `let e=this.b,ty='compositionstart'; e.removeEventListener(ty,getEventListeners(e)[ty][0].listener)`
