# light and color

## Basic concepts

color of light arriving at camera depends on:

* spectral radiance of light falling on that patch
* spectral reflectance of the surface light is leaving

color perceived depends on:

* physics of light
* visual system receptors
* brain processing, envieronment 

##### white light

​	white light composed of about equal energy in all wavelengths of the visible spectrum

##### physics of light

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/f1/EM_spectrum.svg/2880px-EM_spectrum.svg.png" alt="img" style="zoom: 20%;" />

light can be described by its spectrum: the amount of energy emitted (per time unit) at each wavelength. 可见光是400 - 700nm



## color mixing

### additive color mixing vs. subtractive color mixing:

<img src="https://bkimg.cdn.bcebos.com/pic/a50f4bfbfbedab6460cafab1f836afc379311e02?x-bce-process=image/watermark,image_d2F0ZXIvYmFpa2UyNzI=,g_7,xp_5,yp_5" alt="img" style="zoom:18%;" />

#### 加法三原色

针对光源色，即来自发光物体

e.g. 红(600-700)+绿(500-600)=黄(500-700)

​	   红(600-700)+绿(500-600)+蓝(400-500)=白(400-700)

#### 减法三原色

颜色越混合越暗，应用于印刷，颜料，油墨等。颜料不发光，看到的颜色其实是没被吸收而被反射出来的光的颜色。所以颜料吸收的颜色就是我们看到颜料颜色的补色。我们知道表面色颜色决定于物体对光的吸收和反射：

​	如果物体分子吸收的能量不是可见光，或者说不吸收可见光，我们就只看到白色；

​	如果物体分子吸收的能量在可见光范围（380~780nm）里，我们就看到到的颜色就是被吸收的波长颜色的补色。

e.g. 黄(500-700)=白(400-700)-蓝(400-500)=红(600-700)+绿(500-600)



## Color matching

被匹配的颜色和要匹配的颜色并排放置，不断在三原色上面做加法/减法直至两者相同



## Color space

### RGB

* single wavelength primaries
* good for devices, but not for perception

### CIE XYZ color space

* here, Y value approximates brightness, Z is quasi-equal to blue, X is a mix of response curves chosen to be nonnegative

* usually projected to display:
  $$
  (x,y)=(X/(X+Y+Z),Y/(X+Y+Z))
  $$

### Uniform color space (Lab)

* due to the limitation of CIE XYZ color space, which is not a uniform color space, so magnitude of differences in coordinates are poor indicator of color "distance".
* thus, attempt to correct the limitation by remapping color space so that just-noticeable differences are contained by circles -> distances more perceptually meaningful

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gh3rb24srfj31cu0ikq5p.jpg" style="zoom:40%;" />

### HSV color space 

* H,S,V -> Hue, Saturation, Value
* it is nonlinear representation, that reflects topology of colors by coding hue as an angle

