# 相机与图像

## 相机模型

### 针孔相机模型

针孔相机模型描述了一束光通过针孔之后，在针孔背面投影成像

<img src="C:\Users\Wenyue Wang\AppData\Roaming\Typora\typora-user-images\image-20200711184452957.png" alt="image-20200711184452957" style="zoom: 50%;" />

#### 这里有如下几个坐标系/平面：

* 世界坐标系：P点在现实世界中的坐标为 $P:[X,Y,Z]$
* 相机坐标系：$O-x-y-z$ 为相机坐标系，一般 z 指向相机前方，x 轴向右，y 轴向下。O为光心，即针孔
* 物理成像平面：P 点通过针孔后在物理成像平面上为 P', 它的坐标为 $P':[X',Y',Z']$

* 像素平面：将其他平面以米为单位的坐标转换成二维像素坐标：$P':[u,v]^T$

  * 像素坐标系：

    * 一般原点 $o'$ 位于图像的左上角，$u$ 轴向右与 $x$ 轴平行，$v$ 轴向下与 $y$ 轴平行
    * 这里引入了一个缩放和原点的平移，在物理成像平面和像素平面之间
    * 像素坐标在 $u$ 轴上缩放了 $\alpha$ 倍，在 $v$ 轴上缩放了 $\beta$ 倍，原点平移了 $[c_x, c_y]^T$

    

#### 数学关系：

世界坐标系与物理成像平面：(这里忽视倒立成像)，$f$ 为焦距（物理成像平面到小孔的距离）
$$
\frac{Z}{f}=\frac{X}{X'}=\frac{Y}{Y'},\space \space \space X'=f\frac{X}{Z},\space \space \space Y'=f\frac{Y}{Z}
$$
物理成像平面与像素平面：
$$
\begin{cases}
u=\alpha X'+c_x=f_x\frac{X}{Z}+c_x\space \space \space \space ,  \space \space \space \space f_x=\alpha f\\
v=\beta Y'+c_y=f_y\frac{Y}{Z}+c_y\space \space \space \space ,  \space \space \space \space f_y=\beta f
\end{cases}
$$
​	写成矩阵形式：
$$

$$




