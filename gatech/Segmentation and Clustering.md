# Segmentation and Clustering

## grouping in vision

### goals:

* gather features that belong together
* obtain an intermediate representation that compactly describes key image or video parts



## top down vs. bottom up segmentation

top down: pixels belong together because they are from the same object

bottom up: pixel belong together because they look similar 

### goals of segmentation:
* separate image into coherent "objects"
* group together similar-looking pixels for efficiently of further processing

### example

<img src="C:\Users\Wenyue Wang\AppData\Roaming\Typora\typora-user-images\image-20200304124934076.png" alt="image-20200304124934076" style="zoom:36%;" />

Based on this image, we would like to divide the image into three groups (clusters). To do it, we use the intensities to divide. We could label every pixel in the images according to which of these primary intensities it is, then get the image intensities' histogram as: 

<img src="C:\Users\Wenyue Wang\AppData\Roaming\Typora\typora-user-images\image-20200304125200806.png" alt="image-20200304125200806" style="zoom:30%;" />

Thus, we can see the image has clearly been divided into three groups: black, gray, white.

But, what is the image is not so clear, like with some noise?

<img src="C:\Users\Wenyue Wang\AppData\Roaming\Typora\typora-user-images\image-20200304125402413.png" alt="image-20200304125402413" style="zoom:30%;" />

According to the noise case, we need **<font color=red >cluster</font>** to determine the three main intensities to define the group.




## bottom-up segmentation via clustering

#### algorithms:

* k-means 
* graph-based: normalized cuts



## K-means Clustering

#### basic idea: 

Randomly initialize the k cluster centers, allocate points to groups by assigning each to its closest center. Then based on group membership, update the center by computing the mean per group. Iterate this process.

1. randomly initialize the cluster centers c1, c2, ..., ck

2. given cluster centers, determine points in each cluster

   ​	for each point p, find the closest ci, put p into that cluster i

3. given points in each cluster, solve for ci

   ​	set ci to be the mean of points in cluster i

4. if ci have changed, repeat step 2

#### properties:

* will always converge to some solution
* can be a "local minimum"
  * does not always find the global minimum of objective function: 

$$
\sum_{clusters\ i}\sum_{points\ p\ in\ cluster\ i}||p-c_i||^2
$$

#### pros:

* simple, fast to compute
* converges to local minimum of within-cluster squared error

#### cons:

* setting k? 
  * sometimes hard to define how many clusters
* sensitive to initial centers
  * distance function can be changed
* sensitive to outliers

<img src="C:\Users\Wenyue Wang\AppData\Roaming\Typora\typora-user-images\image-20200304133243601.png" alt="image-20200304133243601" style="zoom:63%;" />

* detects spherical clusters
  * shape sensitive

<img src="C:\Users\Wenyue Wang\AppData\Roaming\Typora\typora-user-images\image-20200304133308320.png" alt="image-20200304133308320" style="zoom:67%;" />

* assuming means can be computed

#### feature space choice

* intensity (1D)

<img src="C:\Users\Wenyue Wang\AppData\Roaming\Typora\typora-user-images\image-20200304133701386.png" alt="image-20200304133701386" style="zoom:33%;" />

* color similarity (3D)

<img src="C:\Users\Wenyue Wang\AppData\Roaming\Typora\typora-user-images\image-20200304133741980.png" alt="image-20200304133741980" style="zoom:43%;" />

* intensity + position (3D)

<img src="C:\Users\Wenyue Wang\AppData\Roaming\Typora\typora-user-images\image-20200304133822011.png" alt="image-20200304133822011" style="zoom:43%;" />

* texture (more than 3D...)

<img src="C:\Users\Wenyue Wang\AppData\Roaming\Typora\typora-user-images\image-20200304133858173.png" alt="image-20200304133858173" style="zoom:43%;" />



## Graph Based Clustering

