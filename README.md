# Nebula Remix

[Green Fog](https://duangsuse.github.io/NebulaRemix/?algo=A%3A+Cotton&color=Toxic+Bio&M=1&W=9.4&S=8&K=1.2&G=0.43&O=10&flu=3&b=0%2C0.1%2C0.05&f=0.3%2C0.8%2C0.2&h=0.9%2C1%2C0.8), [Try Fractals](https://duangsuse.github.io/NebulaRemix/mandel.html)

A WebGL reconstruction of the recursive domain warping technique found in Android's ["Magic Smoke" live wallpaper](https://live-wallpaper.fandom.com/wiki/Magic_Smoke).

It simulates fluid dynamics and cloud formations not through Navier-Stokes equations, but via iterated fractional Brownian motion (FBM), where the noise domain is distorted by the noise itself.

more retro-goods like Phase Beam、Bubble、Nexus、Water Ripple、Grass can be found in `livewall.zip`

## See Also

* [Inigo Quilez: Domain Warping](https://iquilezles.org/articles/warp/) - The foundational article on this technique.
* [The Book of Shaders: Fractal Brownian Motion](https://thebookofshaders.com/13/) - Visual guide to noise layering.
* [Curl Noise](https://www.overdraw.xyz/blog/tag/Curl+Noise) - An alternative approach for divergence-free fluid simulation.
* [CURL Noise function](https://observablehq.com/@hellonearthis/curl-noise-function)


## Algorithm

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
