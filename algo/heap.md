# Heap

heap就像一个店员记住了所有商品所对应的价格，所以在顾客问某个商品的价格时，店员只需要O(1)的时间给出答案。最大的优点是把数据的储存和查找的时间大大降低到常数时间，代价是消耗较多的内存。

## 散射函数

* 将输入映射到数字
* 散列函数必须满足的一些要求：
  * 一致性，输入所映射的数字必须每次都保持一致
  * 不同的输入应该映射到不同的数字中（理想情况）

## 散列表(hash table)

hash table 是由散列函数和数组组成的数据结构，它使用散列函数来决定元素的存储位置，用数组来储存数据。散列表由键（key）和值（value）组成，将键映射到值。

### 应用案例

#### 创建

```c++
unordered_map<int, string> hash  // unordered_map<key, val> hash
hash[0] = "abc"
hash[1] = "bcd"
```

插入过程：

1. 得到key
2. 通过hash函数得到hash值
3. 得到index，一般是hash值对内存总量求模
4. 在内存内存放key和value

#### 查找

```c++
if (hash.find(1) != hash.end()){
		...
}
```

取值过程：

1. 得到key
2. 通过hash函数得到hash值
3. 得到index
4. 比较index对应的内部元素是否与key相等，比较所有的index，如果都不相等则没有找到
5. 去除相等记录的value

#### 防止重复

``` python
voted = {}   ## python中创建hash
def check_voter(name):
    if voted.get(name):  ## 检测value是否已经存在
        print("Kick them out!")
    else:
        voted[name] = True  ## 如果不存在，则将元素加入hash中
        print("Let them vote!")
```





