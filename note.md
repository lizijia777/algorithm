## 由数据范围反推算法复杂度以及算法内容

![image-20230710152832359](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230710152832359.png)

## 常见思考方向

![image-20230710152956218](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230710152956218.png)

## 排序+双指针

### Leetcode-18 四数之和

属于n数之和类型，实际就是比三数之和多了一层循环后面两个数用双指针。

难点照样是去重，一个是两层循环的时候判断**i > 0 && nums[i] == nums[i - 1]**去重；
另一部分是双指针循环时，得到答案后去重

```
  					while(l<r&&nums[l]==nums[l+1])
                    {
                         l++;
                    }
                    while(l<r&&nums[r]==nums[r-1])
                    {
                         r--;
                    }   
```



### Leetcode-15 三数之和

![image-20230710153205156](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230710153205156.png)

本题的难点在于如何去除重复解。对于重复元素i：跳过，避免重复解；对于左右指针L和R，执行循环，判断左界和右界是否和下一位置重复，去除重复解。并同时将 L*,*R移到下一位置，寻找新的解；

### Leetcode-16 最接近的三数之和

![image-20230710153651400](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230710153651400.png)

![image-20230710154014435](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230710154014435.png)

### Leetcode-167 两数之和II - 输入有序数组

![image-20230710154110141](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230710154110141.png)

最简单的双指针，三个数的双指针还需要遍历i；

## 贪心

### 区间覆盖

![QQ图片20201026093918.jpg](https://cdn.acwing.com/media/article/image/2020/10/26/652_1a48e6a417-QQ%E5%9B%BE%E7%89%8720201026093918.jpg)区间覆盖模板题 贪心思想：左端点排序 然后在能保证覆盖整个区间的情况找最大的右端点区间(使用双指针查找)

1.将所有区间按照左端点从小到大进行排序

2.从前往后枚举每个区间，在所有能覆盖start的区间中，选择右端点的最大区间，然后将start更新成右端点的最大值

这一步用到了**贪心决策**

#### AcWing-907 区间覆盖(模板示例题)

```
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 100010;

int n;

//c++在函数后加const的意义(一般只出现在类的成员函数定义中)
我们定义的类的成员函数中，常常有一些成员函数不改变类的数据成员，也就是说，这些函数是"只读"函数，而有一些函数要修改类数据成员的值。如果把不改变数据成员的函数都加上const关键字进行标识，显然，可提高程序的可读性。其实，它还能提高程序的可靠性，已定义成const的成员函数，一旦企图修改数据成员的值，则编译器按错误处理。

// 加const是为了保护数据，同时提高代码可读性
struct Range //区间排序，按左端点从大到小排序
{
    int l, r;
    // // 前面的const表示不能修改引用的值 后面的const表示不能修改类的数据成员
    bool operator< (const Range &W)const //const Range &W 其实就是用Range类型定义了一个W
										   这跟你平时用 int W是一个道理，只不过这里的Range是一个类，所以最好加上引用&以及const
    {
        return l < W.l;
    }
}range[N];

int main()
{
    int st, ed;
    scanf("%d%d", &st, &ed);
    scanf("%d", &n);
    for (int i = 0; i < n; i ++ )
    {
        int l, r;
        scanf("%d%d", &l, &r);
        range[i] = {l, r};
    }
    sort(range, range + n);
    int res = 0;
    bool success = false;
    for (int i = 0; i < n; i ++ )
    {
        int j = i, r = -2e9;
        while (j < n && range[j].l <= st)
        {
            r = max(r, range[j].r);
            j ++ ;
        }
        if (r < st)
        {
            res = -1;
            break;
        }
        res ++ ;
        if (r >= ed)
        {
            success = true;
            break;
        }
        st = r;
        i = j - 1; 
    }

    if (!success) res = -1;
    printf("%d\n", res);
    return 0;
}

```



#### Codefun2000-P1294 华为-河流水质监测

区间覆盖模板题

## 动态规划DP

![图片4.jpg](https://cdn.acwing.com/media/article/image/2020/03/25/13039_154f0d0e6e-%E5%9B%BE%E7%89%874.jpg)



### 数字三角形模型

#### AcWing-898 数字三角形

**思路：数字三角形模板**

**属性：max**

**边界条件：当j = 0时不选也是一种方案**

**集合：从(i,j)到最后一层的所有方案**

**状态计算：**![image-20230713110202681](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230713110202681.png)

#### LeetCode-931 下降路径最小和

**思路：数字三角形模板**

#### AcWing-1015 摘花生

**思路：数字三角形模板**

#### 洛谷-P2774 方格取数问题

**思路：同AcWing-275 传纸条**

#### AcWing-275 传纸条（方格取数变形）

**思路：首先考虑路径有交集该如何处理。可以发现交集中的格子一定在每条路径的相同步数处。因此可以让两个人同时从起点出发，每次同时走一步，这样路径中相交的格子一定在同一步内。**
***关于如何解决不能经过重复格子的问题?***
*我们先给定一个结论： 方格取数 dp模型的最优方案可以是不经过重复格子的*

*证明：*

*情况一：最优解的两条路线是相互交叉经过的*

*<img src="https://cdn.acwing.com/media/article/image/2021/05/28/55909_535aaa60bf-IMG_41A02C1923F3-1.jpeg" alt="IMG_41A02C1923F3-1.jpeg" style="zoom:25%;" />*

*则我们可以对交叉出来的部分进行路线交换，如下图的操作:*

*<img src="https://cdn.acwing.com/media/article/image/2021/05/28/55909_f4f7c194bf-IMG_D04B2FA5BFB0-1.jpeg" alt="IMG_D04B2FA5BFB0-1.jpeg" style="zoom:25%;" />*

*于是，我们可以发现，所有的交叉路线都会映射成一种一条路线只在下方走，一条路线只在上方走的不交叉路线*

*因此我们只需集中解决情况二即可*

*情况二：最优解的两条路线不交叉，但在某些点有重合。*

*<img src="https://cdn.acwing.com/media/article/image/2021/05/28/55909_8122f3b1bf-IMG_91EF0191824B-1.jpeg" alt="IMG_91EF0191824B-1.jpeg" style="zoom:25%;" />*

*由于方格取数，对于走到相同格子时，只会累加一次格子的价值*

*于是我们可以使用贪心中的微调法来进行这部分的证明*

*对于重合的格子，我们必然可以在两条路线中找到额外的一条或两条路线，使得新的路线不发生重合*

*具体参照下图：<img src="https://cdn.acwing.com/media/article/image/2021/05/28/55909_f3349557bf-IMG_6202C63C2DFE-1.jpeg" alt="IMG_6202C63C2DFE-1.jpeg" style="zoom:25%;" />*

*由于原路线是最优解，则必然 wA=wB=0*

*，否则最优解路线必然是经过 A 或 B 的因此，我们可以通过微调其中的一条路线，使之不经过重合点 C，同时路线的总价值没有减少。*

*得证：最优解路线可以是不经过重复路线的 （部分证明方法参照了第一赞的题解了）*

**属性：max**

**边界条件：![image-20230713163004846](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230713163004846.png)**

**集合：<img src="https://cdn.acwing.com/media/article/image/2021/05/27/55909_2df58fe8be-IMG_623B17197D17-1.jpeg" alt="IMG_623B17197D17-1.jpeg" style="zoom:25%;" />**

**状态计算：**![image-20230713160734547](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230713160734547.png)

### 背包问题

#### *AcWing-900 整数划分* 

**思路：把1,2,3, … n分别看做n个物体的体积，这n个物体均无使用次数限制，问恰好能装满总体积为n的背包的总方案数（完全背包问题变形）**

**属性：max**

**边界条件：当j = 0时不选也是一种方案**

**集合：从1~i中选，体积恰好为j的方案数量**

**状态计算：**
$$
因为f[i][j] =  f[i-1][j] + f[i-1][j-i] + f[i-1][j-i*2] + …… ;\\
   和f[i][j-1] =  f[i-1][j-i] + f[i-1][j-i*2] + ……  ;\\
所以 f[i][j] = f[i-1][j] + f[i][j-1];
$$

### 状态机 DP

#### AcWing-1055 股票买卖II

**思路：**

**属性:max**

**边界条件：第0天的时候不可能持股；**

**集合：考虑前i天的股市，第i天手中持股状态为j（j=0,未持股；j=1,持股）的方案**

**状态计算**：<img src="C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230711151008630.png" alt="image-20230711151008630"  />

#### AcWing-1911 最大子序列交替和

与股票买卖类似

### 最长上升子序列模型

#### AcWing-895 最长上升子序列

**思路：**

**属性:max**

**边界条件：**因为每一个数以自己结尾的最长上升子序列的长度为 1 ，所以初始 f[i]=1

**集合：** 表示从第一个数开始算，以第 i个数结尾的最长上升子序列

**状态计算：**![image-20230712111539316](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230712111539316.png)

#### AcWing-482 合唱队形

**思路：![image-20230712111031924](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230712111031924.png)**

**属性:min**

**边界条件：**因为每一个数以自己结尾的最长上升子序列的长度为 1 ，所以初始 g[i] = f[i] = 1

**集合：**表示从第一个数开始算，以第 i个数结尾的最长上升子序列和 从后往前算，从最后一个数开始算，以第i个数结尾的最长上升子序列

**状态计算：**

#### *AcWing-896 最长上升子序列 II*

**思路：首先数组a中存输入的数（原本的数），开辟一个数组f用来存结果，最终数组f的长度就是最终的答案；假如数组f现在存了数，当到了数组a的第i个位置时，首先判断a[i] > f[cnt] ？ 若是大于则直接将这个数添加到数组f中，即f[++cnt] = a[i];这个操作时显然的。**
**当a[i] <= f[cnt] 的时,我们就用a[i]去替代数组f中的第一个大于等于a[i]的数，因为在整个过程中我们维护的数组f 是一个递增的数组，所以我们可以用二分查找在 logn 的时间复杂的的情况下直接找到对应的位置，然后替换，即f[l] = a[i]。**

**我们用a[i]去替代f[i]的含义是：以a[i]为最后一个数的严格单调递增序列,这个序列中数的个数为l个。**

**这样当我们遍历完整个数组a后就可以得到最终的结果。**

**属性:max**

**边界条件：**二分查找，当**查找第一个大于等于a[i]**时：

```
  while(l <= r) {
        int mid = l + r >> 1;
        if(f[mid] >= x) r = mid-1; //注意等号
        else l = mid + 1;
    }

    return l;
```

**集合：** 

**状态计算：** 时间复杂度 O(nlogn)