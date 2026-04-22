# Nebula Remix

[Green Fog wallpaper](https://duangsuse.github.io/NebulaRemix/live/?algo=A%3A+Cotton&color=Toxic+Bio&M=1&W=9.4&S=8&K=1.2&G=0.43&O=10&flu=3&b=0%2C0.1%2C0.05&f=0.3%2C0.8%2C0.2&h=0.9%2C1%2C0.8), [Try Mandelbrot Fractals!](https://duangsuse.github.io/NebulaRemix/mandel.html)

A WebGL reconstruction of the recursive domain warping technique found in Android's ["Magic Smoke" live wallpaper](https://live-wallpaper.fandom.com/wiki/Magic_Smoke).

It simulates fluid dynamics and cloud formations not through Navier-Stokes equations, but via iterated fractional Brownian motion (FBM), where the noise domain is distorted by the noise itself.

more retro-goods like Phase Beam、Bubble、Nexus、Water Ripple、Grass can be found in `livewall.zip`

## Gallery

- [🧬c_life](./c_life.html)
- [🦢 Boids](./my_boids.htm)
- [💧 Liquid Glass](./c_glass.htm)

normal phys:

- [🔴balls_bezier](./demo/balls_bezier.htm)
- [🔴balls](./demo/balls.htm)... [`D2(x,y)(runs,turn)` func](https://editor.p5js.org/duangsuse/sketches/bwMmjzmX_), [🏀 Box 3D balls](https://codepen.io/duangsuz/pen/OPRzoBL)
- [🧷box_bezier](./demo/box_bezier.htm), [👍 Sandboxels](https://neal.fun/sandboxels/)
- [🖥cmatrix](./demo/cmatrix.htm), [👍 GUI+TUI Game 2048](./study/2048.htm)
- [🍥fft](./demo/fft.htm)


field:

- [⏳c_sand](./c_sand.html), [heat_death](./demo/field/heat_death.htm)
- [Water](./demo/field/water.htm) & [Our Holo Cosmos](./cosmos.html): [☯️](./study/yinyang.htm) [🍩](https://codepen.io/duangsuz/full/NPRmwYv)
- [hair_style](./demo/field/hair_style.htm), [👍 xyz Voxels(Minecraft)](https://mrdoob.com/#/129/voxels)
- [🏙van_gogh](./demo/van_gogh.htm)
- [🏙van_sfx](./demo/van_sfx.htm)

field(hard):

- [Clothes/Jelly 🎄 Blob Opera](https://www.youtube.com/watch?v=U4xafX1jR3c)
- [💦flow](./demo/field/flow.htm), [👍 WASM phys](https://oimo.io/works/water/), [🎄 XMas tree/z-index](./study/xmas.htm)
- [Clothes](https://codepen.io/duangsuz/pen/OPXrgVy?editors=1000) & [👕 ⛈Live2D/Piecewise-Affine trans](https://codepen.io/duangsuz/pen/QwEzgWG?editors=1000) drop a image, mid-click!

## See Also

* [📚 Good First DSP - 👍👍👍](./study/)
* [Inigo Quilez: Domain Warping](https://iquilezles.org/articles/warp/) - The foundational article on this technique.
* [The Book of Shaders: Fractal Brownian Motion](https://thebookofshaders.com/13/) - Visual guide to noise layering.
* [Curl Noise](https://www.overdraw.xyz/blog/tag/Curl+Noise) - An alternative approach for divergence-free fluid simulation.
* [CURL Noise function](https://observablehq.com/@hellonearthis/curl-noise-function)


## Algorithm

[Sorting Vis (Text mode)](https://duangsuse.github.io/mkey/making_reco/#sorts3) 玩法：tap【记录】，tap播放。

[Stalin & PNG bars sort](https://www.bilibili.com/video/BV1od9KB2ENd), [With sound](https://www.bilibili.com/video/BV1j6mVBJELL) + [🐧 Giegie](https://www.bilibili.com/video/BV1h1muB8EA4)

[🧷 Trie URL/Dict tokenizer & `basenc --help` impl](./study/trie.htm) _逆波兰带步骤计算器、Trie后缀压缩树、cubic-bezier()，这些拓扑排序（可视化）日用算法_

^ [tutour](./demo/)

## Game-Life

![Image](https://github.com/user-attachments/assets/83c6c838-f0da-4178-a9f2-e9707a521987)

![Image](https://github.com/user-attachments/assets/8937e60f-fc31-4fa1-8ebe-e7c907490992)

![Image](https://github.com/user-attachments/assets/97af50a3-72ad-410c-be30-18caf8e8153f)

## ASCII Art

> Cool [📊 PoV display, Holographic Volume](https://jsbin.com/sowamo/edit?output), Open Menu&"Apply", idShape=SDF Voxel

```py
import os, cv2, numpy as np
grays = [*" .-:=*+%@#"]
def ascii(img, wh=np.int32(os.popen('stty size', 'r').read().split()[::-1])  ):
  g=np.array(grays)
  a=cv2.cvtColor(cv2.resize(img, wh) , cv2.COLOR_BGR2GRAY)
  b=np.int8(np.interp(a, (0,256), (0,len(g)) ))
  return [*
    (''.join(y) for y in g[b] )
  ]

s=os.popen('ls ~/Pictures/Screenshots/*').read()[:-1]
@get_ipython().pt_app.key_bindings.add('c-k')
def f(ev):
  print(*ascii(cv2.imread(s)), sep="\n")

# Heart anim
import os, cv2, time
from numpy import*

Nm=array; mix=interp
sh=lambda s: os.popen(s).read()[:-1]

def heart(P, k=.8):
  t = mix(mod(iTime, k), [0, k], [0.3, k])  # Heartbeat
  r = (P[1] - power(abs(P[0]), t))**2 + P[0]**2 - 1.0  # Grayscale function
  return where(r < 0.3, mix(-r, [0, 0.3], [1, 4]), r)

grays = Nm([*" .-:", '\x1B[1;31m=\x1B[0m', *"+*%@#"])
GL=lambda w,h: Nm(meshgrid(linspace(0,1, w), linspace(1,0, h)))

def drawBW(g, wh=flip(int32(sh('stty size').split())), bg=grays):
  a=(1-g(GL(*wh)*5-2-sin(iTime)))*256 if callable(g) else cv2.cvtColor(cv2.resize(g, wh) , cv2.COLOR_BGR2GRAY)
  b=int8(mix(a, (0,256), (0,len(bg)-1) ))
  print('\x1B[0;0H', '\n'.join(''.join(y) for y in bg[b] ))

try: #.: ipy -i
  @get_ipython().pt_app.key_bindings.add('c-k')
  def cropBW(F5):
    g=cv2.imread(sh('zenity --file-selection'))
    drawBW(g)
except:
  for iTime in (x/100 for x in range(0, 600)): drawBW(heart); time.sleep(1/60)
```

### FBm

Logic projection of the shader's `main()` loop.

```python
import numpy as np
import matplotlib.pyplot as plt

def noise(p):
  # Grid coords & fractional offsets
  i = np.floor(p).astype(int); f = p - i
  f = f*f*(3 - 2*f) # Hermite smoothing
  
  # Hashing logic (Vectorized)
  def h(c):
    return (np.sin(c[...,0]*12.9898 + c[...,1]*78.233) * 43758.5453) % 1

  # Bilinear interpolation across the 4 corners
  a, b = h(i), h(i + [1,0])
  c, d = h(i + [0,1]), h(i + [1,1])
  
  return a + (b-a)*f[...,0] + (c-a)*f[...,1] + (a-b-c+d)*f[...,0]*f[...,1]

def fbm(p, octaves=6):
  v, amp = 0, .5
  for _ in range(octaves):
    v += amp * noise(p); p *= 2.; amp *= .5
  return v

def warp(p, t):
  # q = f(p + d1)
  q = np.stack([fbm(p + t), fbm(p + t + 5.2)], axis=-1)
  # r = f(p + 4q + d2)
  r = np.stack([fbm(p + 4*q + 1.7), fbm(p + 4*q + 9.2)], axis=-1)
  # output = f(p + 4r)
  return fbm(p + 4*r)

# Execute
res = 256
x = np.linspace(0, 5, res)
grid = np.stack(np.meshgrid(x, x), axis=-1)
img = warp(grid, 0.5)

plt.imshow(img, cmap='magma', origin='lower')
plt.axis('off')
plt.show()
```

* [Recursive FBM (numpy-cpu notebook)](https://colab.research.google.com/drive/1wL-9bIQgehS4ODqVP93FZsja-0g6CJlk?usp=sharing)
* [that .zip: Android Live Wallpapers](https://www.reddit.com/r/Android/comments/1jxajwi/here_are_all_the_stock_live_wallpapers_from/)
