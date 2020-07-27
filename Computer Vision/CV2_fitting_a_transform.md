# fitting a transform

## 2D transformation

Alignment problem: fit the parameters of some transformation according to a set of matching feature pairs

### parametric warping:

A point on original pic will transform to another point through a transformation matrix T, that is, transformation matrix  $T$  is a coordinate-changing machine:
$$
p(x',y')=T(p(x,y))=Mp(x,y)
$$
we need to represent transformation matrix as $M$

#### scaling

Scaling: Multiplying each of the coordinate's components by a scalar

Uniform scaling: scalar is same for all components

Non-uniform scaling: scalars are different
$$
x'=ax;\space\space y'=by \\
\begin{bmatrix} x' \\y' \end{bmatrix} = \begin{bmatrix} a && 0 \\ 0 && b\end{bmatrix}=
\begin{bmatrix} x \\ y \end{bmatrix}
$$
Only linear 2D transformations can be represented with a $2\times2$ matrix, like scale, rotation, shear and mirror

### Homogeneous coordinates

To deal with non-pure translation, we need to introduce homogeneous coordinates: 

##### regular to homogeneous:

$$
(x,y) \to \begin{bmatrix} x\\y\\1\end{bmatrix}
$$

##### homogenous to regular:

$$
\begin{bmatrix} x\\y\\w\end{bmatrix} \to (x/w, y.w)
$$

then, we can use homogeneous coordinates to represent translation:
$$
\begin{bmatrix} x'\\y'\\1\end{bmatrix} = \begin{bmatrix}1&&0&&t_x\\0&&1&&t_y\\0&&0&&1 \end{bmatrix} \begin{bmatrix} x\\y\\1\end{bmatrix}= \begin{bmatrix} x+t_x\\y+t_y\\1 \end{bmatrix}
$$


### 2D Affine Transformations

$$
\begin{bmatrix} x'\\y'\\w'\end{bmatrix} = \begin{bmatrix}a&&b&&c\\d&&e&&f\\0&&0&&1 \end{bmatrix} \begin{bmatrix} x\\y\\w\end{bmatrix}
$$

Affline transformations are combinations of linear transformation and translation, its parallel lines remain parallel

### Projective transformation

$$
\begin{bmatrix} x'\\y'\\w'\end{bmatrix} = \begin{bmatrix}a&&b&&c\\d&&e&&f\\g&&h&&i \end{bmatrix} \begin{bmatrix} x\\y\\w\end{bmatrix}
$$

Projective transformations are combinations of affine transformations and projective warps, where parallel lines do not necessarily remain parallel. 



## Affine fit

### Alignment problem

in alignment, we will fit the parameters of some transformation according to a set of matching feature pairs (correspondences)

<img src="https://tva1.sinaimg.cn/large/007S8ZIlgy1gh5115wjoyj30r60gc0tr.jpg" style="zoom:33%;" />

我们已知很多个pair个点的correspondence，如何从中求得transformation matrix T？
$$
\begin{bmatrix} x_i' \\ y_i'\end{bmatrix} = \begin{bmatrix}m_1 &&m_2 \\ m_3 && m_4\end{bmatrix} \begin{bmatrix}x_i\\y_i \end{bmatrix} + \begin{bmatrix} t_1 \\ t_2\end{bmatrix}
$$
从上面的公式，已知的是$(x,y),(x',y')$ pair，我们想要求的是$m_1,m_2,m_3,m_4,t_1,t_2$, 因此我们可以写成：
$$
\begin{bmatrix} && && ... && && && \\ 
x_i && y_i && 0 && 0 && 1 && 0 \\
0 && 0 && x_i && y_i && 0 && 1 \\
 && && ... && && &&
\end{bmatrix} \begin{bmatrix} m_1\\m_2\\m_3\\m_4\\t_1\\t_2 \end{bmatrix} = 
\begin{bmatrix} ...\\ x_i' \\ y_i' \\ ... \end{bmatrix}
$$
这里一共有六个未知量，我们需要三个correspondence pair 来求这六个未知量



## RANSAC

在我们估计parameter的时候，有一些outlier point会破坏我们的准确性，所以需要排除这些点。

随机抽样一致（RANSAC）是一种通过使用观测到的数据点来估计数学模型参数的迭代方法。其中数据点包括inlier，outlier。outlier对模型的估计没有价值，因此该方法也可以叫做outlier检测方法。这是一种非确定性算法，因为它是在一定概率下得到一个合理的结果，当迭代次数增加，概率也会增加。

![img](https://pic3.zhimg.com/80/v2-1854713d206c6d9161379ff3fe30c8d9_1440w.jpg)

这里如果用least square，就会将outlier来包括进来做fitting。所以我们需要引入RANSAC

#### RANSAC loop

1. Randomly select a seed group of points on which to base transformation estimate (e.g. a group of matches)
2. compute transformation from seed group
3. find inliers to this transformation (look at all points)
4. if the number of inliers is sufficiently large, re-compute estimate of transformation on all of the inliers. 

总结来说，就是每次都随机选择两个点，这两个点构造一条直线。直线两边根据threshold构造一个区域，在这个区域中的点称为inlier，在这个区域外面的点叫做outlier。经过很多次的随机选点之后，保留inlier数量最多的那条线的parameter

#### pros and cons:

Pros:

* simple and general
* applicable to many different problems
* often works well in practice

Cons:

* lots of hyperparameters to tune
* doesn't work well of low inlier ratios, two many iterations, or can fail completely
* can't always get a good initialization of the model based on the minimum number of samples