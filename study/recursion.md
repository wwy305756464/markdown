# 递归

## while循环 vs. 递归

​		假设我们要在盒子里面找钥匙，但是盒子套盒子，那么如何找到呢？

1. 用循环的思想找

   <img src="C:\Users\Wenyue Wang\AppData\Roaming\Typora\typora-user-images\image-20200302090831471.png" alt="image-20200302090831471" style="zoom: 67%;" />

   写成代码即为：

   ```python
   def look_for_key(main_box):
   	pile = main_box.make_a_pile_to_look_through()
   	while pile is not empty:
   		box = pile.grab_a_box()
   		for item in box:
   			if item.is_a_box:
   				pile.append(item)
   			elif item.is_a_key:
   				print("found the key!")
   ```

   

2. 用递归的思想找

<img src="C:\Users\Wenyue Wang\AppData\Roaming\Typora\typora-user-images\image-20200302091527298.png" alt="image-20200302091527298" style="zoom:67%;" />
写成代码即为：
```python
def look_for_key(box):
	for item in box:
		if item.is_a_box():
			look_for_key(item)
		elif item.is_a_key():
			print("found the key!")
```



## 条件

每个递归函数都有两部分，基线条件（base case）和递归条件（recursive case）。递归条件指的是函数调用自己，基线条件指函数不再调用自己。这样可以避免无限循环。

### Example: countdown

```python
def countdown(i):
	print(i)
	if i <= 0:            ## 基线条件
		return
	else:				  ## 递归条件
		countdown(i-1)    
```

```c++
int countdown(int i){
	cout << i;
	if ( i <= 0) {		  // 基线条件
		return i;
	}
	else {				  // 递归条件
		return countdown(i-1);
	}
}
```

<img src="C:\Users\Wenyue Wang\AppData\Roaming\Typora\typora-user-images\image-20200302114716345.png" alt="image-20200302114716345" style="zoom:67%;" />





