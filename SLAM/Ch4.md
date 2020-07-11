# 李群与李代数

​		SLAM中我们需要解决“什么样的相机位姿最符合当前观测数据“这样的问题，很典型的一种解法就是将其构建成一个优化问题，求解最优的$R, t$，使得误差最小

## 为什么引入李群与李代数

我们已经知道了旋转矩阵构成特殊正交群 SO(3)，变换矩阵构成了特殊欧式群 SE(3)。
$$
SO(n)=\{ R\in R^{n\times n} | RR^T=1, det(R)=1 \} \\ \\
SE(3)=\bigg\{ T=  \begin{bmatrix} R & t \\ 0^T & 1 \end{bmatrix} \in R^{4\times 4} | R\in SO(3), t\in R^3 \bigg\}
$$
我们发现这两个矩阵对加法是不封闭的但是对乘法是封闭的，即：
$$
R_1+R_2\notin SO(3), \space T_1+T_2\notin SE(3) \\
R_1R_2\in SO(3),\space T_1T_2\in SE(3)
$$
假设在某个时刻，我们预测机器人的位姿是T，它观测到了在一个惯性坐标系下的点P而产生了一个观测数据z，它是该店在相机坐标系下的坐标，所以我们可以得到：
$$
z=Tp+w
$$
我们知道这里的w是观测噪声，也正是因为这个噪声的存在，z不能直接满足 $z=Tp$. 这里所产生的误差为：
$$
e=z-Tp
$$
那么假设我们这里一共有N个观测值，就有N个这样的式子，N个这样的误差 e，那么机器人的位姿估计就转变为群找下一个最优的T，使得整体的误差最小化：
$$
\min _T J(T)=\sum^N_{i=1} || z_i-Tp_i ||^2_2
$$
这里我们如果想直接求解这个T是十分复杂的，所以我们常常先给定一个初始值 $T_0$ ，然后不断地对其进行迭代更新，这个过程中需要用到导数。从导数的定义我们得知，需要用到加法。但是我们知道这两个矩阵对于加法是不封闭的，那么我们需要引入李群与李代数来进行求解。

## 李群与李代数基础

### 群（Group）

#### 定义

群是一种集合加上一种运算的代数结构。我们把集合记作A，运算记作 $\cdot$ 。那么群可以记作 $G=(A,\cdot)$。群要求这个运算满足以下几个条件：

1. 封闭性：$\forall a_1, a_2\in A, \space a_1 \cdot a_2 \in A$
2. 结合律：$\forall a_1,a_2,a_3\in A, (a_1\cdot a_2)\cdot a_3=a_1 \cdot (a_2 \cdot a_3)$
3. 幺元：$\exists a_0\in A,  \space \space \text{s.t.} \space \space \forall a\in A, \space \space a_0 \cdot a = a \cdot a_0 = a$
4. 逆：$\forall a \in A, \space \space \exist a^{-1} \in A, \space \space \text{s.t.} \space \space a\cdot a^{-1}=a_0$

这里我们可以发现旋转矩阵和矩阵乘法可以构成群，变换矩阵和矩阵乘法也可以构成群。

#### 常见的群

- 一般线性群 GL(n) ： n*n的可逆矩阵，它们对矩阵乘法成群
- 特殊正交群 SO(n) ：旋转矩阵群，一般 n=2, 3 最为常见
- 特殊欧式群 SE(n) ：n维欧式变换，一般 n=2, 3 最为常见

### 李群（Lie Group）

李群指具有连续（光滑）性质的群。像整数群那样离散的群是没有连续性质的，不是李群。上面我们提到的SO(3)和SE(3)在实数空间上是连续的，我们可以直观地想象一个刚体能够连续地在空间中运动，所以是李群。

### 李代数 （Lie Algebraic)

对于旋转矩阵我们得知：
$$
R(t)R(t)^T=I
$$
两边同时对时间求导，我们可以整理得：
$$
\dot{R}(t)R(t)^T=-(\dot{R}(t)R(t)^T)^T
$$
这里我们可以发现，$\dot{R}(t)R(t)^T$是一个反对称矩阵。

我们引入两个符号 $\wedge$ 和 $\vee$，这里$\wedge$ 将一个向量变成反对称矩阵，$\vee$用来找到一个反对称矩阵唯一对应的向量：
$$
\dot{R}(t)R(t)^T = \phi(t)^{\wedge}
$$
两边同乘$R(t)$，我们可以发现：
$$
\dot{R}(t)=\phi(t)^\wedge R(t)=\begin{bmatrix} 
0 & -\phi_3 & \phi_2 \\
\phi_3 & 0 & -\phi_1 \\
-\phi_2 & \phi_1 & 0
\end{bmatrix}R(t)
$$
这里我们得到这样的结论：每对旋转矩阵求一次倒数，只需左乘一个$\phi(t)^\wedge$矩阵

如果用泰勒展开，我们可以发现：
$$
R(t)=I+\phi(t_0)^\wedge(t)
$$
所以，$\phi$反映了R的导数性质，所以称它在SO(3)原点附近的正切空间（tangent space）上。

如果将 $t_0$ 附近设 $\phi$ 保持为常数 $\phi(t_0)=\phi_0$. 那么我们可以得到：
$$
R(t)=\exp(\phi_0 ^{\wedge}t)
$$
说明了在原点附近，旋转矩阵可以由这个式子计算出来。即旋转矩阵 R 与另一个反对称矩阵 $\phi_0 ^{\wedge}t$ 通过指数关系发生了联系。即 R 和 $\phi$ 是一个质数映射关系

每个李群都有对应的李代数，李代数描述了李群的局部性质，单位元附近的正切空间。

#### 李代数的定义

李代数由一个集合 V，一个数域 F 和一个二元运算符 [ , ] （李括号）组成。如果它们满足以下几条性质，则称（V, F, [ , ]）为一个李代数，记作 g。

1. 封闭性： $\forall X,Y\in V, [X,Y]\in V$

2. 双线性：$\forall X,Y,Z \in V, \space a,b\in F, then:$

   ​								$[aX+bY, Z] = a[X,Z]+b[Y,Z], [Z, aX+bY] = a[Z,X]+b[Z,Y]$

3. 自反性（自己与自己的运算为0）：$\forall X\in V, [X,X]=0$ 

4. 雅可比等价：$\forall X,Y,Z \in V, [X,[Y,Z]] + [Z,[X,Y]] + [Y,[Z,X]]=0$

李代数对应李群的正切空间，它描述了李群局部的导数，也可以简单理解为李代数对应了李群的导数

#### 李代数 so(3) 和 se(3)

##### so(3)

SO(3)对应的李代数是定义在 $R^3$ 上的向量，记作 $\phi$。根据上面的推导可知每个$\phi$都可以生成一个反对称矩阵：
$$
\Phi = \phi ^{\wedge} =\begin{bmatrix} 
0 & -\phi_3 & \phi_2 \\
\phi_3 & 0 & -\phi_1 \\
-\phi_2 & \phi_1 & 0
\end{bmatrix}\in R^{3\times 3}
$$
这里两个向量 $\phi_1, \phi_2$的李括号是：
$$
[\phi_1, \phi_2]=(\Phi_1 \Phi_2-\Phi_2 \Phi_1)^{\vee}
$$
所以我们可以将SO(3)的李代数写成：
$$
so(3)=\{\phi\in R^3, \Phi=\phi^{\wedge}\in R^{3\times 3} \}
$$
我们发现so(3)描述了一个由三维向量组成的集合，每个向量对应一个反对称矩阵，可以用于表达旋转矩阵的导数
$$
R=\exp(\phi^\wedge)
$$

##### se(3)

SE(3)的李代数se(3)可以写为：
$$
se(3)=\{ \zeta=\begin{bmatrix}  \rho \\ \phi
\end{bmatrix} \in R^6, \rho \in R^3,  \phi \in so(3), \zeta ^\wedge=\begin{bmatrix}
\phi^\wedge && \rho \\ 0^T && 0
\end{bmatrix} \in R^{4\times 4}
\}
$$
这里的$\zeta$代表了每个se(3)的元素，是一个六维向量，前三维是平移 $\rho$，后三维是旋转 $\phi$，即so(3)的元素。有一点需要注意的是，这里 $\wedge$ 不代表反对称，而是将一个六维向量转换为四维矩阵。简单来说，我们可以将se(3)理解为”由一个平移加上一个so(3)元素构成的向量。
$$
\zeta^\wedge=\begin{bmatrix} \phi ^\wedge && \rho \\ 0^T && 0
\end{bmatrix} \in R^{4 \times 4}
$$
se(3)的李括号是：
$$
[\zeta_1,\zeta_2]=(\zeta_1^\wedge\zeta_2^\wedge-\zeta_2^\wedge\zeta_1^\wedge)^\vee
$$



## 指数与对数映射

![image-20200711142634248](C:\Users\Wenyue Wang\AppData\Roaming\Typora\typora-user-images\image-20200711142634248.png)



## 李代数求导与扰动模型

