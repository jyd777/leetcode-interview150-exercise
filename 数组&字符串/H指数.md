# H指数

给你一个整数数组 `citations` ，其中 `citations[i]` 表示研究者的第 `i` 篇论文被引用的次数。计算并返回该研究者的 **`h` 指数**。

根据维基百科上 [h 指数的定义](https://baike.baidu.com/item/h-index/3991452?fr=aladdin)：`h` 代表“高引用次数” ，一名科研人员的 `h` **指数** 是指他（她）至少发表了 `h` 篇论文，并且 **至少** 有 `h` 篇论文被引用次数大于等于 `h` 。如果 `h` 有多种可能的值，**`h` 指数** 是其中最大的那个。

 

**示例 1：**

```
输入：citations = [3,0,6,1,5]
输出：3 
解释：给定数组表示研究者总共有 5 篇论文，每篇论文相应的被引用了 3, 0, 6, 1, 5 次。
     由于研究者有 3 篇论文每篇 至少 被引用了 3 次，其余两篇论文每篇被引用 不多于 3 次，所以她的 h 指数是 3。
```

**示例 2：**

```
输入：citations = [1,3,1]
输出：1
```

 

**提示：**

- `n == citations.length`
- `1 <= n <= 5000`
- `0 <= citations[i] <= 1000`

## 解法

### 方法一：排序 + 遍历

**思路：**
1. 将citations数组从大到小排序
2. 遍历每个位置i，检查是否至少有i+1篇论文被引用≥i+1次
3. 找到满足条件的最大h值

**代码：**

```cpp
class Solution {
public:
    int hIndex(vector<int>& citations) {
        int num = citations.size();// 发了多少篇论文
        // h指数为[0, num]
        sort(citations.begin(), citations.end(), greater<int>());
        int h = 0;
        for(int i=0;i<num;i++)
            if(i+1 <= citations[i])
                h = i+1;
            else
                break;
        return h;
    }
};
```

**执行过程示例：**

对于 `citations = [3,0,6,1,5]`：

```
排序后: [6,5,3,1,0]

i=0: citations[0]=6 >= 1, h=1 (至少1篇论文引用≥1次)
i=1: citations[1]=5 >= 2, h=2 (至少2篇论文引用≥2次)  
i=2: citations[2]=3 >= 3, h=3 (至少3篇论文引用≥3次)
i=3: citations[3]=1 < 4, 停止

返回: h=3
```

### 方法二：计数法（更优）

**思路：**
- 用计数数组统计每个引用次数的论文数量
- 从大到小遍历可能的h值

**代码：**

```cpp
class Solution {
public:
    int hIndex(vector<int>& citations) {
        int n = citations.size();// 发了多少篇论文
        // 引用次数统计（引用次数为c的个数）
        vector<int> count(n + 1, 0); 
        for(int c :citations)
            count[min(c,n)]++;
        int h = 0;
        // 从最大值开始遍历
        for(int i=n;i>=0;i--){
            h += count[i];// 累加引用次数为i的论文数量
            if(h >= i)// 如果至少有h篇论文引用次数大于等于i次，则h指数为i
                return i;
        }
        return 0;
    }
};
```

**时间复杂度：**
- 方法一：O(n log n) 
- 方法二：O(n)

**空间复杂度：**
- 方法一：O(1)
- 方法二：O(n)