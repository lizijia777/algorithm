## 由数据范围反推算法复杂度以及算法内容

![image-20230717212638950](https://raw.githubusercontent.com/lizijia777/imageSet/main/note1/202307172126051.png)

## 常见思考方向

![image-20230721203637435](https://raw.githubusercontent.com/lizijia777/imageSet/main/note1/202307212037127.png)

## Utils

### 优先队列

```
priority_queue<Type, Container, Functional>
其中Type代表数据类型，Container代表容器类型，缺省状态为vector; Functional是比较方式，默认采用的是大顶堆(less<>)。
//升序队列  小顶堆 great 小到大
priority_queue <int,vector<int>,greater<int> > pri_que;
//降序队列  大顶堆 less  大到小 默认
priority_queue <int,vector<int>,less<int> > pri_que;
q.size();//返回q里元素个数
q.empty();//返回q是否为空，空则返回1，否则返回0
q.push(k);//在q的末尾插入k
q.pop();//删掉q的第一个元素
q.top();//返回q的第一个元素

```



### 构造二叉树

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
```



### 链式向前星

```
int h[N]，e[N], ne[N], idx;//邻接表存储图

void add(int a, int b){
	// a,b均为节点，idx为指针，h[a]来获取a的next指针，e[idx]存取节点b
    e[idx] = b, ne[idx] = h[a], h[a] = idx,idx++;
}

memset(h, -1, sizeof h);//初始化邻接矩阵
```

### 数组模拟队列

**### 数组实现原理**

首先定义一个大小为 N 的数组 q，变量 hh 和 tt 分别表示队头和队尾。这里的队尾是指队列中最后一个元素的位置，而不是最后一个元素的值。

向队尾插入一个数时，先将 tt 的值增加 1，然后将 x 赋值给 q[tt]。这里的 ++tt 表示先执行 tt+1 的操作，再将结果赋值给 tt。

从队头弹出一个数时，先将 hh 的值增加 1，表示弹出了一个元素。这里的 hh++ 表示先使用 hh，然后再执行 hh+1 的操作，并将结果赋值给 hh。

队头的值可以通过 q[hh] 获取。

判断队列是否为空，只需要判断 hh 是否小于等于 tt，如果 hh <= tt，则表示不为空。

```
// hh 表示队头，tt表示队尾
int q[N], hh = 0, tt = -1;

// 向队尾插入一个数
q[ ++ tt] = x;

// 从队头弹出一个数
hh ++ ;

// 队头的值
q[hh];

// 判断队列是否为空，如果 hh <= tt，则表示不为空
if (hh <= tt)
{
}
```

**## 循环队列**
同样定义一个大小为 N 的数组 q，变量 hh 和 tt 分别表示队头和队尾的后一个位置。这里的队尾仍然表示最后一个元素的位置，但是队尾需要增加的方式不同于普通队列，具体实现如下：

向队尾插入一个数时，先将 x 赋值给 q[tt]，然后将 tt 的值增加 1。如果 tt 的值等于 N，则将 tt 的值设为 0，实现队尾在数组中循环移动。

从队头弹出一个数时，先将 hh 的值增加 1，表示弹出了一个元素。如果 hh 的值等于 N，则将 hh 的值设为 0，实现队头在数组中循环移动。

队头的值可以通过 q[hh] 获取。

判断队列是否为空，只需要判断 hh 和 tt 是否相等，如果 hh != tt，则表示不为空。

```
// hh 表示队头，tt表示队尾的后一个位置
int q[N], hh = 0, tt = 0;

// 向队尾插入一个数
q[tt ++ ] = x;
if (tt == N) tt = 0;

// 从队头弹出一个数
hh ++ ;
if (hh == N) hh = 0;

// 队头的值
q[hh];

// 判断队列是否为空，如果hh != tt，则表示不为空
if (hh != tt)
{
}
```

### 二维vector排序（sort用法）

```
static bool cmp(const vector<int>& a,const vector<int>& b){return a.back()<b.back();}
sort(points.begin(),points.end(),cmp);

```



## 图论

#### LeetCode-1222 可以攻击国王的皇后

模拟：枚举可以攻击国王的 888 个方向，每个方向遍历到边界外或第一个皇后为止。

#### LeetCode-2596 检查骑士的巡视方案

模拟：枚举可以攻击国王的 888 个方向，每个方向遍历到边界外或第一个皇后为止。

### 拓扑排序

**经典模板**

首先记录各个点的入度

然后将入度为 0 的点放入队列

将队列里的点依次出队列，然后找出所有出队列这个点发出的边，删除边，同事边的另一侧的点的入度 -1。

如果所有点都进过队列，则可以拓扑排序，输出所有顶点。否则输出-1，代表不可以进行拓扑排序。

```
void topsort(){
    for(int i = 1; i <= n; i++){//遍历一遍顶点的入度。
        if(d[i] == 0)//如果入度为 0, 则可以入队列
            q[++tt] = i;
    }
    while(tt >= hh){//循环处理队列中点的
        int a = q[hh++];
        for(int i = h[a]; i != -1; i = ne[i]){//循环删除 a 发出的边
            int b = e[i];//a 有一条边指向b
            d[b]--;//删除边后，b的入度减1
            if(d[b] == 0)//如果b的入度减为 0,则 b 可以输出，入队列
                q[++tt] = b;
        }
    }
    if(tt == n - 1){//如果队列中的点的个数与图中点的个数相同，则可以进行拓扑排序
        for(int i = 0; i < n; i++){//队列中保存了所有入度为0的点，依次输出
            cout << q[i] << " ";
        }
    }
    else//如果队列中的点的个数与图中点的个数不相同，则可以进行拓扑排序
        cout << -1;//输出-1，代表错误
}
```

#### LeetCode-207 课程表

模板题

#### LeetCode-210 课程表Ⅱ

模板题

#### LeetCode-1462 课程表Ⅳ

拓扑模板+广度优先搜索

```
for (int j = 0; j < numCourses; ++j) {
	ispre[j][k] = ispre[j][k] || ispre[j][t];
}
```



## BFS(最短路径)

BFS（广度优先搜索） 广度优先搜索是一种用于搜索或遍历树或图的算法，其基本思路是从起始节点开始，依次遍历当前节点的所有**邻居节点**，然后再依次遍历邻居节点的所有邻居节点，直到遍历到目标节点或者遍历完所有节点。 BFS的实现方式可以**采用队列**来实现。下面是一个采用队列方式实现的BFS代码示例:

```cpp
void bfsTraversal(vector<vector<int>>& graph) {
    int n = graph.size();
    vector<int> visited(n, 0); // 初始化访问数组
    queue<int> q;
    for (int i = 0; i < n; i++) {
        if (!visited[i]) { // 如果当前节点未被访问
            q.push(i); // 将当前节点加入队列
            visited[i] = 1; // 标记当前节点已经被访问
            while (!q.empty()) { // 循环遍历队列中的节点
                int cur = q.front();
                q.pop();
                // 处理当前节点cur
                for (int j = 0; j < graph[cur].size(); j++) {
                    int next = graph[cur][j];
                    if (!visited[next]) { // 如果下一个节点未被访问
                        q.push(next); // 将下一个节点加入队列
                        visited[next] = 1; // 标记下一个节点已经被访问
                    }
                }
            }
        }
    }
}
```

### LeetCode-1654 到家的最少跳跃次数（hard）

https://leetcode.cn/problems/minimum-jumps-to-reach-home/solutions/2414842/dao-jia-de-zui-shao-tiao-yue-ci-shu-by-l-sza1/

## DFS(所有情况)

其中DFS是一种用于遍历或搜索树或图的算法，BFS则是一种用于搜索或遍历树或图的算法。全需要访问数组两种算法都有其自身的优点和缺点，应用于不同的场景中。 DFS（深度优先搜索） 深度优先搜索是一种用于**遍历**或**搜索树或图**的算法，其基本思路是从起始节点开始，沿着一条路径一直走到底，直到无法再走下去为止，然后回溯到上一个节点，继续走到另外一个路径，重复上述过程，直到遍历完所有节点。

DFS的实现方式可以采用**递归**或者**栈**来实现。下面是一个采用**递归方式**实现的DFS代码示例（C++）：

```cpp
void dfs(int cur, vector<int>& visited, vector<vector<int>>& graph) {
    visited[cur] = 1; // 标记当前节点已经被访问
    // 处理当前节点cur
    for (int i = 0; i < graph[cur].size(); i++) {
        int next = graph[cur][i];
        if (!visited[next]) { // 如果下一个节点未被访问
            dfs(next, visited, graph); // 继续访问下一个节点
        }
    }
}
void dfsTraversal(vector<vector<int>>& graph) {
    int n = graph.size();
    vector<int> visited(n, 0); // 初始化访问数组
    for (int i = 0; i < n; i++) {
        if (!visited[i]) { // 如果当前节点未被访问
            dfs(i, visited, graph); // 从当前节点开始进行深度优先遍历
        }
    }
}
```

## 二分

### 二分答案

       他的实现基础是二分查找操作，即在有序数据中通过对中值进行判断以减半操作区间达到分治或减治的效果；
       他的算法思想就是逆向验证，即先二分枚举可能的答案，然后对答案进行判断，直至找到满足条件的答案；
       他的核心难点就是判断函数的设计，即验证该答案是否能通过题目中给出的条件得到，可以说判断函数确定后二分答案就完成一半了。
最大最小的字眼时
这个题的通常做法是二分答案
一般二分答案会和贪心、动态规划、网络流等算法相结合
二分答案的特点是，答案具有单调性

二分问题有很明显的关键字如：最大值最小，最小值最大。
或者在数据上发现只有log级别才能接受时，我们都应该优先考虑二分。
二分问题的难点也不在二分，而是如何使用[贪心](https://so.csdn.net/so/search?q=贪心&spm=1001.2101.3001.7020)配合二分，贪心策略往往是难点所在。

#### Leetcode-2560 打家劫社Ⅳ



## 双指针

### Leetcode-11 盛最多水的容器

对O(n)的算法写一下自己的理解，一开始两个指针一个指向开头一个指向结尾，此时容器的底是最大的，接下来随着指针向内移动，会造成容器的底变小，在这种情况下想要让容器盛水变多，就只有在容器的高上下功夫。 那我们该如何决策哪个指针移动呢？我们能够发现不管是左指针向右移动一位，还是右指针向左移动一位，容器的底都是一样的，都比原来减少了 1。这种情况下我们想要让指针移动后的容器面积增大，就要使移动后的容器的高尽量大，所以我们选择指针所指的高较小的那个指针进行移动，这样我们就保留了容器较高的那条边，放弃了较小的那条边，以获得有更高的边的机会。

### **Leetcode-42 接雨水**

双指针的傻瓜理解：两边往中间找更大值，在找到更大的值之前由目前最高的板子决定盛多少水

1. 找出最高点
2. 分别从两边往最高点遍历：如果下一个数比当前数小，说明可以接到水

### 双指针+排序

#### Leetcode-18 四数之和

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



#### Leetcode-15 三数之和

![image-20230721203800005](https://raw.githubusercontent.com/lizijia777/imageSet/main/note1/202307212038554.png)

本题的难点在于如何去除重复解。对于重复元素i：跳过，避免重复解；对于左右指针L和R，执行循环，判断左界和右界是否和下一位置重复，去除重复解。并同时将 L*,*R移到下一位置，寻找新的解；

#### Leetcode-16 最接近的三数之和

![image-20230721203823872](https://raw.githubusercontent.com/lizijia777/imageSet/main/note1/202307212038950.png)

![image-20230710154014435](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230710154014435.png)

#### Leetcode-167 两数之和II - 输入有序数组

![](https://raw.githubusercontent.com/lizijia777/imageSet/main/note1/202307212038950.png)

最简单的双指针，三个数的双指针还需要遍历i；

## 贪心

### 区间问题

**（区间问题的贪心一般都是上来先按左端点或者右端点排序）**

#### AcWing-908 最大不相交区间数量

**最大不相交区间数==最少覆盖区间点数**

**为什么最大不相交区间数==最少覆盖区间点数呢？**

- 因为如果几个区间能被同一个点覆盖
- 说明他们相交了
- 所以有几个点就是有几个不相交区间

#### AcWing-905 区间选点

![image_19.png](https://cdn.acwing.com/media/article/image/2020/10/25/652_632882e016-image_19.png)

1. 将每个区间按照**右端点**从小到大进行排序

2. 从前往后枚举区间，end值初始化为无穷小

   - 如果本次区间不能覆盖掉上次区间的右端点， ed < range[i].l
   
   
      - 说明需要选择一个新的点， res ++ ; ed = range[i].r;
   
   
   
      - **如果本次区间可以覆盖掉上次区间的右端点，则进行下一轮循环**
   

------



1. 将每个区间按照**左端点**从小到大进行排序

2. 从前往后枚举区间，end值初始化为无穷小

   - 如果本次区间不能覆盖掉上次区间的右端点， ed < range[i].l


      - 说明需要选择一个新的点， res ++ ; ed = range[i].r;


      - **如果本次区间可以覆盖掉上次区间的右端点， st=min(st,range[i].r);**

#### AcWing-907 区间覆盖(模板示例题)



![image-20230721203925628](https://raw.githubusercontent.com/lizijia777/imageSet/main/note1/202307212039749.png)区间覆盖模板题 贪心思想：左端点排序 然后在能保证覆盖整个区间的情况找最大的右端点区间(使用双指针查找)

1.将所有区间按照左端点从小到大进行排序

2.从前往后枚举每个区间，在所有能覆盖start的区间中，选择右端点的最大区间，然后将start更新成右端点的最大值

这一步用到了**贪心决策**

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

#### AcWing-906 区间分组

从前往后枚举每个区间，判断此区间能否将其放到现有的组中

1. 如果一个区间的左端点比最小组的右端点要小，ranges[i].l<=heap.top() ， 就开一个新组 heap.push(range[i].r);

2. 如果一个区间的左端点比最小组的右端点要大，则放在该组， heap.pop(), heap.push(range[i].r);


​		每组去除右端点最小的区间，只保留一个右端点较大的区间，这样heap有多少区间，就有多少组。

![image-20230921210432293](https://raw.githubusercontent.com/lizijia777/imageSet/main/note1/202309212104370.png)

区间分组，在组内区间不相交的前提下，分成尽可能少的组。
而不是尽可能多的组，因为一个区间一组，就是尽可能多组的答案。
等效于把尽可能多的区间塞进同一组，要满足range[i].l > heap.top。
heap 存储的是每个组的最右的端点，由于是小根堆heap.top()是对应的最小的最右点。

那如果遇到，塞不进去的情况呢？
就是heap.top >= range[i].l, 当前区间的左端点比最小的右端点还要小，放到任何一组都会有相交部分。
那就需要新开一组，heap.push(range[i].r).

1. 把所有区间按照左端点从小到大排序***(因为左端点越小的越容易跟其他线段重叠，从不能找到可能的过程中，能够贪心的找到一个最小的可能性)***

2. 从前往后枚举每个区间，判断此区间能否将其放到现有的组中

3. heap有多少区间，就有多少组



#### Codefun2000-P1294 华为-河流水质监测

区间覆盖模板题

### LeetCode-1921 消灭怪物的最大数量

为了消灭尽可能多的怪物，我方需要坚持尽可能长的时间，因为我方每分钟都能消灭一个怪物。为了坚持更久，我方需要先消灭先来的怪物。因此，贪心的思路是将怪物的到达时间排序，先消灭到达时间早的怪物。我方的攻击时间序列是 [1,2,3,… ][1,2,3,\dots][1,2,3,…]，将我方的攻击时间序列和排序后的怪物到达时间依次进行比较，当第一次出现到达时间小于等于攻击时间，即表示怪物到达城市，我方会输掉游戏。在比较时，因为我方的攻击时间为整数，因此可以将怪物到达时间向上取整，可以达到避免浮点数误差的效果。如果遍历完序列都没有出现这种情况，则表示我方可以消灭全部怪物。

### LeetCode-122 买卖股票的最佳时机Ⅱ

1.题目的全局最优解是：最大化的利润。
2.把总问题分解为子问题：每次只考虑今天买明天卖能不能获利，只要能获利，那我今天就买，不考虑以后的事。
3.子问题最优解合成全局最优解：每个子问题获得的利润加起来就是最大化的利润

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int res = 0;
        for(int i = 0;i<prices.size()-1;i++)
        {
            if(prices[i+1]-prices[i]>0) res+=prices[i+1]-prices[i];
        }
        return res;
    }
};
```



## 动态规划DP

### 数字三角形模型

#### AcWing-898 数字三角形

**思路：数字三角形模板**

**属性：max**

**边界条件：当j = 0时不选也是一种方案**

**集合：从(i,j)到最后一层的所有方案**

**状态计算：**![image-20230721203954090](https://raw.githubusercontent.com/lizijia777/imageSet/main/note1/202307212039124.png)

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

![image-20230721204022117](https://raw.githubusercontent.com/lizijia777/imageSet/main/note1/202307212040150.png)

*则我们可以对交叉出来的部分进行路线交换，如下图的操作:*

![image-20230721204059810](https://raw.githubusercontent.com/lizijia777/imageSet/main/note1/202307212040844.png)

*于是，我们可以发现，所有的交叉路线都会映射成一种一条路线只在下方走，一条路线只在上方走的不交叉路线*

*因此我们只需集中解决情况二即可*

*情况二：最优解的两条路线不交叉，但在某些点有重合。*

![image-20230721204126017](https://raw.githubusercontent.com/lizijia777/imageSet/main/note1/202307212041051.png)

*由于方格取数，对于走到相同格子时，只会累加一次格子的价值*

*于是我们可以使用贪心中的微调法来进行这部分的证明*

*对于重合的格子，我们必然可以在两条路线中找到额外的一条或两条路线，使得新的路线不发生重合*

*具体参照下图：![](https://raw.githubusercontent.com/lizijia777/imageSet/main/note1/202307212041051.png)

*由于原路线是最优解，则必然 wA=wB=0*

*，否则最优解路线必然是经过 A 或 B 的因此，我们可以通过微调其中的一条路线，使之不经过重合点 C，同时路线的总价值没有减少。*

*得证：最优解路线可以是不经过重复路线的 （部分证明方法参照了第一赞的题解了）*

**属性：max**

**边界条件：![image-20230713163004846](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20230713163004846.png)**

![](https://raw.githubusercontent.com/lizijia777/imageSet/main/note1/202307212042700.png)

**状态计算：**![image-20230721204353187](https://raw.githubusercontent.com/lizijia777/imageSet/main/note1/202307212043221.png)

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

### 树形dp

#### LeetCode-337 打家劫舍Ⅲ（结构体树模板题）

```

class Solution {
public:
    unordered_map <TreeNode*, int> f, g;
    void dfs(TreeNode* root)
    {
      
        if (root == NULL) return ;
        f[root] = root->val;
        dfs(root->left);
	    dfs(root->right);
        g[root] += max(f[root->left],g[root->left]) + max(f[root->right],g[root->right]);
        f[root] += g[root->left]+g[root->right];
    }
    int rob(TreeNode* root) {
        dfs(root);
        return max(f[root],g[root]);
    }
};
```



#### AcWing-285 没有上司的舞会（构造数组树模板题）

![没有上司的舞会，DP状态分析.jpg](https://cdn.acwing.com/media/article/image/2022/03/26/98857_bd915ef2ad-%E6%B2%A1%E6%9C%89%E4%B8%8A%E5%8F%B8%E7%9A%84%E8%88%9E%E4%BC%9A%EF%BC%8CDP%E7%8A%B6%E6%80%81%E5%88%86%E6%9E%90.jpg)

```
#include<iostream>
#include<cstring>
#include<algorithm>
using namespace std;
const int N = 20000;
int h[N],ne[N],e[N],idx;
int hasFather[N],happy[N];
int f[N][2];
void add(int a,int b)
{
    e[idx] = b;
    ne[idx]= h[a];
    h[a] = idx++;
}
void dfs(int root)
{
    if(root==0) return ;
    f[root][1] = happy[root];
    for(int i = h[root];i!=-1;i = ne[i])
    {
        auto t =e[i];
        dfs(t);
        f[root][1] += f[t][0];
        f[root][0] += max(f[t][1],f[t][0]);
    }
}
int main()
{
    int n;
    cin>>n;
    memset(h,-1,sizeof h); 
    for(int i = 1 ;i<=n;i++)
    {
        cin>>happy[i];
    }
    for(int i = 0 ;i<n-1;i++)
    {
        int a,b;
        cin>>a>>b;
        add(b,a);
        hasFather[a] = b;
    }
    int root = 1; 
    while(hasFather[root]) root ++; 
    dfs(root);
    cout<<max(f[root][1],f[root][0]);
}

```



### 状态机 DP

#### AcWing-1055 股票买卖II

**思路：**

**属性:max**

**边界条件：第0天的时候不可能持股；**

**集合：考虑前i天的股市，第i天手中持股状态为j（j=0,未持股；j=1,持股）的方案**

**状态计算**：2![image-20230721204237204](https://raw.githubusercontent.com/lizijia777/imageSet/main/note1/202307212042250.png)

#### AcWing-1911 最大子序列交替和

与股票买卖类似

### 最长上升子序列模型

#### AcWing-895 最长上升子序列

**思路：**

**属性:max**

**边界条件：**因为每一个数以自己结尾的最长上升子序列的长度为 1 ，所以初始 f[i]=1

**集合：** 表示从第一个数开始算，以第 i个数结尾的最长上升子序列

**状态计算：**![image-20230721204300671](https://raw.githubusercontent.com/lizijia777/imageSet/main/note1/202307212043702.png)

#### AcWing-482 合唱队形

**思路：**![image-20230721204318512](https://raw.githubusercontent.com/lizijia777/imageSet/main/note1/202307212043546.png)

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

## 离线查询

### LeetCode-1851 包含每个查询的最小区间

自定义排序：

那么我们可以对intervals按左端点 *left* 从小到大进行排序，qindex为正常排序

    sort(qindex.begin(), qindex.end(), [&](int i, int j) -> bool {
        return queries[i] < queries[j];
    });
    sort(intervals.begin(), intervals.end(), [](const vector<int> &it1, const vector<int> &it2) -> bool {
            return it1[0] < it2[0];
    });

### 莫队算法（区间查询）

![image-20230718110334440](https://raw.githubusercontent.com/lizijia777/imageSet/main/note1/202307181103509.png)

![image-20230718110454915](https://raw.githubusercontent.com/lizijia777/imageSet/main/note1/202307181104951.png)

## 单调栈

单调栈主要回答这样的几种问题

- 比当前元素更大的下一个元素
- 比当前元素更大的前一个元素
- 比当前元素更小的下一个元素
- 比当前元素更小的前一个元素

### LeetCode-038 每日温度

模板题

## 并查集

并查集的主要作用是求连通分支数（如果一个图中所有点都存在可达关系（直接或间接相连），则此图的连通分支数为1；如果此图有两大子图各自全部可达，则此图的连通分支数为2……）

并查集的主要用途有以下两点：
1、维护无向图的连通性（判断两个点是否在同一连通块内，或增加一条边后是否会产生环）；
2、用在求解最小生成树的[Kruskal算法](https://blog.csdn.net/the_ZED/article/details/105938353)里。

**初始化**

```
#define MAXN 100
int Parent[MAXN];

void init(int n){
    for(int i=0;i<n;i++){
        Parent[i]=i;        //存放每个结点的结点（或双亲结点）
    }
}
```

**查找**

```
//查询根结点
int find(int x){
    if(Parent[x]==x)
        return x;
    else
        return find(Parent[x]);
}
```

**合并**

```
//合并,把 j 合并到 i 中去，就是把j的双亲结点设为i
void merge(int i,int j){
    Parent[find(j)] = find(i);
}
```

**路径压缩**

```
#include <iostream>
using namespace std;

#define MAXN 100
int Parent[MAXN];

//初始化，现在各自为王，自己就是一个集合
void init(int n){
    for(int i=0;i<n;i++){
        Parent[i]=i;
    }
}

//查询根结点
int find(int x){
    if(Parent[x]==x)
        return x;
    else{
            Parent[x]=find(Parent[x]);   //顺便把双亲结点也设置为根结点，路径压缩
            return Parent[x];
    }
}

//合并,把 j 合并到 i 中去，就是把j的双亲结点设为i
void merge(int i,int j){
    Parent[find(j)] = find(i);
}

int main()
{
    return 0;
}
```

**按秩合并**

![img](https://img-blog.csdnimg.cn/20210113130834895.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODI3OTEwMQ==,size_16,color_FFFFFF,t_70)

```
//初始化
void init(int n){
    for(int i=0;i<n;i++){
        Parent[i]=i;
        Rank[i]=1;
    }
}
//合并代码
void merge(int i,int j){
    int x = find(i),y = find(j); //分别找到结点i和结点j的根节点
    if(Rank[x] < Rank[y]){       //如果以x作为根结点的子树深度小于以y作为根结点的子树的深度，则把x合并到y中
        Parent[x]=y;
    }
    else{
        Parent[y]=x;
    }
    if(Rank[x] == Rank[y] && x != y){  //如果深度相同且根结点不同，以x为根结点的子树深度+1，因为是需要合并到x
        Rank[x]++;
    }
}

```

**模板代码**

```
//并查集类
class DisJointSetUnion
{
private:
    // 所有根结点相同的结点位于同一个集合中
    vector<int> parent;    // 双亲结点数组，记录该结点的双亲结点，用于查找该结点的根结点
    vector<int> rank;      // 秩数组，记录以该结点为根结点的树的深度，主要用于优化，在合并两个集合的时候，rank大的集合合并rank小的集合

public:
    DisJointSetUnion(int n) （按秩序合并）         //构造函数
    {
        for (int i = 0; i < n; i++)
        {
            parent.push_back(i);      //此时各自为王，自己就是一个集合
            rank.push_back(1);        //rank=1，此时每个结点自己就是一颗深度为1的树
        }
    }

    //查找根结点（路径压缩）
    int find(int x)
    {
        if(x==parent[x])
            return x;
        else
        {
            parent[x] = find(parent[x]);   // 路径压缩， 遍历过程中的所有双亲结点直接指向根结点，减少后续查找次数
            return parent[x];
        }
    }

    void merge(int x,int y)（按秩序合并） 
    {
        int rx = find(x);                    //查找x的根结点，即x所在集合的代表元素
        int ry = find(y);

        if (rx != ry)                           //如果不是同一个集合
        {
            if (rank[rx] < rank[ry])     //rank大的集合合并rank小的集合
            {
                swap(rx, ry);               //这里进行交换是为了保证rx的rank大于ry的rank，方便下面合并
            }
             parent[ry] = rx;              //rx 合并 ry
            if (rank[rx] == rank[ry])
                rank[rx] += 1;
        }
    }
};
```

### AcWing-836 合并集合

模板题

### AcWing-837 连通块中点的数量

**询问点a所在连通块中点的数量**

```
vector<int>Size;
void merge(int a,int b)
{
    int x = find(a),y = find(b); 
    Size[y] += Size[x]; //一定是父节点相加
    if(Rank[x]>Rank[y]){
        parent[y] =  x;
    }
    else 
    {
         parent[x] = y;
      
    }
    if(Rank[x] == Rank[y] && x != y){  
        Rank[x]++;
    }
}
```

### 华为0913秋招笔试 互通设备集

查询所有连通大块的个数

```
 for (int i = 0; i < m; i++) {
        if (parent[i] == i) {
            ans++;
        }
    }
 cout << ans << endl;
```

