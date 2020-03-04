# Introduction to Object Recognition

Harris if not alignment to axes?  find eigenvalue and eigenvector, and see which direction is the major one

## instance recognition

## recognition via alignment

### pros:

* effective when we are able to find reliable features within clutter
* great results for matching specific instances

### cons:

* scaling with number of models
* spatial verification as post-processing - not seamless, expensive for large-scale problems
* not suitable for category recognition 
  * category has multiple and distinct template, so it's hard to fit in a template for recognition 



## common recognition tasks

* image classification and tagging 
  * with tags, you can set figures into categories 
* object detection
* activity recognition
  * usually in video
* semantic segmentation
  * label every pixel and check which pixel represents which object
* instance segmentation 
  * instance segmentation would say this is someone's car, while general categorization would say this is a car



## Object Categorization

Given a small number of training images of a category, recognize a priori unknown instances of that category and assign the correct category label.

basic level categories: humans seem to be defined predominantly visually (e.g. cat, dog, cow...)



## Challenges

1. robustness 

   ​	camera can change, object can change position, objection can not focus on camera

   ​	a scene can have multiple objects

2. importance of context

   ​	a group of patches, can have many correspondences from multiple images

   ​	a hypothesis like where the thing may appear. E.g. for autonomous driving, vehicles are most likely to shown along a line

3. learning with minimal supervision



## Supervised Classification

given a collection of labeled examples, come up with a feature that will predict the labels of new examples. 

<img src="C:\Users\Wenyue Wang\AppData\Roaming\Typora\typora-user-images\image-20200303124609978.png" alt="image-20200303124609978" style="zoom:40%;" />

how good is some function we come up with to do the classification?

depend on: 

* mistakes made
* cost associated with the mistakes 

### cost of mistake

### expected loss (s)

### to minimize the total risk

decision boundary: if recognize as 4, will put on one side, if its 9, will be on the other side

L(4->4) = 0, L(9->9) = 0

simplest case: lost is symmetric, but in some case do not want symmetric, like spam...



bayer's rule

where does the prior come from?

​	from the image itself, or from the histogram of training data



