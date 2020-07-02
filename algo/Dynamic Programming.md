# 	动态规划

动态规划的工作原理是先解决子问题，再逐步解决大问题，核心是记住已经解决过的子问题的解

在做题时，我们需要先想怎么将问题分解成最底层的小问题。

## 动态规划算法的两种形式

动态规划的求解一般有两种形式：自顶向下的备忘录法，和自底向上的方法。

### 斐波那契数列例子

$$
Fibonacci(n)=0; 	\ \ \ \ n=0\\
Fibonacci(n)=1; \ \ \ \  n=1\\
Fibonacci(n)=Fibonacci(n-1)+Fibonacci(n-2); \ \ \ \ n=others
$$

#### 递归解法

```c++
int Fib(int n) {
    if (n<=0) return 0;
    if (n == 1) return 1;
    return fib(n-1) + fib(n-2);
}
```

递归解法的问题在于有些量我们重复计算了，而且重复计算了好多次：

<img src="https://img-blog.csdn.net/20170715205029376?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzMwOTg3MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" alt="这里写图片描述" style="zoom: 50%;" />

在递归的基础上，如果我们把已经计算过的量记录下来，就是自顶向下的动态规划算法。

#### 自顶向下的备忘录法

```c++
int Fibonacci(int n) {
    if(n<=0) return n;
    vector<int> vec(n+1, -1); // 创建一个长度为n+1的数组，初赋值为-1.
	return fib(n, vec);
}

int fib(int n, vector<int> vec){
    if (vec[n] != -1) return vec[n];
    if (n<=2) vec[n] = 1;
    else vec[n] = fib(n-1, vec) + fib(n-2, vec);
    return vec[n];
}
```

#### 自底向上的动态规划

自底向上的方法用了动态规划的核心，先计算子问题，再由子问题计算父问题

```c++
int fib(int n){
    if (n<=0) return 0;
    vector<int> vec(n+1);
    vec[0] = 0;
    vec[1] = 1;
    for (int i=2; i<=n; ++i){
        vec[i] = vec[i-1] + vec[i-2];
    }
    return vec[n];
}
```



### 钢条切割

![这里写图片描述](https://img-blog.csdn.net/20170715221117648?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzMwOTg3MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](https://img-blog.csdn.net/20170715222316773?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzMwOTg3MA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)



## 背包问题

---

假设我们有一个可以装4lb物品的背包，现在我们有如下物品可以放到背包中，那么背包中可装的下的最大物品价值是多少？

音响，4lb，3000刀

电脑，3lb，2000刀

吉他，1lb，1500刀

---

1. 我们创立一个3*4的表格，将可选择的商品作为行，一共有3行，将背包容量作为列，一共有四列：
2. 先填第一排。第一个格子的意思是，在只有一个容量为1lb的背包且只有吉他可以放进去的情况下，最大的价值是什么？这时应该放入吉他，价值为1500。以此类推，第一行都应该填入1500。

|           | 1lb     | 2lb      | 3lb      | 4lb     |
| --------- | ------- | -------- | -------- | ------- |
| 吉他（G） | 1500，G | 1500， G | 1500， G | 1500，G |
| 音响（S） |         |          |          |         |
| 电脑（L） |         |          |          |         |

3. 第二排前三个的格子写法相同，因为在只有小于3lb的容积情况下，只能放进吉他，所以这三个格子都是1500。对于第四个格子，我们发现它有两种选择，一是放吉他，二是放音响。考虑到价值最大化，我们将音响放到这个格子里面。

|           | 1lb     | 2lb      | 3lb      | 4lb     |
| --------- | ------- | -------- | -------- | ------- |
| 吉他（G） | 1500，G | 1500， G | 1500， G | 1500，G |
| 音响（S） | 1500，G | 1500，G  | 1500，G  | 2000，S |
| 电脑（L） |         |          |          |         |

4. 对于最后一行，电脑需要3lb的空间，因此前两个格子只能放吉他。对于第三个格子，我们可以选择吉他或者电脑，因此选择电脑得到2000。对于最后一个格子，我们可以像可选择的集合里没有电脑一样，即不将电脑放进去，选择放入音响得到3000。也可以选择放入电脑，然后在余下的可用空间内选择价值最大的，即放入电脑（2000）和吉他（1500）。然后我们选择两者中更大价值的。

|           | 1lb     | 2lb      | 3lb      | 4lb        |
| --------- | ------- | -------- | -------- | ---------- |
| 吉他（G） | 1500，G | 1500， G | 1500， G | 1500，G    |
| 音响（S） | 1500，G | 1500，G  | 1500，G  | 3000，S    |
| 电脑（L） | 1500，G | 1500，G  | 2000，L  | 3500，G，L |

因此，在计算每个单元格的价值时，公式都相同：
$$
ceil[i][j]=max
\begin{cases}
ceil[i-1][j]\\
ceil[i-1][j-当前商品重量]+当前商品价值
\end{cases}
$$
这里第一种情况就是没有选择当前物品，第二种情况是选择了当前物品，然后加上余下容量可容纳的最大价值

如果再加入一种商品，我们不用重新来过，只需要再加一行进行计算。



## 最长公共子串 (Longest Common Substring)

---

对于两个字符串 str1 和 str2，求这两个字符串中最长公共子串长度

​		str1 = acbcbcef

​		str2 = abcbced

​		--> 最长公共子串为：bcbce，长度为5

---

为了比较acbcbcef和abcbced的公共子串，我们必须先知道acbcbce和abcbce的公共子串，以此类推，可以得到最小的问题就是从 str1 = a, str2 = a 开始比较，之后用动态规划得到最后的结果

1. 把两个字符串分别以行和列组成一个二维i而矩阵
2. 比较二维矩阵每个点所对应的行列字符是否相等，相等的话设置为1，不同设置为0
3. 通过查找处值为1的最长对角线就能找到最长公共子串

$$
ceil[i][j]=
\begin{cases}
0 & x_i \neq y_j\\\\
1 & x_i = y_j
\end{cases}
$$

<img src="https://img-blog.csdn.net/20140901133948015?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMDM5NzM2OQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" alt="img" style="zoom:150%;" />

这样在我们遍历过一遍两个字符串之后，仍要需要最长的对角线，所以可以进行进一步优化：
$$
ceil[i][j]=
\begin{cases}
0 & x_i \neq y_j\\\\
ceil[i-1][j-1]+1 & x_i = y_j
\end{cases}
$$
<img src="https://img-blog.csdn.net/20140901134800948?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMDM5NzM2OQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" alt="img" style="zoom:150%;" />